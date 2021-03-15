# 题目详情
==The lowest common ancestor (LCA) of two nodes U and V in a tree is the deepest node that has both U and V as descendants.==

Given any two nodes in a binary tree, you are supposed to find their LCA.

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers: $M (≤1,000)$, the number of pairs of nodes to be tested; and $N (≤10,000)$, the number of keys in the binary tree==, respectively. ==In each of the following two lines, N distinct integers are given as the inorder and preorder traversal sequences of the binary tree==, respectively. It is guaranteed that the binary tree can be uniquely determined by the input sequences. ==Then M lines follow, each contains a pair of integer keys U and V. All the keys are in the range of **int**.==

### Output Specification:

==For each given pair of U and V, print in a line `LCA of U and V is A.` if the LCA is found and `A` is the key==. But ==if `A` is one of U and V, print `X is an ancestor of Y.` where `X` is `A` and `Y` is the other node.== ==If U or V is not found in the binary tree, print in a line `ERROR: U is not found.` or `ERROR: V is not found.` or `ERROR: U and V are not found.`.==

### Sample Input:

    6 8
    7 2 3 4 6 5 1 8
    5 3 7 2 6 4 8 1
    2 6
    8 1
    7 9
    12 -3
    0 8
    99 99

### Sample Output:

    LCA of 2 and 6 is 3.
    8 is an ancestor of 1.
    ERROR: 9 is not found.
    ERROR: 12 and -3 are not found.
    ERROR: 0 is not found.
    ERROR: 99 and 99 are not found.
# 题解

给出一棵二叉树的中序和前序遍历，找两个结点的最低公共祖先节点。



中序遍历是按照左子树、根、右子树的顺序遍历的，所以节点在中序遍历的相对位置就可以判断这两个节点的LCA在哪部分。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <unordered_map>
#include <vector>

#define SIZE 10005

using namespace std;

int in[SIZE], pre[SIZE];
unordered_map<int, int> pos; // pos记录每个结点在中序遍历中的位置

void lca(int inL, int inR, int preL, int a, int b) {
    if (inL > inR)
        return;

    int inRoot = pos[pre[preL]], aIn = pos[a], bIn = pos[b];

    if (aIn < inRoot and bIn < inRoot) // 最低公共结点在root的左子树
        lca(inL, inRoot - 1, preL + 1, a, b);
    else if ((aIn < inRoot and bIn > inRoot) or (aIn > inRoot and bIn < inRoot)) // LCA就是root
        printf("LCA of %d and %d is %d.\n", a, b, in[inRoot]);
    else if (aIn > inRoot and bIn > inRoot) // LCA在root的右子树
        lca(inRoot + 1, inR, preL + 1 + (inRoot - inL), a, b);
    else if (aIn == inRoot)
        printf("%d is an ancestor of %d.\n", a, b);
    else if (bIn == inRoot)
        printf("%d is an ancestor of %d.\n", b, a);
}

int main() {
    int m, n, a, b;
    scanf("%d %d", &m, &n);

    for (int i = 1; i <= n; i++) {
        scanf("%d", &in[i]);
        pos[in[i]] = i;  // 免得重建时还需要查找，也便于确定是否存在
    }
    for (int i = 1; i <= n; i++)
        scanf("%d", &pre[i]);

    for (int i = 0; i < m; i++) {
        scanf("%d %d", &a, &b);
        if (pos[a] == 0 and pos[b] == 0)
            printf("ERROR: %d and %d are not found.\n", a, b);
        else if (pos[a] == 0 or pos[b] == 0)
            printf("ERROR: %d is not found.\n", pos[a] == 0 ? a : b);
        else
            lca(1, n, 1, a, b);
    }
}
```

