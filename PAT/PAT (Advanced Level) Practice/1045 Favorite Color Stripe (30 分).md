Eva is trying to ==make her own color stripe out of a given one==. ==She would like to keep only her favorite colors in her favorite order by cutting off those unwanted pieces and sewing the remaining parts together to form her favorite color stripe.==

It is said that a normal human eye can distinguish about less than 200 different colors, so Eva's favorite colors are limited. However the original stripe could be very long, and Eva would like to have the remaining favorite stripe with the maximum length. So she needs your help to find her the best result.

Note that ==the solution might not be unique, but you only have to tell her the maximum length==. For example, given a stripe of colors {2 2 4 1 5 5 6 3 1 1 5 6}. If Eva's favorite colors are given in her favorite order as {2 3 1 5 6}, then she has 4 possible best solutions {2 2 1 1 1 5 6}, {2 2 1 5 5 5 6}, {2 2 1 5 5 6 6}, and {2 2 3 1 1 5 6}.

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==a positive integer $N (≤200)$ which is the total number of colors== involved (and hence the colors are numbered from 1 to $N$). Then ==the next line starts with a positive integer $M (≤200)$ followed by M Eva's favorite color numbers given in her favorite order.== Finally the third line starts with ==a positive integer $L (≤10^4)$ which is the length of the given stripe, followed by $L$ colors on the stripe==. All the numbers in a line a separated by a space.
### Output Specification:
For each test case, simply print in a line the maximum length of Eva's favorite stripe.
### Sample Input:
```
6
5 2 3 1 5 6
12 2 2 4 1 5 5 6 3 1 1 5 6
```
### Sample Output:
```
7
```
# 题解

给出两列数，其中第一列是没有重复的。在第二列数中选择最大的子序列，使得其每个数字出现的顺序与第一列数是相同的，可以重复。



如果第一列数是1 2 3 4的话，那么这个问题就是最长不下降子序列。可以对第一列数映射到1 2 3 4，使之转化为最长不下降子序列问题。
```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;

const int MAX_C = 210;  // 最大颜色数
const int MAX_N = 10010;  // 最大L

int hashTable[MAX_C];  // 将喜欢的颜色序列映射为递增序列，不喜欢的颜色映射为-1
int a[MAX_N], dp[MAX_N];

int main() {
    int n, m, x;
    scanf("%d%d", &n, &m);
    memset(hashTable, -1, sizeof(hashTable));

    for (int i = 0; i < m; ++i) {
        scanf("%d", &x);
        hashTable[x] = i;  // 将喜欢的颜色按顺序映射到递增序列0 1 2 ...
    }

    int l, num = 0;  // num存放颜色序列中喜欢的颜色的总数
    scanf("%d", &l);
    for (int i = 0; i < l; ++i) {
        scanf("%d", &x);
        if (hashTable[x] >= 0) a[num++] = hashTable[x];
    }

    // 以下全部为LIS问题的模板
    int ans = -1;
    for (int i = 0; i < num; ++i) {
        dp[i] = 1;
        for (int j = 0; j < i; ++j) {
            if (a[j] <= a[i] and dp[i] < dp[j] + 1)
                dp[i] = dp[j] + 1;
        }
        ans = max(ans, dp[i]);
    }

    printf("%d\n", ans);
}
```