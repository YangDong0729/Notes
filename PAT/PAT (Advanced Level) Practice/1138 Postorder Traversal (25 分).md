# 题目详情
Suppose that ==all the keys in a binary tree are distinct positive integers==. Given the preorder and inorder traversal sequences, you are supposed to output ==the first number of the postorder traversal sequence of the corresponding binary tree==.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N ($\le$ 50,000), the total number of nodes in the binary tree. The second line gives the preorder sequence and the third line gives the inorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the first number of the postorder traversal sequence of the corresponding binary tree.

### Sample Input:

    7
    1 2 3 4 5 6 7
    2 3 1 5 4 7 6


### Sample Output:

    3

# 题解

```cpp
#include <iostream>

using namespace std;

int in[50005], pre[50005];

int firstOfPostOder(int *in, int *pre, int len) {
    if (len == 1)
        return *in;
    int root = *pre;
    int *p = in;
    while (*p != root)
        ++p;
    if (p != in)
        return firstOfPostOder(in, pre + 1, p - in);
    else
        return firstOfPostOder(in + 1, pre + 1, len - 1); // 左子树为空
}

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; ++i)
        cin >> pre[i];
    for (int i = 1; i <= n; ++i)
        cin >> in[i];
    cout << firstOfPostOder(in + 1, pre + 1, n);
}
```

