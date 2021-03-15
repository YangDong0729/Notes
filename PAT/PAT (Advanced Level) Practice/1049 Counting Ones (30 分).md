The task is simple: ==given any positive integer $N$, you are supposed to count the total number of 1's in the decimal form of the integers from 1 to $N$==. For example, given $N$ being 12, there are five 1's in 1, 10, 11, and 12.

### Input Specification:

Each input file contains one test case which gives the positive $N (≤2^{30})$.

### Output Specification:
For each test case, print the number of 1's in one line.

### Sample Input:
```
12
```
### Sample Output:
```
5
```
# 题解

计算0-n中出现的"1"的次数。



对n的每一位分别计算出现"1"的次数，left为高位组成的数，right为低位组成的数， base为这位的权（10^(n - i)）
当这位为0时，left * base；（高位为0 ~ left - 1时，这位可能出现1）
当这位为1时，left * base + right + 1 （高位为0 ~ left - 1时，这位可能出现1；当高位为left，且低位为0 ~ right时，这位可以是1）
当这位大于1时，(left + 1) * base （高位为0 ~ left时，这位可能出现1）

```cpp
#include <cstdio>

int main() {
    int n, a = 1, ans = 0;
    int left, now, right;
    scanf("%d", &n);

    while (n / a != 0) {
        left = n / (a * 10);
        now = n / a % 10;
        right = n % a;

        if (now == 0)
            ans += left * a;
        else if (now == 1)
            ans += left * a + right + 1;
        else
            ans += (left + 1) * a;

        a *= 10;
    }
    printf("%d\n", ans);
}
```