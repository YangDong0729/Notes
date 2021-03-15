# 题目详情
Suppose that all the keys in a binary tree are distinct positive integers. A unique binary tree can be determined by a given pair of postorder and inorder traversal sequences. And it is a simple standard routine to print the numbers in level-order. However, if you think the problem is too simple, then you are too naive. This time you are supposed to print the numbers in "zigzagging order" -- that is, ==starting from the root, print the numbers level-by-level, alternating between left to right and right to left.== For example, for the following tree you must output: 1 11 5 8 17 12 20 15.

![zigzag.jpg](https://images.ptausercontent.com/337cbfb0-a7b2-4500-9664-318e9ffc870e.jpg)

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer N ($\le$30), the total number of nodes in the binary tree==. ==The second line gives the inorder sequence and the third line gives the postorder sequence==. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the zigzagging sequence of the tree in a line. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

    8
    12 11 20 17 1 15 8 5
    12 20 17 11 15 8 5 1


### Sample Output:

    1 11 5 8 17 12 20 15

# 题解

通过给出的中序和后序遍历重建这棵树，在重建过程中给每个节点分配一个index（按照完全二叉树的规则），按照index排序后就可以得到层序遍历的结果。还可以用index来区分层次（结点A的index为a，与A在同一层的结点的index小于2a，这样只要知道每一层的第一个结点，就可以确定该层的所有节点）。最后按照层次翻转层序遍历即可。

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

int in[35], post[35];

struct node {
    int val, index;
    bool operator<(const node &x) const {
        return index < x.index;
    }
};

vector<node> t;

void trav(int inL, int postL, int len, int index) {
    if (len <= 0)
        return;
    int root = post[postL + len - 1];
    t.push_back({root, index});

    // 找到root
    int pos = inL;
    for (int i = 0; i < len; ++i) {
        if (in[inL + i] == root) {
            pos += i;
            break;
        }
    }

    // 遍历子树
    trav(inL, postL, pos - inL, index * 2);
    trav(pos + 1, postL + pos - inL, inL + len - pos - 1, index * 2 + 1);
}

int main() {
    int n;
    cin >> n;

    for (int i = 1; i <= n; ++i)
        cin >> in[i];
    for (int i = 1; i <= n; ++i)
        cin >> post[i];

    trav(1, 1, n, 1);
    sort(t.begin(), t.end()); // 得到层序遍历

    bool should_reverse = true;
    for (int i = 0, j, min = 1; i < t.size(); i = j, min *= 2, should_reverse = !should_reverse) {
        j = i;
        while (min <= t[j].index and t[j].index < min * 2)
            ++j;
        if (should_reverse)
            reverse(t.begin() + i, t.begin() + j);
    }

    for (auto i = t.begin(); i != t.end(); ++i)
        cout << i->val << (i + 1 == t.end() ? "\n" : " ");
}
```

