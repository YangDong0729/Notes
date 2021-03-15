# 题目详情
Suppose that ==all the keys in a binary tree are distinct positive integers==. A unique binary tree can be determined by a given pair of postorder and inorder traversal sequences, or preorder and inorder traversal sequences. However, if only the postorder and preorder traversal sequences are given, the corresponding tree may no longer be unique.

Now ==given a pair of postorder and preorder traversal sequences, you are supposed to output the corresponding inorder traversal sequence of the tree. If the tree is not unique, simply output any one of them.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer N ($\le$ 30), the total number of nodes in the binary tree==. The second line gives the preorder sequence and the third line gives the postorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first printf in a line `Yes` if the tree is unique, or `No` if not. Then print in the next line the inorder traversal sequence of the corresponding binary tree. If the solution is not unique, any answer would do. It is guaranteed that at least one solution exists. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input 1:

    7
    1 2 3 4 6 7 5
    2 6 7 4 5 3 1


### Sample Output 1:

    Yes
    2 1 6 4 7 3 5


### Sample Input 2:

    4
    1 2 3 4
    2 4 3 1


### Sample Output 2:

    No
    2 1 3 4
# 题解

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> in;
bool uniq = true;
int pre[35], post[35];

void trav(int preL, int postL, int n) {
    if (n == 1) {
        in.push_back(pre[preL]);
        return;
    }

    int p = pre[preL];
    int l = pre[preL + 1]; // 左子树的根
    int q = postL;
    for (; q < postL + n; ++q) // 在后序遍历中，左子树的根是左子树中最后一个被访问的
        if (post[q] == l)
            break;

    trav(preL + 1, postL, q - postL + 1); // 向左子树递归
    in.push_back(p);
    if (q == postL + n - 2)
        uniq = false; // 说明只有一个子树，那是左子树还是右子树就都有可能了
    else
        trav(preL + 1 + q - postL + 1, q + 1, postL + n - q - 2); // 向右子树递归
}

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; ++i)
        cin >> pre[i];
    for (int i = 0; i < n; ++i)
        cin >> post[i];

    trav(0, 0, n);

    cout << (uniq ? "Yes\n" : "No\n");
    for (auto i = in.begin(); i != in.end(); ++i)
        cout << *i << (i + 1 == in.end() ? "\n" : " ");
}
```

