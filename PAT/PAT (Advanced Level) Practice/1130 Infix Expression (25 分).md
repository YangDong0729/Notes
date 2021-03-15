# 题目详情
==Given a syntax tree (binary), you are supposed to output the corresponding infix expression, with parentheses reflecting the precedences of the operators.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer N ($\le 20$) which is the total number of nodes in the syntax tree==. Then N lines follow, each gives the information of a node (the ii\-th line corresponds to the $i$\-th node) in the format:

    data left_child right_child


where `data` is a string of no more than 10 characters, `left_child` and `right_child` are the indices of this node's left and right children, respectively. The nodes are indexed from 1 to N. The NULL link is represented by $-1$. The figures 1 and 2 correspond to the samples 1 and 2, respectively.

| ![infix1.JPG](https://images.ptausercontent.com/4d1c4a98-33cc-45ff-820f-c548845681ba.JPG) | ![infix2.JPG](https://images.ptausercontent.com/b5a3c36e-91ad-494a-8853-b46e1e8b60cc.JPG) |
| :---------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------: |
|                                         Figure 1                                          |                                         Figure 2                                          |

### Output Specification:

For each case, print in a line the infix expression, with parentheses reflecting the precedences of the operators. Note that ==there must be no extra parentheses for the final expression, as is shown by the samples==. There must be no space between any symbols.

### Sample Input 1:

    8
    * 8 7
    a -1 -1
    * 4 1
    + 2 5
    b -1 -1
    d -1 -1
    - -1 6
    c -1 -1


### Sample Output 1:

    (a+b)*(c*(-d))


### Sample Input 2:

    8
    2.35 -1 -1
    * 6 1
    - -1 4
    % 7 8
    + 2 3
    a -1 -1
    str -1 -1
    871 -1 -1


### Sample Output 2:

    (a*2.35)+(-(str%871))

# 题解

通过中序遍历输出中缀表达式，注意对括号的处理。

```cpp
#include <iostream>
#include <map>
#include <string>

using namespace std;

struct {
    string v;
    int l = -1, r = -1, p = -1;
} tree[25];

void in(int root) {
    if (tree[root].p != -1 and (tree[root].l != -1 or tree[root].r != -1))
        cout << "(";
    if (tree[root].l != -1)
        in(tree[root].l);
    cout << tree[root].v;
    if (tree[root].r != -1)
        in(tree[root].r);
    if (tree[root].p != -1 and (tree[root].l != -1 or tree[root].r != -1))
        cout << ")";
}

int main() {
    string s;
    int n, l, r;

    cin >> n;
    for (int i = 1; i <= n; ++i) {
        cin >> s >> l >> r;
        tree[i].v = s;
        tree[i].l = l;
        tree[i].r = r;
        if (l != -1)
            tree[l].p = i;
        if (r != -1)
            tree[r].p = i;
    }

    int root = 1;
    while (tree[root].p != -1)
        root = tree[root].p;

    in(root);
}
```

