Among all the factors of a positive integer N, there may exist several consecutive numbers. For example, 630 can be factored as 3×5×6×7, where 5, 6, and 7 are the three consecutive numbers. ==Now given any positive N, you are supposed to find the maximum number of consecutive factors, and list the smallest sequence of the consecutive factors.==
### Input Specification:

Each input file contains one test case, which gives the integer $N \ (1<N<2
^{31})$.

### Output Specification:

For each test case, print in the first line the maximum number of consecutive factors. Then in the second line, print the smallest sequence of the consecutive factors in the format `factor[1]*factor[2]*...*factor[k]`, where the factors are listed in increasing order, and ==1 is NOT included==.
### Sample Input:

```
630
```
### Sample Output:

```
3
5*6*7
```
# 题解

给出一个数，给出他的一个因子（或这个数本身），其值等于最多连续的数字的乘积。



暴力法
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <algorithm>
#include <cmath>
#include <iostream>

using namespace std;

int main() {
    int n;
    cin >> n;

    int first = n, len = 0;
    for (int l = 2, sqr = sqrt(n); l <= sqr; ++l) {
        for (int r = l, temp = r; n % temp == 0; temp *= ++r) {
            if (r - l + 1 > len) {
                first = l;
                len = r - l + 1;
            }
        }
    }

    if (len == 0) len = 1;  // len为0，说明这是一个质数
    cout << len << "\n";
    for (int i = 0; i < len; ++i)
        cout << first + i << (i == len - 1 ? "" : "*");
}
```
