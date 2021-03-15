A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:


- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right sub+trees must also be binary search trees.



==Given the structure of a binary tree and a sequence of distinct integer keys, there is only one way to fill these keys into the tree so that the resulting tree satisfies the definition of a BST. You are supposed to output the level order traversal sequence of that tree.== The sample is illustrated by Figure 1 and 2.


![](https://images.ptausercontent.com/24c2521f-aaed-4ef4-bac8-3ff562d80a1b.jpg#align=left&display=inline&height=341&margin=%5Bobject%20Object%5D&originHeight=341&originWidth=549&status=done&style=none&width=549)

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $$N \ (≤100)$$ which is the total number of nodes in the tree==. The next $$N$$ lines each contains the left and the right children of a node in the format `left_index right_index`, ==provided that the nodes are numbered from $$0$$ to $$N-1$$, and 0 is always the root. If one child is missing, then −1 will represent the NULL child pointer.== Finally $$N$$ distinct integer keys are given in the last line.
### Output Specification:

For each test case, print in one line the level order traversal sequence of that tree. All the numbers must be separated by a space, with no extra space at the end of the line.
### Sample Input:

```
9
1 6
2 3
-1 -1
-1 4
5 -1
-1 -1
7 -1
-1 8
-1 -1
73 45 11 58 82 25 67 38 42
```
### Sample Output:

```
58 25 82 11 38 67 45 73 42
```
# 题解

给出一个二叉树的结构和一个不含有相同数字的序列，那么只有一种方法把这些数字填到这棵树的节点中，使之成为一棵BST。求这棵树的层序遍历。



BST的中序遍历是有序的，可以利用这个性质将关键字填到节点中。
```cpp
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

struct {
    int val;
    int lChild, rChild;
} node[105];

int arr[105];

int inOrder(int root, int &index) {
    if (node[root].lChild != -1)
        inOrder(node[root].lChild, index);

    node[root].val = arr[index++];

    if (node[root].rChild != -1)
        inOrder(node[root].rChild, index);
}

void levelOrder() {
    queue<int> q;
    q.push(0);

    while (not q.empty()) {
        int top = q.front();
        q.pop();

        cout << node[top].val;

        if (node[top].lChild != -1) q.push(node[top].lChild);
        if (node[top].rChild != -1) q.push(node[top].rChild);

        if (not q.empty()) cout << " ";
    }
}

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; ++i)
        cin >> node[i].lChild >> node[i].rChild;

    for (int i = 0; i < n; ++i)
        cin >> arr[i];

    sort(arr, arr + n);

    int i = 0;
    inOrder(0, i);

    levelOrder();
}
```
