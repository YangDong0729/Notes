# 题目详情
This is a problem given in the Graduate Entrance Exam in 2018: ==Which of the following is NOT a topological order obtained from the given directed graph? Now you are supposed to write a program to test each of the options.==

![gre.jpg](https://images.ptausercontent.com/5d35ed2a-4d19-4f13-bf3f-35ed59cebf05.jpg)

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers $N$ ($\le 1,000$), the number of vertices in the graph, and $M$ ($\le 10,000$), the number of directed edges.== Then $M$ lines follow, each gives the start and the end vertices of an edge. The vertices are numbered from 1 to $N$. After the graph, there is another positive integer $K$ ($\le 100$). Then $K$ lines of query follow, each gives a permutation of all the vertices. All the numbers in a line are separated by a space.

### Output Specification:

==Print in a line all the indices of queries which correspond to "NOT a topological order". The indices start from zero.== All the numbers are separated by a space, and there must no extra space at the beginning or the end of the line. ==It is graranteed that there is at least one answer.==

### Sample Input:

    6 8
    1 2
    1 3
    5 2
    5 4
    2 3
    2 6
    3 4
    6 4
    5
    1 5 2 3 6 4
    5 1 2 6 3 4
    5 1 2 3 6 4
    5 2 1 6 3 4
    1 2 3 4 5 6


### Sample Output:

    3 4

# 题解

拓扑排序的性质是，某个节点的前驱一定在这个节点被访问之前访问。可以通过这条性质来判断是否是拓扑排序。为了较容易得到某个节点的前驱，应使用邻接表，并将边反向存储。

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

bool visit[1005];
vector<int> pre[1005], ans;

int main() {
    int n, m, a, b, k;

    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        cin >> a >> b;
        pre[b].push_back(a);
    }

    cin >> k;
    for (int i = 0; i < k; ++i) {
        bool flag = true;
        fill_n(visit, 1005, false);

        for (int j = 0; j < n; ++j) {
            cin >> a;
            if (flag) {
                visit[a] = true;
                for (auto &e : pre[a])
                    if (!visit[e]) {
                        ans.push_back(i);
                        flag = false;
                        break;
                    }
            }
        }
    }

    for (auto i = ans.begin(); i != ans.end(); ++i)
        cout << *i << (i + 1 == ans.end() ? "\n" : " ");
}
```

