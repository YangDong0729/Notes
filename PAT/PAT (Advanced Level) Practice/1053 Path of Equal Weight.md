==Given a non-empty tree with root $R$, and with weight $W_i$ assigned to each tree node $T_i$.**The weight of a path from $R$ to $L$** is defined to be the sum of the weights of all the nodes along the path from $R$ to any leaf node $L$.==

==Now given any weighted tree, you are supposed to find all the paths with their weights equal to a given number==. For example, let's consider the tree showed in the following figure: for each node, the upper number is the node ID which is a two-digit number, and the lower number is the weight of that node. Suppose that the given number is 24, then there exists 4 different paths which have the same given weight: {10 5 2 7}, {10 4 10}, {10 3 3 6 2} and {10 3 3 6 2}, which correspond to the red edges in the figure.

![fde280325ee2c27f135d0578c13eaa0d.png](https://images.ptausercontent.com/212)

### Input Specification:
Each input file contains one test case. Each case starts with ==a line containing $0<N≤100$, the number of nodes in a tree, $M (<N)$, the number of non-leaf nodes, and $0<S<2^{30}$, the given weight number==. The next line contains $N$ positive numbers where $W_i (<1000)$ corresponds to the tree node $T_i$. Then $M$ lines follow, each in the format:
```
ID K ID[1] ID[2] ... ID[K]
```
where `ID` is a two-digit number representing a given non-leaf node, `K` is the number of its children, followed by a sequence of two-digit `ID`'s of its children. For the sake of simplicity, let us fix the root ID to be `00`.

### Output Specification:
For each test case, ==print all the paths with weight S in **non-increasing** order.== ==Each path occupies a line with printed weights from the root to the leaf in order. All the numbers must be separated by a space with no extra space at the end of the line.==

Note: sequence $\{A_1,A_2,⋯,A_n\}$ is said to be **greater than** sequence $\{B_1,B_2,⋯,B_m\}$ if there exists $1≤k<min\{n,m\}$ such that $A_i=B_i$ for $i=1,⋯,k$, and $A_{k+1}>B_{k+1}$.

### Sample Input:
```
20 9 24
10 2 4 3 5 10 2 18 9 7 2 2 1 3 12 1 8 6 2 2
00 4 01 02 03 04
02 1 05
04 2 06 07
03 3 11 12 13
06 1 09
07 2 08 10
16 1 15
13 3 14 16 17
17 2 18 19
```
### Sample Output:
```
10 5 2 7
10 4 10
10 3 3 6 2
10 3 3 6 2
```

# 题解

找出所有从根节点到叶子节点的路径，要求这些路径的权重等于一个给定的值。



深搜，在深搜的过程中累计总权重，并适当剪枝。在输入时对节点按照权重排序，可以保证搜索到路径的顺序，于是只要搜索到一条路径就可以直接打印。
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

struct node {
    int weight;
    vector<int> children;

    bool isLeaf() {
        return children.size() == 0;
    }
};

int n, m, s;
node nodes[105];
vector<int> path;

void dfs(int idx, int weight) {
    if (nodes[idx].isLeaf() or weight >= s) {
        if (nodes[idx].isLeaf() and weight == s) {
            for (int i = 0; i < path.size(); ++i) {
                cout << path[i];
                if (i != path.size() - 1)
                    cout << " ";
            }
            cout << '\n';
        }
        return;
    }

    for (auto &e : nodes[idx].children) {
        path.push_back(nodes[e].weight);
        dfs(e, weight + nodes[e].weight);
        path.pop_back();
    }
}

int main() {
    cin >> n >> m >> s;

    for (int i = 0; i < n; ++i)
        cin >> nodes[i].weight;

    for (int i = 0; i < m; ++i) {
        int t, u;
        cin >> t >> u;

        for (int j = 0; j < u; ++j) {
            int c;
            cin >> c;
            nodes[t].children.push_back(c);
        }
        // 排序，这样就能够保证路径按照非升输出
        sort(nodes[t].children.begin(), nodes[t].children.end(),
             [](int a, int b) {
                 return nodes[a].weight > nodes[b].weight;
             });
    }

    path.push_back(nodes[0].weight);
    dfs(0, nodes[0].weight);
}
```