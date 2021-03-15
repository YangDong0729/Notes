# A1004 Counting Leaves
A family hierarchy is usually presented by a pedigree tree. Your job is to count those family members who have no child.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing $0<N<100$, the number of nodes in a tree, and $M$ ($<N$), the number of non-leaf nodes. Then $M$ lines follow, each in the format:

    ID K ID[1] ID[2] ... ID[K]


where `ID` is a two-digit number representing a given non-leaf node, `K` is the number of its children, followed by a sequence of two-digit `ID`'s of its children. For the sake of simplicity, let us fix the root ID to be `01`.

The input ends with $N$ being 0. That case must NOT be processed.

### Output Specification:

For each test case, you are supposed to ==count those family members who have no child **for every seniority level** starting from the root.== The numbers must be printed in a line, separated by a space, and there must be no extra space at the end of each line.

The sample case represents a tree with only 2 nodes, where `01` is the root and `02` is its only child. Hence on the root `01` level, there is `0` leaf node; and on the next level, there is `1` leaf node. Then we should output `0 1` in a line.

### Sample Input:

    2 1
    01 1 02


### Sample Output:

    0 1

# 思路

给出一棵树，计算每一层的叶子节点的个数。



可以用树的层序遍历做，也可以用dfs，但是dfs写起来比较快……
 用静态节点比较方便。

# 代码

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> nodes[105];
// leaf[i]：第i层的叶子结点的个数
// level[i]：结点i的层数
// maxLevel最大层数
int leaf[105], level[105], maxLevel;

void dfs(int root) {
    int l = level[root];  // 当深搜到root时，root的层数一定确定了

    if (l > maxLevel) maxLevel = l;

    if (nodes[root].size() == 0)  // 这个结点是叶子结点
        ++leaf[l];  // 这层的叶子结点数+1
    else   // 这个结点不是叶子节点
        for (auto& e : nodes[root]) {  // 遍历他的孩子
            level[e] = l + 1;
            dfs(e);
        }
}
int main() {
    int n, m;

    cin >> n;

    if (n == 0) return 0;

    cin >> m;

    for (int i = 0; i < m; ++i) {
        int id, k;
        cin >> id >> k;

        for (int j = 0; j < k; ++j) {
            int c;
            cin >> c;

            nodes[id].push_back(c);
        }
    }

    level[1] = 0;

    dfs(1);

    for (int i = 0; i <= maxLevel; ++i) {
        cout << leaf[i];
        if (i != maxLevel) cout << " ";
    }
}
```

