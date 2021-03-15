# 题目详情
Suppose that ==all the keys in a binary tree are distinct positive integers==. ==Given the postorder and inorder traversal sequences, you are supposed to output the level order traversal sequence of the corresponding binary tree.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N$ ($\le 30$), the total number of nodes in the binary tree. ==The second line gives the postorder sequence and the third line gives the inorder sequence.== All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the level order traversal sequence of the corresponding binary tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

    7
    2 3 1 5 7 6 4
    1 2 3 4 5 6 7


### Sample Output:

    4 1 6 3 5 7 2

# 题解

根据中序遍历和后序遍历计算层序遍历。



根据中序遍历和后序遍历重建这棵树，在这个过程中同时记录节点在层序遍历中的的编号，然后按照这个编号排序就得到了层序遍历的结果。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <algorithm>
#include <cstdio>
#include <vector>

using namespace std;

int postOrder[35], inOrder[35];
vector<pair<int, int>> layerOrder;

void build(int inL, int postL, int len, int index) {
    if (len <= 0) return;

    layerOrder.push_back({ index, postOrder[postL + len - 1] });

    int inRoot = inL;
    while (inRoot < inL + len and inOrder[inRoot] != postOrder[postL + len - 1])
        ++inRoot;

    int lLen = inRoot - inL, rLen = len - lLen - 1;

    build(inL, postL, lLen, index * 2);
    build(inRoot + 1, postL + lLen, rLen, index * 2 + 1);
}

int main() {
    int n;

    scanf("%d", &n);
    for (int i = 0; i < n; ++i) scanf("%d", &postOrder[i]);
    for (int i = 0; i < n; ++i) scanf("%d", &inOrder[i]);

    build(0, 0, n, 1);
    sort(layerOrder.begin(), layerOrder.end());

    for (auto i = layerOrder.begin(); i < layerOrder.end(); ++i) {
        printf("%d", i->second);
        if (i + 1 != layerOrder.end()) printf(" ");
    }
}
```

