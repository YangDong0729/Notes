# 题目详情
==Indeed there are many different tourist routes from our city to Rome. You are supposed to find your clients the route with the least cost while gaining the most happiness.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==2 positive integers $N$ ($2\le N\le 200$), the number of cities, and $K$, the total number of routes between pairs of cities;== followed by the name of the starting city. The next $N-1$ lines each gives the name of a city and an integer that represents the happiness one can gain from that city, except the starting city. Then $K$ lines follow, each describes a route between two cities in the format `City1 City2 Cost`. Here the name of a city is a string of 3 capital English letters, and the destination is always `ROM` which represents Rome.

### Output Specification:

==For each test case, we are supposed to find the route with the least cost. If such a route is not unique, the one with the maximum happiness will be recommanded. If such a route is still not unique, then we output the one with the maximum average happiness== -- it is guaranteed by the judge that such a solution exists and is unique.

Hence in the first line of output, you must print 4 numbers: ==the number of different routes with the least cost, the cost, the happiness, and the average happiness (take the integer part only) of the recommanded route. Then in the next line, you are supposed to print the route in the format `City1->City2->...->ROM`.==

### Sample Input:

    6 7 HZH
    ROM 100
    PKN 40
    GDN 55
    PRS 95
    BLN 80
    ROM GDN 1
    BLN ROM 1
    HZH PKN 1
    PRS ROM 2
    BLN HZH 2
    PKN GDN 1
    HZH PRS 1


### Sample Output:

    3 3 195 97
    HZH->PRS->ROM
# 题解

给出一个图，带有点权和边权，求最短路径。第一标尺是边权之和尽量小，第二标尺是点权之和尽量大，第三标尺是平均点权尽量大。源点点权不计算。



第一标尺只需运行一次最短路径算法。
第三标尺破坏了最短路径问题的最优子结构（因为这个值有可能会随着路径的增长而变小），所以只能在求最短路径时构建最短路径树，然后使用深搜来枚举所有最短路径。
第二标尺虽然也可以在计算最短路径时考虑，但是基于这道题的特点，也放在深搜中了。

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>
#include <map>
#include <unordered_map>
#include <string>

using namespace std;

struct Node {
    bool visit;                     // 访问标志
    vector<string> pre;             // 最短路径上的前驱
    vector<pair<int, string>> adj;  // 邻接表
    int dist = -1, happiness, pathCnt;   // 最短路径估计 快乐值 最短路径个数
};

unordered_map<string, Node> node;

void dijkstra(string start) {
    node[start].dist = 0;
    node[start].pathCnt = 1;

    using pair = pair<int, string>;
    priority_queue<pair, vector<pair>, greater<pair>> pq;
    pq.push({ 0, start });

    while (not pq.empty()) {
        int u_dist = pq.top().first;
        string u = pq.top().second;
        pq.pop();

        node[u].visit = true;

        if (node[u].dist < u_dist) continue;
        for (auto& e : node[u].adj) {
            string v = e.second;
            int uv_cost = e.first;

            if (not node[v].visit and (node[v].dist == -1 or node[v].dist > node[u].dist + uv_cost)) {
                node[v].dist = node[u].dist + uv_cost;
                pq.push({ node[v].dist, v });

                node[v].pre.clear();
                node[v].pre.push_back(u);

                node[v].pathCnt = node[u].pathCnt;
            }
            else if (node[v].dist == node[u].dist + uv_cost) {
                node[v].pre.push_back(u);
                node[v].pathCnt += node[u].pathCnt;
            }
        }
    }
}

int max_sum = 0;
double max_avg = 0;
vector<string> temp_path, path;

void dfs(string name, int sum_happiness) {
    sum_happiness += node[name].happiness;
    temp_path.push_back(name);
    if (node[name].pre.empty()) {  // 深搜到了路径的源点
        double avgHappiness = double(sum_happiness) / (int(temp_path.size()) - 1);
        if (sum_happiness > max_sum
            or (sum_happiness == max_sum and avgHappiness > max_avg)) {
            max_sum = sum_happiness;
            max_avg = avgHappiness;

            path = temp_path;
            reverse(path.begin(), path.end());
        }
    }
    else {
        for (auto e : node[name].pre)
            dfs(e, sum_happiness);
    }
    temp_path.pop_back();
}

int main() {
    int n, k;
    string start, end = "ROM";
    cin >> n >> k >> start;

    // 读取每个城市的快乐值
    for (int i = 0; i < n - 1; ++i) {
        int happiness;
        string name;
        cin >> name >> happiness;
        
        node[name].happiness = happiness;
    }

    // 读取所有的道路
    for (int i = 0; i < k; ++i) {
        int cost;
        string c1, c2;

        cin >> c1 >> c2 >> cost;

        node[c1].adj.push_back({ cost, c2 });
        node[c2].adj.push_back({ cost, c1 });
    }

    dijkstra(start);   // dijkstra找出所有最短路径
    dfs(end, 0);       // dfs选出快乐值最大且平均快乐值尽可能大的最短路径

    cout << node["ROM"].pathCnt << " " << node["ROM"].dist << " "
        << max_sum << " " << int(max_avg) << "\n";

    for (auto iter = path.begin(); iter != path.end(); ++iter) {
        cout << *iter;
        if (iter + 1 != path.end()) cout << "->";
    }
}
```

