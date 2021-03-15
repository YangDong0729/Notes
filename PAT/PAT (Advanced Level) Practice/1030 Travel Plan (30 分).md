# 题目详情
==A traveler's map gives the distances between cities along the highways, together with the cost of each highway.== Now you are supposed to write a program to help a traveler to decide ==the shortest path between his/her starting city and the destination. If such a shortest path is not unique, you are supposed to output the one with the minimum cost, which is guaranteed to be unique.==

### Input Specification:

Each input file contains one test case. Each case starts with a line containing ==4 positive integers $N$, $M$, $S$, and $D$, where $N$ ($\le 500$) is the number of cities (and hence the cities are numbered from 0 to $N−1$); $M$ is the number of highways; $S$ and $D$ are the starting and the destination cities==, respectively. Then $M$ lines follow, each provides the information of a highway, in the format:

    City1 City2 Distance Cost


where the numbers are all integers no more than 500, and are separated by a space.

### Output Specification:

For each test case, ==print in one line the cities along the shortest path from the starting point to the destination, followed by the total distance and the total cost of the path.== The numbers must be separated by a space and there must be no extra space at the end of output.

### Sample Input:

    4 5 0 3
    0 1 1 20
    1 3 2 30
    0 3 4 10
    0 2 2 20
    2 3 1 20


### Sample Output:

    0 2 3 3 40

# 题解

求起点到终点的最短路径，要求在路径最短的前提下花费最少。



这种题通用的方法是dijkstra求出所有的最短路径，然后通过dfs遍历每条最短路径，对每条最短路径计算其在第二标尺下的值。但是本题的花费的计算满足最优子结构，所以可以修改dijkstra算法，直接求出满足题意的路径。

# 代码

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

const int MAX_N = 505;
const int INF = -1;

struct edge {
    int to, weight, cost;

    bool operator>(const edge &e) const {
        return weight > e.weight;
    }
};

bool vis[MAX_N];
vector<edge> graph[MAX_N];
int dist[MAX_N], pre[MAX_N], cost[MAX_N];

void dijkstra(int s) {
    fill_n(dist, MAX_N, INF);

    dist[s] = 0;
    cost[s] = 0;
    pre[s] = INF;
    priority_queue<edge, vector<edge>, greater<edge>> pq;
    pq.push({s, 0, 0});

    while (not pq.empty()) {
        edge top = pq.top();
        pq.pop();

        int u = top.to, uDist = top.weight;

        vis[u] = true;

        if (dist[u] < uDist) continue;
        for (auto &e: graph[u]) {
            int v = e.to, uvWeight = e.weight, uvCost = e.cost;

            if (not vis[v] and (dist[v] == INF or dist[v] > dist[u] + uvWeight)) {
                pre[v] = u;

                dist[v] = dist[u] + uvWeight;
                cost[v] = cost[u] + uvCost;

                pq.push({v, dist[v], cost[v]});
            } else if (dist[v] == dist[u] + uvWeight and cost[v] > cost[u] + uvCost) {
                pre[v] = u;
                cost[v] = cost[u] + uvCost;
            }
        }
    }
}

vector<int> getPath(int d) {
    vector<int> path;
    while (d != INF) {
        path.push_back(d);
        d = pre[d];
    }
    reverse(path.begin(), path.end());
    return path;
}

int main() {
    int n, m, s, d;
    cin >> n >> m >> s >> d;
    for (int i = 0; i < m; ++i) {
        int c1, c2, weight, c;
        cin >> c1 >> c2 >> weight >> c;
        graph[c1].push_back({c2, weight, c});
        graph[c2].push_back({c1, weight, c});
    }

    dijkstra(s);

    auto path = getPath(d);
    for (auto e: path) cout << e << " ";
    cout << dist[d] << " " << cost[d];
}
```