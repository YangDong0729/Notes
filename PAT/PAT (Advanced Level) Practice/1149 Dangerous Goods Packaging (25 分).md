# 题目详情
When shipping goods with containers, we have to be careful not to pack some incompatible goods into the same container, or we might get ourselves in serious trouble. For example, oxidizing agent （氧化剂） must not be packed with flammable liquid （易燃液体）, or it can cause explosion.

==Now you are given a long list of incompatible goods, and several lists of goods to be shipped. You are supposed to tell if all the goods in a list can be packed into the same container.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers: $N (≤10^4)$, the number of pairs of incompatible goods, and $M (≤100)$, the number of lists of goods to be shipped.==

Then two blocks follow. ==The first block contains N pairs of incompatible goods, each pair occupies a line;== and ==the second one contains M lists of goods to be shipped, each list occupies a line== in the following format:

    K G[1] G[2] ... G[K]


where `K` ($≤1,000$) is the number of goods and `G[i]`'s are the IDs of the goods. To make it simple, each good is represented by a 5-digit ID number. All the numbers in a line are separated by spaces.

### Output Specification:

For each shipping list, print in a line `Yes` if there are no incompatible goods in the list, or `No` if not.

### Sample Input:

    6 3
    20001 20002
    20003 20004
    20005 20006
    20003 20001
    20005 20004
    20004 20006
    4 00001 20004 00002 20003
    5 98823 20002 20003 20006 10010
    3 12345 67890 23333


### Sample Output:

    No
    Yes
    Yes
# 题解

用邻接表来表示物品之间的互斥关系。考察每一条边的两个端点是否同时出现在清单中，所以清单要用set存储。

```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
#include <unordered_map>

using namespace std;

int main() {
    int n, m, k, t, a, b;

    cin >> n >> m;
    unordered_map<int, vector<int>> adj;
    for (int i = 0; i < n; ++i) {
        cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }

    for (int i = 0; i < m; ++i) {
        cin >> k;

        unordered_set<int> list;
        for (int j = 0; j < k; ++j) {
            cin >> t;
            list.insert(t);
        }

        for (auto& u : list)
            for (auto& v : adj[u])
                if (list.count(v)) goto No;

        cout << "Yes\n";
        continue;
    No: 
        cout << "No\n";
    }
}
```

