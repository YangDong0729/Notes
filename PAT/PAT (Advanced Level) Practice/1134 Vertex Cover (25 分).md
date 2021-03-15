# 题目详情
==A **vertex cover** of a graph is a set of vertices such that each edge of the graph is incident to at least one vertex of the set.== Now given a graph with several vertex sets, you are supposed to tell if each of them is a vertex cover or not.

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers $N$ and $M$ (both no more than $10^4$), being the total numbers of vertices and the edges==, respectively. Then $M$ lines follow, each describes an edge by giving the indices (from 0 to $N-1$) of the two ends of the edge.

After the graph, ==a positive integer $K$ ($\le$ 100) is given, which is the number of queries.== Then $K$ lines of queries follow, each in the format:

$N_v$ $v[1]$ $v[2] \cdots v[N_v]$

where $N_v$ is the number of vertices in the set, and $v[i]$'s are the indices of the vertices.

### Output Specification:

For each query, print in a line `Yes` if the set is a vertex cover, or `No` if not.

### Sample Input:

    10 11
    8 7
    6 8
    4 5
    8 4
    8 1
    1 2
    1 4
    9 8
    9 1
    1 0
    2 4
    5
    4 0 3 8 4
    6 6 1 7 5 4 9
    3 1 8 4
    2 2 8
    7 9 8 7 6 5 4 2


### Sample Output:

    No
    Yes
    Yes
    No
    No
# 题解

输入时，存储每个结点都关联了哪些边。

```cpp
#include <iostream>
#include <vector>

using namespace std;

int visit[10005];
vector<int> graph[10005];

int main() {
    int n, m, k, l, a, b, cnt;

    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        cin >> a >> b;
        graph[a].push_back(i); // 与这个点关联的所有的边
        graph[b].push_back(i);
    }

    cin >> k;
    for (int i = 0; i < k; ++i) {
        cnt = 0;
        fill_n(visit, m, false);

        cin >> l;
        for (int j = 0; j < l; ++j) {
            cin >> a;
            for (auto &e : graph[a])
                if (!visit[e]) {
                    ++cnt;
                    visit[e] = true;
                }
        }

        cout << (cnt == m ? "Yes" : "No") << "\n";
    }
}
```

