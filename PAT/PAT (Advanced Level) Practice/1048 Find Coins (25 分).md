Eva loves to collect coins from all over the universe, including some other planets like Mars. One day she visited a universal shopping mall which could accept all kinds of coins as payments. However, there was a special requirement of the payment: ==for each bill, she could only use exactly two coins to pay the exact amount. Since she has as many as $10^5$  coins with her, she definitely needs your help. You are supposed to tell her, for any given amount of money, whether or not she can find two coins to pay for it.==

### Input Specification:
  Each input file contains one test case. For each case, the first line contains ==2 positive numbers: $N (≤10^5$, the total number of coins$)$ and $M (≤10^3$, the amount of money Eva has to pay$)$.== ==The second line contains $N$ face values of the coins, which are all positive numbers no more than 500. All the numbers in a line are separated by a space.==
### Output Specification:
For each test case, print in one line the two face values $V_1$ and $V_2$ (separated by a space) such that $V_1+V_2=M$ and $V_1≤V_2$. If such a solution is not unique, output the one with the smallest $V_1$. If there is no solution, output `No Solution` instead.
### Sample Input 1:

```in
8 15
1 2 8 7 2 4 11 15
```

### Sample Output 1:

```out
4 11
```

### Sample Input 2:

```in
7 14
1 8 7 2 4 11 15
```

### Sample Output 2:

```out
No Solution
```

# 题解

给出一些数，找出一对数字，使得其和等于给定的值，且第一个数不大于第二个数。



散列法：用散列表记录每个数出现的次数。
二分查找：排序后用二分法查找。
双指针：排序后用双指针分别指向最左和最右，不断缩小范围直到满足题意。
### 散列法

```cpp
#include <cstdio>
#include <algorithm>

using namespace std;

int HashTable[1005];

int main () {
    int n, m, a;

    scanf("%d %d", &n, &m);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &a);
        ++HashTable[a];
    }

    for (int i = 1; i < m; ++i) {
        if (HashTable[i] and HashTable[m - i]) {
            if (i == m - i and HashTable[i] <= 1)
                continue;

            printf("%d %d\n", i, m - i);
            return 0;
        }
    }
    printf("No Solution\n");
}
```
### 二分查找

```cpp
#include <cstdio>
#include <algorithm>

using namespace std;

int a[100005];

int main() {
    int n, m;

    scanf("%d %d\n", &n, &m);
    for (int i = 0; i < n; ++i)
        scanf("%d", a + i);

    sort(a, a + n);

    for (int j = 0; j < n; ++j) {
        if (j + 1 < n
            and binary_search(a + j + 1, a + n, m - a[j])) {
            printf("%d %d\n", a[j], m - a[j]);
            return 0;
        }
    }
    printf("No Solution\n");
}
```
### 双指针

```cpp
#include <cstdio>
#include <algorithm>

using namespace std;

int a[100010];

int main () {
    int n, m;

    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; ++i)
        scanf("%d", &a[i]);

    sort(a, a + n);

    int i = 0, j = n - 1;
    while (i < j) {
        if (a[i] + a[j] == m) break;  // 找到a[i]与a[j]的和为m，退出循环
        else if (a[i] + a[j] < m) ++i;
        else --j;
    }
    if (i < j) printf("%d %d\n", a[i], a[j]);
    else printf("No Solution\n");
}
```