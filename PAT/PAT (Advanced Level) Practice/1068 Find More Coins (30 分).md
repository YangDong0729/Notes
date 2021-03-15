Eva loves to collect coins from all over the universe, including some other planets like Mars. One day she visited a universal shopping mall which could accept all kinds of coins as payments. However, there was a special requirement of the payment: ==for each bill, she must pay the exact amount. Since she has as many as $10^4$ coins with her, she definitely needs your help. You are supposed to tell her, for any given amount of money, whether or not she can find some coins to pay for it.==
### Input Specification:
Each input file contains one test case. For each case, the first line contains ==2 positive numbers: `N` ($≤10^4$), the total number of coins) and `M` ($≤10^2$, the amount of money Eva has to pay).== The second line contains `N` face values of the coins, which are all positive numbers. All the numbers in a line are separated by a space.
### Output Specification:
For each test case, print in one line the face values $V_1≤V_2≤⋯≤V_k$ such that $V_1+V_2+⋯+V_k=$`M`. All the numbers must be separated by a space, and there must be no extra space at the end of the line. If such a solution is not unique, output the smallest sequence. If there is no solution, output "No Solution" instead.

==Note: sequence {A[1], A[2], ...} is said to be "smaller" than sequence {B[1], B[2], ...} if there exists $k≥1$ such that A[$i$]=B[$i$] for all $i<k$, and A[$k$] < B[$k$].==

### Sample Input 1:

```
8 9
5 9 8 7 2 3 4 1
```
### Sample Output 1:
```
1 3 5
```
### Sample Input 2:
```
4 8
7 2 4 3
```
### Sample Output 2:
```
No Solution
```
# 题解

给出N枚硬币，判断是否存在一种组合能够恰好支付M元，如果存在输出最小的序列。



01背包动态规划。注意`dp[v] == dp[v - w[i]] + w[i]`的时候也要更新选法，因为这个序列一定更小。另外还要记录选法，以便还原出序列。
### 代码
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <algorithm>
#include <cstdio>
#include <functional>

using namespace std;

const int max_n = 10010;
const int max_v = 110;

int w[max_n], dp[max_v]; // w为钱币的价值
bool choice[max_n][max_v];

int main() {
    int n, m;
    scanf("%d%d", &n, &m);

    for (int i = 1; i <= n; ++i)
        scanf("%d", &w[i]);

    sort(w + 1, w + 1 + n, greater<int>()); // 从大到小排列

    for (int i = 1; i <= n; ++i) {  // 每一轮多考虑一件物品
        for (int v = m; v >= w[i]; v--) {  // 从后到前，因为要用到上一轮的结果
            if (dp[v] <= dp[v - w[i]] + w[i]) { // 等于时也要放，因为这个序列一定更小
                dp[v] = dp[v - w[i]] + w[i];
                choice[i][v] = true; // 在大小为v的背包中放入第i件物品
            }
        }
    }

    if (dp[m] != m) printf("No Solution");
    else {
        for (; n and m; --n) {
            if (choice[n][m]) {
                printf("%d", w[n]);
                if (m -= w[n]) printf(" ");
            }
        }
    }
}
```