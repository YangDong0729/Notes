# 题目详情
The lowest common ancestor (LCA) of two nodes U and V in a tree is the deepest node that has both U and V as descendants.

A binary search tree (BST) is recursively defined as a binary tree which has the following properties:

*   The left subtree of a node contains only nodes with keys less than the node's key.
*   The right subtree of a node contains only nodes with keys ==greater than or equal to== the node's key.
*   Both the left and right subtrees must also be binary search trees.

Given any two nodes in a BST, you are supposed to find their LCA.

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers: $M$ ($\le 1,000$), the number of pairs of nodes to be tested; and $N$ ($\le 10,000$), the number of keys in the BST==, respectively. In the second line, N distinct integers are given as the ==preorder traversal sequence of the BST==. ==Then M lines follow, each contains a pair of integer keys U and V==. All the keys are in the range of **int**.

### Output Specification:

For each given pair of U and V, print in a line `LCA of U and V is A.` if the LCA is found and `A` is the key. But if `A` is one of U and V, print `X is an ancestor of Y.` where `X` is `A` and `Y` is the other node. If U or V is not found in the BST, print in a line `ERROR: U is not found.` or `ERROR: V is not found.` or `ERROR: U and V are not found.`.

### Sample Input:

    6 8
    6 3 1 2 5 4 8 7
    2 5
    8 7
    1 9
    12 -3
    0 8
    99 99


### Sample Output:

    LCA of 2 and 5 is 3.
    8 is an ancestor of 7.
    ERROR: 9 is not found.
    ERROR: 12 and -3 are not found.
    ERROR: 0 is not found.
    ERROR: 99 and 99 are not found.
# 题解

给出一棵二叉搜索树的前序遍历，找出给定的两个节点的LCA。



由于BST的性质，这棵树中只有LCA的值在这两个节点之间，而且这两个结点分别在LCA的左右子树中。所以只需要在前序遍历中找到第一个值在这两个结点之间的结点，就是LCA。

```cpp
#include <algorithm>
#include <cstdio>
#include <unordered_set>
#include <vector>

using namespace std;

vector<int> pre;
unordered_set<int> all;

int main() {
    int m, n, x, y;

    scanf("%d%d", &m, &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &x);
        all.insert(x);
        pre.push_back(x);
    }

    for (int i = 0; i < m; ++i) {
        scanf("%d%d", &x, &y);
        bool p = all.count(x);
        bool q = all.count(y);
        if (!p or !q) {
            if (p or q)
                printf("ERROR: %d is not found.\n", !p ? x : y);
            else
                printf("ERROR: %d and %d are not found.\n", x, y);
        } else {
            auto iter = pre.begin();
            for (; iter != pre.end(); ++iter) {
                if (*iter >= min(x, y) and *iter <= max(x, y))
                    break;
            }
            if (*iter == x)
                printf("%d is an ancestor of %d.\n", x, y);
            else if (*iter == y)
                printf("%d is an ancestor of %d.\n", y, x);
            else
                printf("LCA of %d and %d is %d.\n", x, y, *iter);
        }
    }
}
```

