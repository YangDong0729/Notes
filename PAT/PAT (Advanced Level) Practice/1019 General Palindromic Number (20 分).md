# 题目详情
A number that will be the same when it is written forwards or backwards is known as a **Palindromic Number**. For example, 1234321 is a palindromic number. All single digit numbers are palindromic numbers.

Although palindromic numbers are most often considered in the decimal system, the concept of palindromicity can be applied to the natural numbers in any numeral system. Consider a number $N > 0$ in base $b≥2$, where it is written in standard notation with $k+1$ digits $a_i$ as $\sum_{i=0}^k (a_ib^i)$. Here, as usual, $0 \le a_i < b$ for all $i$ and $a_k$ is non-zero. Then $N$ is palindromic if and only if $a_i = a_{k-i}$ for all $i$. Zero is written 0 in any base and is also palindromic by definition.

Given any positive decimal integer NN and a base bb, you are supposed to tell if NN is a palindromic number in base bb.

### Input Specification:

Each input file contains one test case. Each case consists of ==two positive numbers $N$ and $b$, where $0 < N \le 10^9$ is the decimal number and $2 \le b \le 10^9$ is the base==. The numbers are separated by a space.

### Output Specification:

For each test case, first print in one line `Yes` if $N$ is a palindromic number in base $b$, or `No` if not. Then in the next line, print $N$ as the number in base$b$ in the form "$a_k a_{k-1} ... a_0$". Notice that there must be no extra space at the end of output.

### Sample Input 1:

    27 2


### Sample Output 1:

    Yes
    1 1 0 1 1


### Sample Input 2:

    121 5


### Sample Output 2:

    No
    4 4 1


**鸣谢网友“CCPC拿不到牌不改名”修正数据！**

# 题解

给出两个数，计算a在b进制下是否是回文数。

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n, b;
    cin >> n >> b;

    vector<int> num;
    for (int i = 0; n != 0; ++i, n /= b) num.push_back(n % b);
    vector<int> front(num.begin(), num.begin() + num.size() / 2);
    vector<int> back(num.rbegin(), num.rbegin() + num.size() / 2);

    cout << (front == back ? "Yes\n" : "No\n");
    for (auto i = num.rbegin(); i != num.rend(); ++i)
        cout << *i << (i + 1 == num.rend() ? "" : " ");
}
```

