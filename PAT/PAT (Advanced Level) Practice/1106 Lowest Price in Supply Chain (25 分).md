# 题目详情
A supply chain is a network of retailers（零售商）, distributors（经销商）, and suppliers（供应商）-- everyone involved in moving a product from supplier to customer.

Starting from one root supplier, everyone on the chain buys products from one's supplier in a price $P$ and sell or distribute them in a price that is $r%$ higher than $P$. Only the retailers will face the customers. It is assumed that each member in the supply chain has exactly one supplier except the root supplier, and there is no supply cycle.

==Now given a supply chain, you are supposed to tell the lowest price a customer can expect from some retailers.==

### Input Specification:

Each input file contains one test case. For each case, The first line contains ==three positive numbers: $N$ ($\le 10^5$), the total number of the members in the supply chain== (and hence their ID's are numbered from 0 to $N-1$, and the root supplier's ID is 0); ==$P$, the price given by the root supplier==; and ==$r$, the percentage rate of price increment for each distributor or retailer==. Then $N$ lines follow, each describes a distributor or retailer in the following format:

$$K_i\ ID[1]\ ID[2]\ ...\ ID[K_i]$$

where in the $i$-th line, $K_i$ is the total number of distributors or retailers who receive products from supplier $i$, and is then followed by the ID's of these distributors or retailers. $K_j$ being 0 means that the $j$-th member is a retailer. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, ==print in one line the lowest price we can expect from some retailers, accurate up to 4 decimal places, and the number of retailers that sell at the lowest price==. There must be one space between the two numbers. It is guaranteed that the all the prices will not exceed $10^{10}$.

### Sample Input:

    10 1.80 1.00
    3 2 3 5
    1 9
    1 4
    1 7
    0
    2 6 1
    1 8
    0
    0
    0

### Sample Output:

    1.8362 2

# 题解

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <iomanip>
#include <algorithm>

using namespace std;

struct {
    double price;
    vector<int> child;
} node[100005];

int main() {
    int n;
    double r;
    cin >> n >> node[0].price >> r;

    r = 1 + r / 100;

    for (int i = 0; i < n; ++i) {
        int k;
        cin >> k;
        for (int j = 0; j < k; ++j) {
            int c;
            cin >> c;
            node[i].child.push_back(c);
        }
    }

    queue<int> q;
    q.push(0);

    int cnt = -1;
    double minPrice = -1;
    while (not q.empty()) {
        int top = q.front();
        q.pop();

        vector<int>& child = node[top].child;
        if (child.size() == 0) {  // 找到了一个零售商
            if (cnt == -1) {
                minPrice = node[top].price;
                cnt = 1;
            }
            else if (minPrice == node[top].price) ++cnt;
            else break;
        }
        else
            for (int i = 0, size = child.size(); i < size; ++i) {
                node[child[i]].price = node[top].price * r;
                q.push(child[i]);
            }
    }

    cout << fixed << setprecision(4) << minPrice << " " << cnt;
}
```