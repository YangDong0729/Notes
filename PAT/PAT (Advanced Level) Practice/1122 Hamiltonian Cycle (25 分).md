# 题目详情
==The "Hamilton cycle problem" is to find a simple cycle that contains every vertex in a graph. Such a cycle is called a "Hamiltonian cycle".==

In this problem, you are supposed to ==tell if a given cycle is a Hamiltonian cycle.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==2 positive integers $N$ ($2< N \le 200$), the number of vertices, and $M$, the number of edges in an undirected graph.== Then $M$ lines follow, each describes an edge in the format `Vertex1 Vertex2`, where the vertices are numbered from 1 to $N$. The next line gives a positive integer $K$ which is the number of queries, followed by $K$ lines of queries, each in the format:

$n$ $V_1$ $V_2 ... V_n$

where $n$ is the number of vertices in the list, and $V_i'$s are the vertices on a path.

### Output Specification:

For each query, print in a line `YES` if the path does form a Hamiltonian cycle, or `NO` if not.

### Sample Input:

    6 10
    6 2
    3 4
    1 5
    2 5
    3 1
    4 1
    1 6
    6 3
    1 2
    4 5
    6
    7 5 1 4 3 6 2 5
    6 5 1 4 3 6 2
    9 6 2 1 6 3 4 5 2 6
    4 1 2 5 1
    7 6 1 3 4 5 2 6
    7 6 1 2 5 4 3 1


### Sample Output:

    YES
    NO
    NO
    NO
    YES
    NO
# 题解

```cpp
#include <iostream>
#include <set>

using namespace std;

int cir[205];
set<int> graph[205];

int main() {
    int n, m, a, b, k;
    cin >> n >> m;

    for (int i = 0; i < m; ++i) {
        cin >> a >> b;
        graph[a].insert(b);
        graph[b].insert(a);
    }

    cin >> k;
    for (int i = 0; i < k; ++i) {
        int p;
        bool vis[205] = {}, hc;

        cin >> p;
        for (int j = 0; j < p; ++j)
            cin >> cir[j];
        hc = false;
        if (p == n + 1) {
            hc = true;
            if (cir[0] != cir[p - 1]) hc = false;
            for (int j = 0; j < p - 1; ++j) {
                if (graph[cir[j]].count(cir[j + 1]) == 0)
                    hc = false;
                if (vis[cir[j]]) hc = false;
                else vis[cir[j]] = true;
                if (!hc) break;
            }
        }
        cout << (hc ? "YES\n" : "NO\n");
    }
}
```

