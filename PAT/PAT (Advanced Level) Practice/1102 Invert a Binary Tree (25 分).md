# 题目详情
The following is from Max Howell @twitter:

    Google: 90% of our engineers use the software you wrote (Homebrew), but you can't invert a binary tree on a whiteboard so fuck off.


Now it's your turn to prove that YOU CAN invert a binary tree!

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N (\le 10)$ which is the total number of nodes in the tree== -- and hence the nodes are numbered from 0 to $N−1$. Then $N$ lines follow, each corresponds to a node from 0 to $N−1$, and gives the indices of the left and right children of the node. If the child does not exist, a `-` will be put at the position. Any pair of children are separated by a space.

### Output Specification:

For each test case, ==print in the first line the level-order, and then in the second line the in-order traversal sequences of the inverted tree==. There must be exactly one space between any adjacent numbers, and no extra space at the end of the line.

### Sample Input:

    8
    1 -
    - -
    0 -
    2 7
    - -
    - -
    5 -
    4 6


### Sample Output:

    3 7 2 6 4 0 5 1
    6 5 7 4 3 2 0 1

# 题解

```cpp
#include <iostream>
#include <cctype>
#include <sstream>
#include <queue>
#include <algorithm>

using namespace std;

struct {
    int lChild = -1, rChild = -1;
} node[15];

bool isChild[15];

void levelOrder(int root) {
    queue<int> q;
    q.push(root);

    while (!q.empty()) {
        int top = q.front();
        q.pop();
        if (node[top].lChild != -1) q.push(node[top].lChild);
        if (node[top].rChild != -1) q.push(node[top].rChild);

        cout << top;
        if (!q.empty()) cout << " ";
    }

    cout << "\n";
}

void inOrder(int root) {
    if (node[root].lChild != -1) {
        inOrder(node[root].lChild);
        cout << " ";
    }
    cout << root;
    if (node[root].rChild != -1) {
        cout << " ";
        inOrder(node[root].rChild);
    }
}

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; ++i) {
        char c;
        cin >> c;
        if (isdigit(c)) {  // 在读入时反向
            int r = c - '0';
            node[i].rChild = r;
            isChild[r] = true;
        }
        cin >> c;
        if (isdigit(c)) {
            int l = c - '0';
            node[i].lChild = l;
            isChild[l] = true;
        }
    }

    int root = 0;
    while (isChild[root]) ++root;

    levelOrder(root);
    inOrder(root);
}
```

