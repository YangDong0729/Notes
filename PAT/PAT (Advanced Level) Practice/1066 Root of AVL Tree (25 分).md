# 题目详情
An AVL tree is a self-balancing binary search tree. In an AVL tree, the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done to restore this property. Figures 1-4 illustrate the rotation rules.

![](https://images.ptausercontent.com/31) ![](https://images.ptausercontent.com/32)

![](https://images.ptausercontent.com/33) ![](https://images.ptausercontent.com/34)

==Now given a sequence of insertions, you are supposed to tell the root of the resulting AVL tree.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer $N$ ($\le 20$) which is the total number of keys to be inserted. Then $N$ distinct integer keys are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the root of the resulting AVL tree in one line.

### Sample Input 1:

    5
    88 70 61 96 120

### Sample Output 1:

    70


### Sample Input 2:

    7
    88 70 61 96 120 90 65


### Sample Output 2:

    88

# 题解

```cpp
#include <algorithm>
#include <cstdio>

using namespace std;

struct node {
    int v;
    node* left, * right;

    static int height(node* root) {
        return root ? max(height(root->left), height(root->right)) + 1 : 0;
    }

    int balance_factor() {
        return height(left) - height(right);
    }
};

void left_rotate(node*& root) {
    node* temp = root->right;
    root->right = temp->left;
    temp->left = root;
    root = temp;
}

void right_rotate(node*& root) {
    node* temp = root->left;
    root->left = temp->right;
    temp->right = root;
    root = temp;
}

void insert(node*& root, int v) {
    if (root == nullptr) {
        root = new node{ v };
        return;
    }

    if (v < root->v) {
        insert(root->left, v);
        if (root->balance_factor() == 2) {
            if (v < root->left->v) {
                right_rotate(root);
            }
            else {
                left_rotate(root->left);
                right_rotate(root);
            }
        }
    }
    else {
        insert(root->right, v);
        if (root->balance_factor() == -2) {
            if (v >= root->right->v) {
                left_rotate(root);
            }
            else {
                right_rotate(root->right);
                left_rotate(root);
            }
        }
    }
}

int main() {
    node* root = nullptr;

    int n, v;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &v);
        insert(root, v);
    }
    printf("%d\n", root->v);
}
```

