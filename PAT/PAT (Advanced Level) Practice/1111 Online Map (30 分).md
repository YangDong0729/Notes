# 题目详情
Input our current position and a destination, an online map can recommend several paths. ==Now your job is to recommend two paths to your user: one is the shortest, and the other is the fastest. It is guaranteed that a path exists for any request.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers $N$ ($2 \le N \le 500$), and $M$, being the total number of streets intersections on a map, and the number of streets==, respectively. Then $M$ lines follow, each describes a street in the format:

    V1 V2 one-way length time


where `V1` and `V2` are the indices (from 0 to $N-1$) of the two ends of the street; `one-way` is 1 if the street is one-way from `V1` to `V2`, or 0 if not; `length` is the length of the street; and `time` is the time taken to pass the street.

Finally a pair of source and destination is given.

### Output Specification:

For each case, first print the shortest path from the source to the destination with distance `D` in the format:

    Distance = D: source -> v1 -> ... -> destination


Then in the next line print the fastest path with total time `T`:

    Time = T: source -> w1 -> ... -> destination

==In case the shortest path is not unique, output the fastest one among the shortest paths, which is guaranteed to be unique. In case the fastest path is not unique, output the one that passes through the fewest intersections, which is guaranteed to be unique.==

==In case the shortest and the fastest paths are identical, print them in one line in the format:==

    Distance = D; Time = T: source -> u1 -> ... -> destination


### Sample Input 1:

    10 15
    0 1 0 1 1
    8 0 0 1 1
    4 8 1 1 1
    3 4 0 3 2
    3 9 1 4 1
    0 6 0 1 1
    7 5 1 2 1
    8 5 1 2 1
    2 3 0 2 2
    2 1 1 1 1
    1 3 0 3 1
    1 4 0 1 1
    9 7 1 3 1
    5 1 0 5 2
    6 5 1 1 2
    3 5


### Sample Output 1:

    Distance = 6: 3 -> 4 -> 8 -> 5
    Time = 3: 3 -> 1 -> 5


### Sample Input 2:

    7 9
    0 4 1 1 1
    1 6 1 1 3
    2 6 1 1 1
    2 5 1 2 2
    3 0 0 1 1
    3 1 1 1 3
    3 2 1 1 2
    4 5 0 2 2
    6 5 1 1 2
    3 5


### Sample Output 2:

    Distance = 3; Time = 4: 3 -> 2 -> 5

# 题解

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

#define INF -1
#define MAX_V 505

struct Node {
    int dist;                             // 与源点的距离
    bool visit;                           // 访问
    vector<int> pre;                      // 前驱
    vector<pair<int, array<int, 2>>> adj; // 邻接的节点 {距离, 时间}
} graph[MAX_V];                           // 邻接表

int flag = 0;
array<int, 2> weight[MAX_V][MAX_V]; // 邻接矩阵表示的 {距离, 时间}

void dijkstra(int start) {
    for (int i = 0; i < MAX_V; ++i) {
        graph[i].dist = INF;
        graph[i].pre.clear();
        graph[i].visit = false;
    }

    using pair = pair<int, int>;
    priority_queue<pair, vector<pair>, greater<pair>> pq;

    graph[start].dist = 0;
    pq.push({0, start});

    while (not pq.empty()) {
        int uIdx = pq.top().second;
        int uDist = pq.top().first;
        auto &u = graph[uIdx];
        pq.pop();

        u.visit = true;

        if (u.dist < uDist)
            continue;

        for (auto &e : u.adj) {
            auto &v = graph[e.first];
            int uvWeight = e.second[flag];

            if (not v.visit and (v.dist == INF or v.dist > u.dist + uvWeight)) {
                v.dist = u.dist + uvWeight;
                pq.push({v.dist, e.first});

                v.pre.clear();
                v.pre.push_back(uIdx);
            } else if (v.dist == u.dist + uvWeight)
                v.pre.push_back(uIdx);
        }
    }
}

int minSecondStandard = INF;
vector<int> shortestPath[2], tmpPath;

void dfs(int end, int secondStandard) {
    tmpPath.push_back(end);
    if (graph[end].pre.empty() and (minSecondStandard == INF or minSecondStandard > secondStandard)) {
        minSecondStandard = secondStandard;
        shortestPath[flag] = tmpPath;
    }
    for (auto e : graph[end].pre)
        dfs(e, secondStandard + (flag == 0 ? weight[e][end][1] : 1));
    tmpPath.pop_back();
}

void printPath(vector<int> &path) {
    for (auto i = path.rbegin(); i != path.rend(); ++i) {
        cout << *i;
        if (i + 1 != path.rend())
            cout << " -> ";
        else
            cout << "\n";
    }
}

int main() {
    int n, m;
    cin >> n >> m;

    for (int i = 0; i < m; ++i) {
        int v1, v2, one;
        array<int, 2> value;

        cin >> v1 >> v2 >> one >> value[0] >> value[1];

        graph[v1].adj.emplace_back(v2, value);
        weight[v1][v2] = value;
        if (not one) {
            graph[v2].adj.emplace_back(v1, value);
            weight[v2][v1] = value;
        }
    }

    int source, destination;
    cin >> source >> destination;

    array<int, 2> shortest;

    for (flag = 0; flag < 2; ++flag) {
        dijkstra(source);
        minSecondStandard = INF;
        dfs(destination, 0);
        shortest[flag] = graph[destination].dist;
    }

    if (shortestPath[0] == shortestPath[1]) {
        cout << "Distance = " << shortest[0] << "; Time = " << shortest[1] << ": ";
        printPath(shortestPath[0]);
    } else {
        cout << "Distance = " << shortest[0] << ": ";
        printPath(shortestPath[0]);
        cout << "Time = " << shortest[1] << ": ";
        printPath(shortestPath[1]);
    }
}
```

