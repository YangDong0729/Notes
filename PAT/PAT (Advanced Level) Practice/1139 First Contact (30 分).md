# 题目详情
Unlike in nowadays, the way that boys and girls expressing their feelings of love was quite subtle in the early years. When a boy A had a crush on a girl B, he would usually not contact her directly in the first place. Instead, he might ask another boy C, one of his close friends, to ask another girl D, who was a friend of both B and C, to send a message to B -- quite a long shot, isn't it? Girls would do analogously.

==Here given a network of friendship relations, you are supposed to help a boy or a girl to list all their friends who can possibly help them making the first contact.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers N (1 $<$ N $\le$ 300) and M, being the total number of people and the number of friendship relations==, respectively. Then M lines follow, each gives a pair of friends. Here a person is represented by a 4-digit ID. To tell their genders, we use ==a negative sign to represent girls.==

After the relations, a positive integer K ($\le$ 100) is given, which is the number of queries. Then K lines of queries follow, each gives a pair of lovers, separated by a space. It is assumed that ==the first one is having a crush on the second one.==

### Output Specification:

For each query, ==first print in a line the number of different pairs of friends they can find to help them, then in each line print the IDs of a pair of friends.==

==If the lovers A and B are of opposite genders, you must first print the friend of A who is of the same gender of A, then the friend of B, who is of the same gender of B.== ==If they are of the same gender, then both friends must be in the same gender as theirs. It is guaranteed that each person has only one gender.==

The friends must be ==printed in non-decreasing order of the first IDs, and for the same first ones, in increasing order of the seconds ones.==

### Sample Input:

    10 18
    -2001 1001
    -2002 -2001
    1004 1001
    -2004 -2001
    -2003 1005
    1005 -2001
    1001 -2003
    1002 1001
    1002 -2004
    -2004 1001
    1003 -2002
    -2003 1003
    1004 -2002
    -2001 -2003
    1001 1003
    1003 -2001
    1002 -2001
    -2002 -2003
    5
    1001 -2001
    -2003 1001
    1005 -2001
    -2002 -2004
    1111 -2003


### Sample Output:

    4
    1002 2004
    1003 2002
    1003 2003
    1004 2002
    4
    2001 1002
    2001 1003
    2002 1003
    2002 1004
    0
    1
    2003 2001
    0
# 题解

题目本身思路很简单，两层循环找出满足题意的朋友即可。但是要注意有一个坑，女同学的ID前面加一个负号，**不代表整体是个负数**，比如说-0000，如果直接读入的话负号就没有了。

```cpp
#include <algorithm>
#include <iostream>
#include <set>
#include <string>
#include <unordered_map>
#include <unordered_set>

using namespace std;

unordered_map<int, int> gender;
unordered_map<int, unordered_set<int>> graph;

int main() {
    int n, m, o;

    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        string a, b;
        cin >> a >> b;

        int ida = abs(stoi(a));
        int idb = abs(stoi(b));

        graph[ida].insert(idb);
        graph[idb].insert(ida);

        gender[ida] = a[0] == '-';
        gender[idb] = b[0] == '-';
    }

    cin >> o;
    for (int i = 0; i < o; ++i) {
        int a, b;
        set<pair<int, int>> ans; // set 保证有序

        cin >> a >> b;

        a = abs(a);
        b = abs(b);

        for (auto &j : graph[a]) {
            if (j == a or j == b or gender[j] != gender[a])
                continue;
            for (auto &k : graph[j]) {
                if (k == a or k == b or gender[k] != gender[b] or !graph[k].count(b))
                    continue;
                ans.insert({j, k});
            }
        }

        cout << ans.size() << "\n";
        for (auto &e : ans) // 注意：输出格式（然而题中并没有写）
            printf("%04d %04d\n", e.first, e.second);
    }
}
```

