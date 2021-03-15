Shopping in Mars is quite a different experience. ==The Mars people pay by chained diamonds==. Each diamond has a value (in Mars dollars M\$). When making the payment, the chain can be cut at any position for only once and some of the diamonds are taken off the chain one by one. Once a diamond is off the chain, it cannot be taken back. For example, if we have a chain of 8 diamonds with values M\$3, 2, 1, 5, 4, 6, 8, 7, and we must pay M$15. We may have 3 options:

1. Cut the chain between 4 and 6, and take off the diamonds from the position 1 to 5 (with values 3+2+1+5+4=15).
2. Cut before 5 or after 6, and take off the diamonds from the position 4 to 6 (with values 5+4+6=15).
3. Cut before 8, and take off the diamonds from the position 7 to 8 (with values 8+7=15).

==Now given the chain of diamond values and the amount that a customer has to pay, you are supposed to list all the paying options for the customer.==

==If it is impossible to pay the exact amount, you must suggest solutions with minimum lost.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==2 numbers: $N (≤10^5)$, the total number of diamonds on the chain, and $M (≤10^8)$, the amount that the customer has to pay==. Then the next line contains $N$ positive numbers $D_1⋯D_N(D_i≤10^3$ for all $i=1,⋯,N)$ which are the values of the diamonds. All the numbers in a line are separated by a space.
### Output Specification:
For each test case, print `i-j` in a line for each pair of $i ≤ j$ such that $D_i + ... + D_j = M$. ==Note that if there are more than one solution, all the solutions must be printed in increasing order of `i`.==

If there is no solution, output `i-j` for pairs of $i ≤ j$ such that $D_i + ... + D_j >M$ with $(D_i + ... + D_j −M)$ minimized. Again all the solutions must be printed in increasing order of `i`.

It is guaranteed that the total value of diamonds is sufficient to pay the given amount.
### Sample Input 1:
```
16 15
3 2 1 5 4 6 8 7 16 10 15 11 9 12 14 13
```
### Sample Output 1:
```
1-5
4-6
7-8
11-11
```
### Sample Input 2:
```
5 13
2 4 5 7 9
```
### Sample Output 2:
```
2-4
4-5
```
# 题解

找出所有的数字串，使其和为一个确定的值，如果没有则要求在和大于这个值的情况下尽可能接近。



基本想法是遍历所有可能的子串，通过剪枝使时间复杂度降到$O(n)$。在左边界固定的情况下，通过不断右移右边界，就可以考察一组子串。一旦发现了一个有可能成为答案的子串（和大于M），右边界再右移一定不是答案（因为和更大）。这时就应该将左边界右移，考察下一组子串。
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <cstdio>
#include <climits>

using namespace std;

int ans[100005][2];
int diamond[100005];

int main() {
    int n, m;

    scanf("%d %d\n", &n, &m);
    for (int i = 1; i <= n; ++i)
        scanf("%d", diamond + i);

    int cnt = 0;
    for (int sum = 0, l = 1, r = 0, cost = INT_MAX;;) {
        while (sum < m and r <= n) sum += diamond[++r];
        if (sum < m) break;
        if (cost > sum - m) {
            cnt = 1;
            cost = sum - m;
            ans[0][0] = l;
            ans[0][1] = r;
        } else if (cost == sum - m) {
            ans[cnt][0] = l;
            ans[cnt][1] = r;
            ++cnt;
        }
        do {
            sum -= diamond[l++];
        } while (l <= r and sum - diamond[l] >= m);
    }

    for (int i = 0; i < cnt; ++i)
        printf("%d-%d\n", ans[i][0], ans[i][1]);
}
```