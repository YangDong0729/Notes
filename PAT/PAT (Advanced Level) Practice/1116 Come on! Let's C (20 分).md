# 题目详情
"Let's C" is a popular and fun programming contest hosted by the College of Computer Science and Technology, Zhejiang University. Since the idea of the contest is for fun, the award rules are funny as the following:

*   0、 ==The Champion will receive a "Mystery Award"== (such as a BIG collection of students' research papers...).
*   1、 ==Those who ranked as a prime number will receive the best award== -- the Minions (小黄人)!
*   2、 ==Everyone else will receive chocolates==.

Given the final ranklist and a sequence of contestant ID's, you are supposed to tell the corresponding awards.

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer N ($\le 10^4$), the total number of contestants==. Then N lines of the ranklist follow, each in order gives a contestant's ID (a 4-digit number). After the ranklist, there is a positive integer K followed by K query ID's.

### Output Specification:

For each query, print in a line `ID: award` where the award is `Mystery Award`, or `Minion`, or `Chocolate`. If the ID is not in the ranklist, print `Are you kidding?` instead. If the ID has been checked before, print `ID: Checked`.

### Sample Input:

    6
    1111
    6666
    8888
    1234
    5555
    0001
    6
    8888
    0001
    1111
    2222
    8888
    2222


### Sample Output:

    8888: Minion
    0001: Chocolate
    1111: Mystery Award
    2222: Are you kidding?
    8888: Checked
    2222: Are you kidding?
# 题解

```cpp
#include <iostream>
#include <map>

using namespace std;

bool isPrime(int x) {
    if (x == 2) return true;
    if (x % 2 == 0) return false;
    for (int i = 3; i * i <= x; i += 2) {
        if (x % i == 0) {
            return false;
        }
    }
    return true;
}

map<int, int> mp, check;

int main() {
    int n, c;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> c;
        mp[c] = i + 1;
    }

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> c;
        printf("%04d: ", c);
        if (not mp.count(c)) cout << "Are you kidding?\n";
        else if (check[c]) cout << "Checked\n";
        else if (mp[c] == 1) cout << "Mystery Award\n";
        else if (isPrime(mp[c])) cout << "Minion\n";
        else cout << "Chocolate\n";
        check[c] = 1;
    }
}
```

