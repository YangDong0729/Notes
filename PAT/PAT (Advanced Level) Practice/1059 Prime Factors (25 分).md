# 题目详情

==Given any positive integer $N$, you are supposed to find all of its prime factors, and write them in the format $N=p_1^{k_1}×p_2^{k_2}×⋯×p_m^{k_m}$==.

### Input Specification:
Each input file contains one test case which gives a positive integer $N$ in the range of **long int**.

### Output Specification:

Factor N in the format $N$ `=` $p_1$`^`$k_1$`*`$p_2$`^`$k_2$`*`$…$`*`$p_m$`^`$k_m$, where $p_i$'s are prime factors of $N$ in increasing order, and the exponent $k_i$ is the number of $p_i$ -- hence when there is only one $p_i$, $k_i$ is 1 and must **NOT** be printed out.

### Sample Input:
```
97532468
```
### Sample Output:
```
97532468=2^2*11*17*101*1291
```
# 题解

将一个数进行质因数分解。



首先要算质数，题中的数据范围是long int，也就是$2^{32}$。由于$2^{16}=65536$，所以只需要算出100000以内的全部质数即可。得到质数后用这些质数中能整除的去除x，直到x=1。

有可能全部的质数都除过后x还是不为1，这时的x是一个很大的质数。但是数据很弱，不考虑这种情况也能过。

注意边界情况x=1。
```cpp
#include <iostream>

using namespace std;
#define MAX_N 100000

bool not_prime[MAX_N];

int main() {
    int x;
    cin >> x;
    cout << x << "=";
    if (x == 1)
        cout << "1";

    for (int i = 2; i < MAX_N; ++i)  // 欧拉筛
        if (!not_prime[i])
            for (int j = i + i; not_prime[j / i]; j += i)
                not_prime[j] = true;

    bool first = true;
    for (int i = 2; i < MAX_N and x != 1; ++i) {
        if (!not_prime[i]) {
            int cnt = 0;
            while (x % i == 0) {
                x /= i;
                ++cnt;
            }
            if (cnt > 0) {
                if (first)
                    first = false;
                else
                    cout << '*';
                cout << i;
                if (cnt > 1)
                    cout << '^' << cnt;
            }
        }
    }
    if (x != 1)
        cout << '*' << x;
}

```