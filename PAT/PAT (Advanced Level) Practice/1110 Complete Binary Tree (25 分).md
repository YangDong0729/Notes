# 题目详情
==Given a tree, you are supposed to tell if it is a complete binary tree.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N$ ($\le 20$) which is the total number of nodes in the tree== -- and hence ==the nodes are numbered from 0 to $N−1$. Then $N$ lines follow, each corresponds to a node, and gives the indices of the left and right children of the node==. If the child does not exist, a `-` will be put at the position. Any pair of children are separated by a space.

### Output Specification:

For each case, print in one line `YES` and the index of the last node if the tree is a complete binary tree, or `NO` and the index of the root if not. There must be exactly one space separating the word and the number.

### Sample Input 1:

    9
    7 8
    - -
    - -
    - -
    0 1
    2 3
    4 5
    - -
    - -


### Sample Output 1:

    YES 8


### Sample Input 2:

    8
    - -
    4 5
    0 6
    - -
    2 3
    - 7
    - -
    - -


### Sample Output 2:

    NO 1

# 题解

```cpp
#include <iostream>
#include <queue>
#include <string>

using namespace std;

struct Node {
    int l = -1, r = -1, p = -1;
} node[25];

int maxIdx = 0, last;
void dfs(int root, int idx) {
    if (idx > maxIdx) {
        maxIdx = idx;
        last = root;
    }
    if (node[root].l != -1)
        dfs(node[root].l, idx * 2);
    if (node[root].r != -1)
        dfs(node[root].r, idx * 2 + 1);
}

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; ++i) {
        string l, r;
        cin >> l >> r;
        if (l != "-") {
            int idx = stoi(l);
            node[i].l = idx;
            node[idx].p = i;
        }
        if (r != "-") {
            int idx = stoi(r);
            node[i].r = idx;
            node[idx].p = i;
        }
    }

    int p = 0;
    while (node[p].p != -1)
        p = node[p].p;
    dfs(p, 1);

    if (n == maxIdx) // 完全二叉树中最大的结点序号一定等于结点个数
        cout << "YES " << last;
    else
        cout << "NO " << p;
}
```

