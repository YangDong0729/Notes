A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

+ The left subtree of a node contains only nodes with keys less than the node's key.
+ The right subtree of a node contains only nodes with keys ==greater than or equal to== the node's key.
+ Both the left and right subtrees must also be binary search trees.

==If we swap the left and right subtrees of every node, then the resulting tree is called the **Mirror Image** of a BST.==

==Now given a sequence of integer keys, you are supposed to tell if it is the preorder traversal sequence of a BST or the mirror image of a BST.==

### Input Specification:
Each input file contains one test case. For each case, ==the first line contains a positive integer $N (≤1000)$. Then $N$ integer keys are given in the next line==. All the numbers in a line are separated by a space.
### Output Specification:
For each test case, first print in a line `YES` if the sequence is the preorder traversal sequence of a BST or the mirror image of a BST, or `NO` if not. Then if the answer is `YES`, print in the next line the postorder traversal sequence of that tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.
### Sample Input 1:
```
7
8 6 5 7 10 8 11
```
### Sample Output 1:
```
YES
5 7 6 8 11 10 8
```
### Sample Input 2:
```
7
8 10 11 8 6 7 5
```
### Sample Output 2:
```
YES
11 8 10 7 5 6 8
```
### Sample Input 3:
```
7
8 6 8 5 10 9 11
```
### Sample Output 3:

```
NO
```
# 题解

给出一个二叉树的前序遍历，判断它是不是二叉搜索树或者二叉搜索树的镜像（左右子树互换），是则输出后序遍历。



给定一个二叉搜索树的前序遍历后，有两种方法重建树：

+ 根据二叉搜索树的性质划分左右子树，时间复杂度$O(n)$。
+ 按照序列的顺序一个个插入到一个空的二叉搜索树中，最坏时间复杂度$O(n^2)$。这种方式能构建出同一颗二叉搜索树的所有序列中，其前序遍历是字典序最小的。

按照第一种思路，在划分子树并获得后序遍历的过程中就可以判定它是不是二叉搜索树，从而提前终止，按照镜像的方式再划分一次，就可以判定它是不是二叉搜索树的镜像。

按照第二种思路，根据序列重新构建一颗二叉搜索树，然后获得前序遍历，判断与给定序列是否相同就可以判断是不是二叉搜索树。由于镜像实际上就是左右子树对换，所以按照根右左（也就是镜像的前序）的顺序遍历，再与给定序列比较，就可以判定是不是二叉搜索树的镜像。在获得后续遍历时也应该考虑镜像的问题。

注意，这道题实际上无法完全区分二叉搜索树及其镜像，但是答案是唯一的，因为对于无法区分的情况，后序遍历一定是相同的。
#### 思路1
```cpp
#include <iostream>
#include <vector>

using namespace std;

int pre[1005];
vector<int> post;
bool mirror = false;

bool postTravel(int preL, int len) {
    if (len < 1) return true;

    int root = pre[preL];
    int leftR = preL + 1, rightR;

    if (mirror) {
        while (leftR < preL + len and pre[leftR] >= root) ++leftR;
        rightR = leftR;
        while (rightR < preL + len and pre[rightR] < root) ++rightR;
    }
    else {
        while (leftR < preL + len and pre[leftR] < root) ++leftR;
        rightR = leftR;
        while (rightR < preL + len and pre[rightR] >= root) ++rightR;
    }

    if (rightR == preL + len
        and postTravel(preL + 1, leftR - preL - 1)
        and postTravel(leftR, rightR - leftR)) {
        post.push_back(root);
        return true;
    }
    return false;
}

int main() {
    int n;

    cin >> n;
    for (int i = 0; i < n; ++i)
        cin >> pre[i];

    if (not postTravel(0, n)) {
        post.clear();
        mirror = true;
        if (not postTravel(0, n)) {
            cout << "NO\n";
            return 0;
        }
    }

    cout << "YES\n";
    for (auto i = post.begin(); i != post.end(); ++i)
        cout << *i << (i + 1 == post.end() ? "\n" : " ");
}
```
#### 思路2
```cpp
#include <iostream>
#include <vector>

using namespace std;

struct node {
    int val;
    node *right, *left;
};

bool mirror = false;
vector<int> pre, post, seq;

void insert(node *&root, int val) {
    if (root == nullptr) root = new node{val};
    else if (root->val > val) insert(root->left, val);
    else insert(root->right, val);
}

void getPre(node *root) {
    if (root == nullptr) return;
    pre.push_back(root->val);
    if (mirror) {
        getPre(root->right);
        getPre(root->left);
    } else {
        getPre(root->left);
        getPre(root->right);
    }
}

void getPost(node *root) {
    if (root == nullptr) return;
    if (mirror) {
        getPost(root->right);
        getPost(root->left);
    } else {
        getPost(root->left);
        getPost(root->right);
    }
    post.push_back(root->val);
}

int main() {
    int n, x;
    node *root = nullptr;

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> x;
        seq.push_back(x);
        insert(root, x);
    }

    getPre(root);
    if (seq == pre)
        getPost(root);
    else {
        mirror = true;
        pre.clear();
        getPre(root);
        if (seq == pre)
            getPost(root);
        else {
            cout << "NO\n";
            return 0;
        }
    }
    cout << "YES\n";
    for (auto i = post.begin(); i != post.end(); ++i)
        cout << *i << (i + 1 == post.end() ? "\n" : " ");
}
```