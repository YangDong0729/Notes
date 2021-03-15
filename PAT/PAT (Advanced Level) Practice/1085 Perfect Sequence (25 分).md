==Given a sequence of positive integers and another positive integer $p$. The sequence is said to be a **perfect sequence​** if $M≤m×p$ where $M$ and $m$ are the maximum and minimum numbers in the sequence, respectively.==

==Now given a sequence and a parameter $p$, you are supposed to find from the sequence as many numbers as possible to form a perfect subsequence.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains two positive integers $N$ and $p$, where $N (≤10^5)$ is the number of integers in the sequence, and $p (≤10^9)$ is the parameter. In the second line there are $N$ positive integers, each is no greater than $10^9$.
### Output Specification:
For each test case, print in one line the maximum number of integers that can be chosen to form a perfect subsequence.
### Sample Input:
```
10 8
2 3 20 4 5 1 6 7 8 9
```
### Sample Output:

```
8
```
# 题解

给一组数，从这些数中选出一些数，使其最大值不超过最小值的p倍。求所有选法中含有数字最多的选法所含的数字的个数。



## 二分查找

排序后遍历，对于每一个数，把它当成最小值，用二分查找的方式寻找满足条件的最大值，用这个选法中所含数字的个数更新答案。

```cpp
#include <algorithm>
#include <cstdio>

using namespace std;

long long a[100005];

int main() {
    int n, p;
    scanf("%d %d", &n, &p);
    for (int i = 0; i < n; ++i)
        scanf("%lld", &a[i]);

    sort(a, a + n);

    int r = 0;
    for (int i = 0; i < n; ++i)
        r = max(r, int(upper_bound(a, a + n, a[i] * p) - (a + i)));
    printf("%d", r);
}
```

## 双指针

排序后将两个指针i，j都放到0的位置，先右移j，直到不满足条件；再右移i，直到满足条件；如此往复，记录出现过的最大值。

```cpp
#include <algorithm>
#include <cstdio>

using namespace std;

long long a[100005];

int main() {
    int n, p;
    scanf("%d %d", &n, &p);
    for (int i = 0; i < n; ++i)
        scanf("%lld", &a[i]);

    sort(a, a + n);

    int r = 0;
    for (int i = 0, j = 0; j < n;) {
        while (j < n and a[i] * p >= a[j])
            ++j;
        r = max(r, j - i);
        while (j < n and a[i] * p < a[j])
            ++i;
    }

    printf("%d", r);
}
```

**注意：所有的数字都小于10^9，但是每个数字的p倍可能会超过int的范围。**

