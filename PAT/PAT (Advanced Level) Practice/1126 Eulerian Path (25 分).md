# 题目详情
In graph theory, ==an Eulerian path is a path in a graph which visits every edge exactly once==. Similarly, ==an Eulerian circuit is an Eulerian path which starts and ends on the same vertex==. They were first discussed by Leonhard Euler while solving the famous Seven Bridges of Konigsberg problem in 1736. It has been proven that ==connected graphs with all vertices of even degree have an Eulerian circuit==, and ==such graphs are called **Eulerian**==. ==If there are exactly two vertices of odd degree, all Eulerian paths start at one of them and end at the other. A graph that has an Eulerian path but not an Eulerian circuit is called **semi-Eulerian=**==. (Cited from [https://en.wikipedia.org/wiki/Eulerian\_path](https://en.wikipedia.org/wiki/Eulerian_path))

==Given an undirected graph, you are supposed to tell if it is Eulerian, semi-Eulerian, or non-Eulerian.==

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 2 numbers N ($\le 500$), and M, which are the total number of vertices, and the number of edges, respectively. Then M lines follow, each describes an edge by giving the two ends of the edge (the vertices are numbered from 1 to N).

### Output Specification:

For each test case, ==first print in a line the degrees of the vertices in ascending order of their indices==. ==Then in the next line print your conclusion about the graph== -- either `Eulerian`, `Semi-Eulerian`, or `Non-Eulerian`. Note that all the numbers in the first line must be separated by exactly 1 space, and there must be no extra space at the beginning or the end of the line.

### Sample Input 1:

    7 12
    5 7
    1 2
    1 3
    2 3
    2 4
    3 4
    5 2
    7 6
    6 3
    4 5
    6 4
    5 6


### Sample Output 1:

    2 4 4 4 4 4 2
    Eulerian


### Sample Input 2:

    6 10
    1 2
    1 3
    2 3
    2 4
    3 4
    5 2
    6 3
    4 5
    6 4
    5 6


### Sample Output 2:

    2 4 4 4 3 3
    Semi-Eulerian


### Sample Input 3:

    5 8
    1 2
    2 5
    5 4
    4 1
    1 3
    3 2
    3 4
    5 3


### Sample Output 3:

    3 3 4 3 3
    Non-Eulerian
# 题解

用dfs判断图是否连通；统计度为奇数的点的个数。

```cpp
#include <iostream>
#include <vector>

using namespace std;

bool vis[505];
vector<int> g[505];

void dfs(int x) {
    vis[x] = true;
    for (auto& e : g[x])
        if (not vis[e])
            dfs(e);
}

int main() {
    int n, m, x, y;
    cin >> n >> m;

    for (int i = 0; i < m; ++i) {
        cin >> x >> y;
        g[x].push_back(y);
        g[y].push_back(x);
    }

    int odd = 0;
    for (int i = 1; i <= n; ++i) {
        cout << g[i].size() << (i == n ? "\n" : " ");
        if (g[i].size() % 2) ++odd;
    }

    dfs(1);
    for (int i = 1; i <= n; ++i)
        if (not vis[i]) {
            cout << "Non-Eulerian";
            return 0;
        }

    if (odd == 2) cout << "Semi-Eulerian";
    else if (odd == 0) cout << "Eulerian";
    else cout << "Non-Eulerian";
}
```

