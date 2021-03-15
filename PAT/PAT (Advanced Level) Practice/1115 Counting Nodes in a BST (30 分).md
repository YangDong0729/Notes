# 题目详情

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

*   The left subtree of a node contains only nodes with keys ==less than or equal to== the node's key.
*   The right subtree of a node contains only nodes with keys greater than the node's key.
*   Both the left and right subtrees must also be binary search trees.

==Insert a sequence of numbers into an initially empty binary search tree. Then you are supposed to count the total number of nodes in the lowest 2 levels of the resulting tree.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N$ ($\le 1000$) which is the size of the input sequence==. Then given in the next line are the $N$ integers in $[−1000,1000]$ which are supposed to be inserted into an initially empty binary search tree.

### Output Specification:

For each case, print in one line the numbers of nodes in the lowest 2 levels of the resulting tree in the format:

    n1 + n2 = n


where `n1` is the number of nodes in the lowest level, `n2` is that of the level above, and `n` is the sum.

### Sample Input:

    9
    25 30 42 16 20 20 35 -5 28


### Sample Output:

    2 + 4 = 6

# 题解

```cpp
#include <iostream>
#include <queue>

using namespace std;

struct node {
    int val;
    node* left, * right;
};

void insert(node*& root, int val) {
    if (root == nullptr) {
        root = new node{ val };
        return;
    }

    if (root->val < val)  // 题写错了，按照题意这里应该是<=
        insert(root->right, val);
    else insert(root->left, val);
}

int cnt[1005];

void dfs(node *root, int level) {
    cnt[level]++;
    if (root->left) dfs(root->left, level + 1);
    if (root->right) dfs(root->right, level + 1);
}

int main() {
    int n, c;
    node* root = nullptr;

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> c;
        insert(root, c);
    }

    dfs(root, 1);

    for (int i = 1004; i >= 1; --i) {
        if (cnt[i] != 0) {
            cout << cnt[i] << " + " << cnt[i - 1] << " = " << cnt[i] + cnt[i - 1];
            return 0;
        }
    }
}
```

