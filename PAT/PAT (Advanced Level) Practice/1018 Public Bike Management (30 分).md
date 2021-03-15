# 题目详情
There is a public bike service in Hangzhou City which provides great convenience to the tourists from all over the world. One may rent a bike at any station and return it to any other stations in the city.

The ==Public Bike Management Center (PBMC) keeps monitoring the real-time capacity of all the stations==. ==A station is said to be in **perfect** condition if it is exactly half-full==. ==If a station is full or empty, PBMC will collect or send bikes to adjust the condition of that station to perfect. And more, all the stations on the way will be adjusted as well.==

==When a problem station is reported, PBMC will always choose the shortest path to reach that station.== ==If there are more than one shortest path, the one that requires the least number of bikes sent from PBMC will be chosen.==

![](https://images.ptausercontent.com/213)

The above figure illustrates an example. The stations are represented by vertices and the roads correspond to the edges. ==The number on an edge is the time taken to reach one end station from another. The number written inside a vertex $S$ is the current number of bikes stored at $S$==. Given that ==the maximum capacity of each station is 10==. To solve the problem at $S_3$, we have 2 different shortest paths:

1.  PBMC -> $S_1$ -> $S_3$. In this case, 4 bikes must be sent from PBMC, because we can collect 1 bike from $S_1$ and then take 5 bikes to $S_3$, so that both stations will be in perfect conditions.
    
2.  PBMC -> $S_2$ -> $S_3$. This path requires the same time as path 1, but only 3 bikes sent from PBMC and hence is the one that will be chosen.
    

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 numbers: ==$C_{max}$ ($\le 100$), always an even number, is the maximum capacity of each station;== ==$N$ ($\le 500$), the total number of stations;== ==$S_p$, the index of the problem station (the stations are numbered from 1 to $N$, and PBMC is represented by the vertex 0);== and ==$M$, the number of roads.== The second line contains ==$N$ non-negative numbers $C_i$ ($i=1,\cdots ,N$) where each $C_i$ is the current number of bikes at $S_i$ respectively==. Then $M$ lines follow, each contains 3 numbers: ==$S_i$, $S_j$, and $T_{ij}$ which describe the time $T_{ij}$ taken to move betwen stations $S_i$ and $S_j$.== All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print your results in one line. ==First output the number of bikes that PBMC must send.== Then after one space, ==output the path in the format: $0->S_1->\cdots ->S_p$.== Finally after another space, ==output the number of bikes that we must take back to PBMC after the condition of $S_p$ is adjusted to perfect==.

Note that ==if such a path is not unique, output the one that requires minimum number of bikes that we must take back to PBMC==. The judge's data guarantee that such a path is unique.

### Sample Input:

    10 3 3 5
    6 7 0
    0 1 1
    0 2 1
    0 3 3
    1 3 1
    2 3 1


### Sample Output:

    3 0->2->3 0

# 题解

计算从节点0开始到某个节点的路径，第一标尺是路径最短，第二标尺是出发时携带的自行车数量最少，第三标尺是带回节点0的自行车数量最少。注意只有去的时候能够调整每个节点的自行车的数量，带回的时候是不调整的。



要得到最短路径可以使用dijkstra算法，但是对于后两个标尺无法在dijkstra算法中判断，因为这两个问题不满足最优子结构，只能先求出所有的最短路径，然后选择符合题意的路径。

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

#define MAX_V 505
#define INF 0x3fffffff

struct node {
    int v, weight;

    bool operator>(const node& e) const {
        return weight > e.weight;
    }
};

bool visit[MAX_V];  // 访问标志
vector<int> pre[MAX_V];  // 前驱
vector<node> graph[MAX_V];  // 邻接表
int dist[MAX_V], bikeCnt[MAX_V];  // 从起点到每个点的最短距离 每个车站的车的数量

void dijkstra(int start) {
    fill_n(dist, MAX_V, INF);

    dist[start] = 0;
    pre[start].push_back(INF);

    priority_queue<node, vector<node>, greater<node>> pq;
    pq.push({ start, 0 });

    while (not pq.empty()) {
        node top = pq.top();
        pq.pop();

        int u = top.v, uDist = top.weight;

        visit[u] = true;

        if (dist[u] < uDist) continue;

        for (auto& e : graph[u]) {
            int v = e.v, uvWeight = e.weight;

            if (not visit[v] and (dist[v] == INF or dist[v] > dist[u] + uvWeight)) {
                dist[v] = dist[u] + uvWeight;
                pq.push({ v, dist[v] });

                pre[v].clear();
                pre[v].push_back(u);
            }
            else if (dist[v] == dist[u] + uvWeight) {
                pre[v].push_back(u);
            }
        }
    }
}

int perfectNum;  // 完美条件的数量
vector<int> path, tempPath;
int minSend = INF, minTakeBack = INF;  // 需要从节点0发出的车的数量 需要带回节点0的车的数量

void dfs(int end) {
    if (end == INF) {  // 深搜到了一条完整的路径，开始计算花费
        int send = 0, takeBack = 0;
        for (int i = tempPath.size() - 2; i >= 0; --i) {  // 从经过的第一个**车站**开始到终点
            int p = tempPath[i];

            if (bikeCnt[p] > perfectNum) {
                takeBack += bikeCnt[p] - perfectNum;
            }
            else if (bikeCnt[p] < perfectNum) {
                if (takeBack >= perfectNum - bikeCnt[p])
                    takeBack -= perfectNum - bikeCnt[p];
                else {
                    send += perfectNum - bikeCnt[p] - takeBack;
                    takeBack = 0;
                }
            }
        }

        if (send < minSend or (send == minSend and takeBack < minTakeBack)) {
            path = tempPath;
            reverse(path.begin(), path.end());

            minSend = send;
            minTakeBack = takeBack;
        }

        return;
    }

    tempPath.push_back(end);
    for (auto e : pre[end]) dfs(e);
    tempPath.pop_back();
}

int main() {
    int c_max, n, sp, m;  // 最大容量 站台的数量 需要处理的车站的编号 路的数量
    cin >> c_max >> n >> sp >> m;
    perfectNum = c_max / 2;

    for (int i = 1; i <= n; ++i)
        cin >> bikeCnt[i];

    for (int i = 0; i < m; ++i) {
        int si, sj, t;
        cin >> si >> sj >> t;

        graph[si].push_back({ sj, t });
        graph[sj].push_back({ si, t });
    }

    dijkstra(0);
    dfs(sp);

    cout << minSend << " ";
    for (auto i = path.begin(); i != path.end(); ++i) {
        cout << *i;
        if (i + 1 != path.end()) cout << "->";
    }
    cout << " " << minTakeBack;
}
```

