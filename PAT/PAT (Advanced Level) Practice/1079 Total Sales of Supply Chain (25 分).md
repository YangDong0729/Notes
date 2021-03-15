A supply chain is a network of retailers（零售商）, distributors（经销商）, and suppliers（供应商）-- everyone involved in moving a product from supplier to customer.

Starting from one root supplier, everyone on the chain buys products from one's supplier in a price $P$ and sell or distribute them in a price that is r% higher than $P$. Only the retailers will face the customers. It is assumed that each member in the supply chain has exactly one supplier except the root supplier, and there is no supply cycle.

Now given a supply chain, you are supposed to tell the total sales from all the retailers.
### Input Specification:
Each input file contains one test case. For each case, the first line contains three positive numbers: $N (≤10^5)$, the total number of the members in the supply chain (and hence their ID's are numbered from 0 to $N−1$, and the root supplier's ID is 0); P, the unit price given by the root supplier; and $r$, the percentage rate of price increment for each distributor or retailer. Then $N$ lines follow, each describes a distributor or retailer in the following format:

$K_i$ ID[1] ID[2] ... ID[$K_i$]

where in the i-th line, $K_i$ is the total number of distributors or retailers who receive products from supplier i, and is then followed by the ID's of these distributors or retailers. $K_j$ being 0 means that the j-th member is a retailer, then instead the total amount of the product will be given after $K_j$. All the numbers in a line are separated by a space.
### Output Specification:
For each test case, print in one line the total sales we can expect from all the retailers, accurate up to 1 decimal place. It is guaranteed that the number will not exceed $10^{10}$.
### Sample Input:
```
10 1.80 1.00
3 2 3 5
1 9
1 4
1 7
0 7
2 6 1
1 8
0 9
0 4
0 3
```
### Sample Output:
```
42.4
```
# 题解

有一个货源，供货结构形成一个树形。每个人都要挣%r，现给出叶子节点的货物数量和根节点的单价，求所有货物的销售额。



BFS或者DFS都可以。
```cpp
#include <algorithm>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct {
    double p, cnt;
    vector<int> children;
} graph[100005];

int main() {
    int n, k, c;
    double r, sum = 0;

    cin >> n >> graph[0].p >> r;
    r = 1 + r / 100;
    for (int i = 0; i < n; ++i) {
        cin >> k;
        if (k == 0)
            cin >> graph[i].cnt;
        else
            for (int j = 0; j < k; ++j) {
                cin >> c;
                graph[i].children.push_back(c);
            }
    }

    queue<int> q;
    q.push(0);
    while (not q.empty()) {
        int top = q.front();
        q.pop();
        auto &children = graph[top].children;
        if (children.size() == 0)
            sum += graph[top].cnt * graph[top].p;
        else
            for (int i = 0; i < children.size(); ++i) {
                graph[children[i]].p = graph[top].p * r;
                q.push(children[i]);
            }
    }

    printf("%.1f", sum);
}
```