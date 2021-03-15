# 题目详情
There is a kind of ==balanced binary search tree== named **red-black tree** in the data structure. It has the following 5 properties:

*   (1) Every node is either red or black.
*   (2) The root is black.
*   (3) Every leaf (NULL) is black.
*   (4) If a node is red, then both its children are black.
*   (5) For each node, all simple paths from the node to descendant leaves contain the same number of black nodes.

For example, the tree in Figure 1 is a red-black tree, while the ones in Figure 2 and 3 are not.

| ![rbf1.jpg](https://images.ptausercontent.com/eff80bd4-c833-4818-9786-81680d1b304a.jpg) | ![rbf2.jpg](https://images.ptausercontent.com/b11184df-eaab-451c-b7d4-7fc1dc82b028.jpg) | ![rbf3.jpg](https://images.ptausercontent.com/625c532b-22fc-47b9-80ea-0537cf00d922.jpg) |
| :-------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------: |
|                                        Figure 1                                         |                                        Figure 2                                         |                                        Figure 3                                         |

==For each given binary search tree, you are supposed to tell if it is a legal red-black tree.==

### Input Specification:

Each input file contains several test cases. The first line gives a positive integer ==K ($\le$30) which is the total number of cases.== For each case, the first line gives a positive integer ==N ($\le$30), the total number of nodes in the binary tree.== The second line gives the ==preorder traversal sequence of the tree.== While ==all the keys in a tree are positive integers, we use negative signs to represent red nodes.== All the numbers in a line are separated by a space. The sample input cases correspond to the trees shown in Figure 1, 2 and 3.

### Output Specification:

For each test case, print in a line "Yes" if the given tree is a red-black tree, or "No" if not.

### Sample Input:

    3
    9
    7 -2 1 5 -4 -11 8 14 -15
    9
    11 -2 1 -7 5 -4 8 14 -15
    8
    10 -7 5 -6 8 15 -11 17


### Sample Output:

    Yes
    No
    No

# 题解

**红黑树首先是二叉搜索树，满足二叉搜索树的性质。**对于BST，只需要前序遍历或者后序遍历就可以确定树的结构。对于普通的二叉树，还需要中序遍历，中序遍历的作用是划分左右子树，但是BST的性质可以直接确定左右子树，所以不需要中序遍历。

而且，对于前序遍历，只要逐个插入即可获得二叉搜索树。因为前序遍历是中左右，和插入的位置是相同的。

```cpp
#include <iostream>

using namespace std;

struct node {
    int val;
    node *left, *right;
};

bool is_black(node *x) {
    return x == nullptr or x->val > 0; // 3
}

bool is_red(node *x) {
    return x and x->val < 0;
}

void add(node *&root, int val) {
    if (root == nullptr)
        root = new node{val};
    else if (abs(val) < abs(root->val))
        add(root->left, val);
    else
        add(root->right, val);
}

bool check(node *root, int &black_cnt) {
    if (is_black(root))
        ++black_cnt;
    if (root == nullptr)
        return true;
    if (is_red(root) and !(is_black(root->left) and is_black(root->right)))
        return false; // 4
    int l_b_cnt = 0, r_b_cnt = 0;
    if (check(root->left, l_b_cnt) and check(root->right, r_b_cnt) and l_b_cnt == r_b_cnt) { // 5
        black_cnt += l_b_cnt;
        return true;
    }
    return false;
}

int main() {
    int k, n, x;
    cin >> k;
    for (int i = 0; i < k; ++i) {
        int black_cnt = 0;
        node *root = nullptr;

        cin >> n;
        for (int j = 0; j < n; ++j) {
            cin >> x;
            add(root, x);
        }
        // 2
        cout << (is_black(root) and check(root, black_cnt) ? "Yes" : "No") << "\n";
    }
}
```





