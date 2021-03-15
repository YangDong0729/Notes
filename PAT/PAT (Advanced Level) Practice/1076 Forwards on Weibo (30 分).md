Weibo is known as the Chinese version of Twitter. One user on Weibo may have many followers, and may follow many other users as well. Hence a social network is formed with followers relations. When a user makes a post on Weibo, all his/her followers can view and forward his/her post, which can then be forwarded again by their followers. ==Now given a social network, you are supposed to calculate the maximum potential amount of forwards for any specific user, assuming that only $L$ levels of indirect followers are counted.==
### Input Specification:
Each input file contains one test case. For each case, the first line contains ==2 positive integers: $N (≤1000)$, the number of users; and $L (≤6)$, the number of levels of indirect followers that are counted==. Hence it is assumed that all the users are numbered from 1 to $N$. Then $N$ lines follow, each in the format:
```
M[i] user_list[i]
```
==where `M[i]` $(≤100)$ is the total number of people that `user[i]` follows; and `user_list[i]` is a list of the `M[i]` users that followed by `user[i]`. It is guaranteed that no one can follow oneself. All the numbers are separated by a space.==

==Then finally a positive K is given, followed by K `UserID`'s for query.==

### Output Specification:

==For each `UserID`, you are supposed to print in one line the maximum potential amount of forwards this user can trigger, assuming that everyone who can view the initial post will forward it once, and that only $L$ levels of indirect followers are counted.==

### Sample Input:

```
7 3
3 2 3 4
0
2 5 6
2 3 1
2 3 4
1 4
1 5
2 2 6
```
### Sample Output:
```
4
5
```
# 题解

给出N个人的关注列表，和最大计算层数L，计算一个人发一条微博最多能被多少人转发。



根据输入建立有向图时，要从被关注者指向关注者，这是传播的方向。在进行BFS时要记录每个节点的层数，当节点层数达到最大值时不再入栈。
```cpp
#include <algorithm>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct Node {
    int level;
    vector<int> follower;
} node[1005];

bool visited[1005];

int bfs(int x, int maxLevel, int n) {
    fill_n(visited + 1, n, false);

    int cnt = 0;
    queue<int> q;
    q.push(x);
    visited[x] = true;
    node[x].level = 0;

    while (not q.empty()) {
        int f = q.front();
        q.pop();
        if (node[f].level < maxLevel) {
            for (auto &e : node[f].follower) {
                if (not visited[e]) {
                    ++cnt;
                    node[e].level = node[f].level + 1;
                    visited[e] = true;
                    q.push(e);
                }
            }
        }
    }

    return cnt;
}

int main() {
    int n, l, m, c, q, k;

    cin >> n >> l;
    for (int i = 1; i <= n; ++i) {
        cin >> m;
        for (int j = 0; j < m; ++j) {
            cin >> c;
            node[c].follower.push_back(i);
        }
    }

    cin >> k;
    for (int i = 0; i < k; ++i) {
        cin >> q;
        cout << bfs(q, l, n) << "\n";
    }
}
```