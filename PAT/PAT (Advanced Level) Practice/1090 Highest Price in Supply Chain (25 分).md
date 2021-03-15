# 题目详情
A supply chain is a network of retailers（零售商）, distributors（经销商）, and suppliers（供应商）-- everyone involved in moving a product from supplier to customer.

==Starting from one root supplier, everyone on the chain buys products from one's supplier in a price $P$ and sell or distribute them in a price that is $r$% higher than $P$. It is assumed that each member in the supply chain has exactly one supplier except the root supplier, and there is no supply cycle.==

Now given a supply chain, you are supposed to tell the highest price we can expect from some retailers.

### Input Specification:

Each input file contains one test case. For each case, The first line contains ==three positive numbers: $N$ ($\le 10^5$), the total number of the members in the supply chain (and hence they are numbered from 0 to $N-1$); $P$, the price given by the root supplier; and $r$, the percentage rate of price increment for each distributor or retailer.== Then the next line contains $N$ numbers, each number $S_i$ is the index of the supplier for the $i$-th member. $S_{root}$ for the root supplier is defined to be $-1$. All the numbers in a line are separated by a space.

### Output Specification:

==For each test case, print in one line the highest price we can expect from some retailers, accurate up to 2 decimal places, and the number of retailers that sell at the highest price==. There must be one space between the two numbers. It is guaranteed that the price will not exceed $10^{10}$.

### Sample Input:

    9 1.80 1.00
    1 5 4 4 -1 4 5 3 6


### Sample Output:

    1.85 2

# 题解

销售网络形成一棵树，根节点有一个基础价格，每向下传递一个就溢价r%，求零售商的最高价格和这个价格的零售商的个数。

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

struct Node {
    double p;
    vector<int> children;
} node[100005];

int n, root = -1, cnt;
double rootPrice, r, maxP;

void dfs(int x) {
    if (node[x].children.empty()) {
        if (node[x].p > maxP) {
            maxP = node[x].p;
            cnt = 1;
        }
        else if (node[x].p == maxP) {
            ++cnt;
        }
    }
    else {
        for (auto i : node[x].children) {
            node[i].p = node[x].p * r;
            dfs(i);
        }
    }
}

int main() {
    cin >> n >> rootPrice >> r;

    r = 1 + r / 100;

    for (int i = 0; i < n; ++i) {
        int k;
        cin >> k;

        if (k == -1) {
            root = i;
            node[root].p = rootPrice;
        }
        else
            node[k].children.push_back(i);
    }

    dfs(root);

    printf("%.2f %d", maxP, cnt);
}
```

