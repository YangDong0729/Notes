# 题目详情
==An AVL tree is a self-balancing binary search tree==. In an AVL tree, ==the heights of the two child subtrees of any node differ by at most one==; if at any time they differ by more than one, rebalancing is done to restore this property. Figures 1-4 illustrate the rotation rules.

| ![F1.jpg](https://images.ptausercontent.com/fb337acb-93b0-4af2-9838-deff5ce98058.jpg) | ![F2.jpg](https://images.ptausercontent.com/d1635de7-3e3f-4aaa-889b-ba29f35890db.jpg) |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
| ![F3.jpg](https://images.ptausercontent.com/e868e4b9-9fea-4f70-b7a7-1f5d8a3be4ef.jpg) | ![F4.jpg](https://images.ptausercontent.com/98aa1782-cea5-4792-8736-999436cf43a9.jpg) |

==Now given a sequence of insertions, you are supposed to output the level-order traversal sequence of the resulting AVL tree, and to tell if it is a complete binary tree.==

### Input Specification:

Each input file contains one test case. For each case, ==the first line contains a positive integer N ($\le$ 20). Then N distinct integer keys are given in the next line==. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, insert the keys one by one into an initially empty AVL tree. Then first print in a line the level-order traversal sequence of the resulting AVL tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line. Then in the next line, print `YES` if the tree is complete, or `NO` if not.

### Sample Input 1:

    5
    88 70 61 63 65


### Sample Output 1:

    70 63 88 61 65
    YES


### Sample Input 2:

    8
    88 70 61 96 120 90 65 68


### Sample Output 2:

    88 65 96 61 70 90 120 68
    NO
# 题解

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct node {
    int val;
    node *left, *right;
};

void leftRotate(node *&A) {
    node *B = A->right;
    A->right = B->left;
    B->left = A;
    A = B;
}

void rightRotate(node *&A) {
    node *B = A->left;
    A->left = B->right;
    B->right = A;
    A = B;
}

void leftRightRotate(node *&A) {
    leftRotate(A->left);
    rightRotate(A);
}

void rightLeftRotate(node *&A) {
    rightRotate(A->right);
    leftRotate(A);
}

int getHeight(node *A) {
    if (A == nullptr)
        return 0;
    int l = getHeight(A->left);
    int r = getHeight(A->right);
    return max(l, r) + 1;
}

void insert(node *&A, int val) {
    if (A == nullptr) {
        A = new node{val};
    } else if (A->val > val) {
        insert(A->left, val);
        if (getHeight(A->left) - getHeight(A->right) >= 2) {
            if (val < A->left->val)
                rightRotate(A);
            else
                leftRightRotate(A);
        }
    } else {
        insert(A->right, val);
        if (getHeight(A->left) - getHeight(A->right) <= -2) {
            if (val > A->right->val)
                leftRotate(A);
            else
                rightLeftRotate(A);
        }
    }
}

int main() {
    int n, a;
    node *root = nullptr;

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> a;
        insert(root, a);
    }

    queue<node *> q;
    q.push(root);

    vector<int> level_order;
    bool complete = true, leaf = false;

    while (not q.empty()) {
        auto front = q.front();
        q.pop();
        if (front->left)
            q.push(front->left);
        if (front->right)
            q.push(front->right);
        level_order.push_back(front->val);
        // 判定完全二叉树
        if (!front->left and !front->right) // 注意这个判断应该放在前面
            leaf = true;                    // 后面全应该是叶子
        else if (leaf)
            complete = false;
        if (front->right and !front->left) // 右有左无一定不能出现
            complete = false;
        else if (!front->right and front->left)
            leaf = true; // 后面全应该是叶子
    }

    for (auto i = level_order.begin(); i != level_order.end(); ++i)
        cout << *i << (i + 1 == level_order.end() ? "\n" : " ");
    cout << (complete ? "YES" : "NO") << "\n";
}
```

