# 题目详情
==A **clique** is a subset of vertices of an undirected graph such that every two distinct vertices in the clique are adjacent. A **maximal clique** is a clique that cannot be extended by including one more adjacent vertex.== (Quoted from [https://en.wikipedia.org/wiki/Clique\_(graph\_theory](https://en.wikipedia.org/wiki/Clique_(graph_theory)))

==Now it is your job to judge if a given subset of vertices can form a maximal clique.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers Nv ($\le 200$), the number of vertices in the graph, and Ne, the number of undirected edges.== Then Ne lines follow, each gives a pair of vertices of an edge. The vertices are numbered from ==1 to Nv==.

After the graph, there is another positive integer M ($\le 100$). Then M lines of query follow, each first gives a positive number K ($\le$ Nv), then followed by a sequence of K distinct vertices. All the numbers in a line are separated by a space.

### Output Specification:

For each of the M queries, print in a line `Yes` if the given subset of vertices can form a maximal clique; or if it is a clique but not a **maximal clique**, print `Not Maximal`; or if it is not a clique at all, print `Not a Clique`.

### Sample Input:

    8 10
    5 6
    7 8
    6 4
    3 6
    4 5
    2 3
    8 2
    2 7
    5 3
    3 4
    6
    4 5 4 3 6
    3 2 8 7
    2 2 3
    1 1
    3 4 3 6
    3 3 2 1


### Sample Output:

    Yes
    Yes
    Yes
    Yes
    Not Maximal
    Not a Clique
# 题解

clique中所有的点两两之间都有边。先检查这个性质是否满足，如果满足，再检查是否能加入新的点，同时维持这个性质。

```cpp
#include <iostream>
#include <vector>

using namespace std;

int g[205][205];

int main() {
    int nv, ne, m, k;

    cin >> nv >> ne;
    for (int i = 0; i < ne; ++i) {
        int a, b;
        cin >> a >> b;
        g[a][b] = g[b][a] = 1;
    }

    cin >> m;
    for (int i = 0; i < m; ++i) {
        cin >> k;
        vector<int> seq(k);
        for (int j = 0; j < k; ++j)
            cin >> seq[j];

        bool clique = true;
        for (int p = 0; p < k and clique; ++p)
            for (int q = p + 1; q < k and clique; ++q)
                if (g[seq[p]][seq[q]] == 0)
                    clique = false;

        if (not clique) {
            cout << "Not a Clique\n";
            continue;
        }

        bool max_c = true;
        vector<bool> in(nv + 5);
        for (auto &e : seq) in[e] = true;
        for (int j = 1; j <= nv and max_c; ++j) {
            if (in[j]) continue;
            bool found = true;
            for (auto p = seq.begin(); p != seq.end() and found; ++p)
                if (g[*p][j] == 0)
                    found = false;
            if (found) max_c = false;
        }

        if (max_c) cout << "Yes\n";
        else cout << "Not Maximal\n";
    }
}
```

