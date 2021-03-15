# 题目详情
"Damn Single (单身狗)" is the Chinese nickname for someone who is being single. ==You are supposed to find those who are alone in a big party==, so they can be taken care of.

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer N ($\le$ 50,000), the total number of couples==. Then N lines of the couples follow, each gives a couple of ID's which are 5-digit numbers (i.e. from 00000 to 99999). After the list of couples, there is ==a positive integer M ($\le$ 10,000) followed by M ID's of the party guests==. The numbers are separated by spaces. It is guaranteed that nobody is having bigamous marriage (重婚) or dangling with more than one companion.

### Output Specification:

==First print in a line the total number of lonely guests==. ==Then in the next line, print their ID's in increasing order==. The numbers must be separated by exactly 1 space, and there must be no extra space at the end of the line.

### Sample Input:

    3
    11111 22222
    33333 44444
    55555 66666
    7
    55555 44444 10000 88888 22222 11111 23333


### Sample Output:

    5
    10000 23333 44444 55555 88888
# 题解

```cpp
#include <iostream>
#include <unordered_map>
#include <unordered_set>
#include <vector>
#include <algorithm>
#include <iomanip>

using namespace std;

unordered_set<int> guest;
vector<int> single;
unordered_map<int, int> couple;

int main() {
    int n, a, b;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> a >> b;
        couple[a] = b;
        couple[b] = a;
    }

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> a;
        guest.insert(a);
    }

    for (auto& i : guest) {
        if (not guest.count(couple[i]))  // map的count的用法
            single.push_back(i);
    }

    sort(single.begin(), single.end());

    cout << single.size() << "\n";
    for (auto i = single.begin(); i != single.end(); ++i)
        cout << setw(5) << setfill('0') << *i << (i + 1 == single.end() ? "\n" : " ");
}
```

