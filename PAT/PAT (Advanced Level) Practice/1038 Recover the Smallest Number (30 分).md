Given a collection of number segments, you are supposed to recover the smallest number from them. For example, given { 32, 321, 3214, 0229, 87 }, we can recover many numbers such like 32-321-3214-0229-87 or 0229-32-87-321-3214 with respect to different orders of combinations of these segments, and the smallest number is 0229-321-3214-32-87.
### Input Specification:
Each input file contains one test case. Each case gives ==a positive integer $N (≤10^4)$ followed by $N$ number segments==. Each segment contains a non-negative integer of no more than 8 digits. All the numbers in a line are separated by a space.
### Output Specification:
For each test case, print the smallest number in one line. ==Notice that the first digit must not be zero.==
### Sample Input:
```
5 32 321 3214 0229 87
```
### Sample Output:
```
22932132143287
```
### 题意
给一些数，把他们首尾相接拼在一起可以组成一个数，求这个数的最小值。
### 思路
按照字符串输入。为了拼接后的数尽可能小，一定要让尽量小的数放在前面，所以排序，比较函数是`x + y < y + x`。然后按照排序的顺序拼接在一起。
边界情况：

1. 结果前面可能有0，不能输出，需要跳过。
2. 结果有可能就是0，要保证输出0。

### 代码

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

string a[10005];

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; ++i) cin >> a[i];
    sort(a, a + n, [](string &x, string &y) {
        return x + y < y + x;
    });

    string s;
    for (int i = 0; i < n; ++i) s += a[i];

    int l = 0, size = s.length();
    for (; l < size; ++l) if (s[l] != '0') break;

    if (l == size) cout << "0";
    else cout << s.substr(l);
}
```