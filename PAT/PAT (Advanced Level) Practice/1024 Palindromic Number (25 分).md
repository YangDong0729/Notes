https://pintia.cn/problem-sets/994805342720868352/problems/994805346063728640

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
A number that will be the same when it is written forwards or backwards is known as a **Palindromic Number**. For example, 1234321 is a palindromic number. All single digit numbers are palindromic numbers.

==Non-palindromic numbers can be paired with palindromic ones via a series of operations. First, the non-palindromic number is reversed and the result is added to the original number. If the result is not a palindromic number, this is repeated until it gives a palindromic number.== For example, if we start from 67, we can obtain a palindromic number in 2 steps: 67 + 76 = 143, and 143 + 341 = 484.

Given any positive integer $N$, you are supposed to find its paired palindromic number and the number of steps taken to find it.

### Input Specification:

Each input file contains one test case. Each case consists of ==two positive numbers $N$ and $K$, where $N$ ($\le 10^{10}$) is the initial numer and $K$ ($\le 100$) is the maximum number of steps.== The numbers are separated by a space.

### Output Specification:

For each test case, output two numbers, one in each line. ==The first number is the paired palindromic number of $N$, and the second number is the number of steps taken to find the palindromic number.== ==If the palindromic number is not found after $K$ steps, just output the number obtained at the $K$th step and $K$ instead.==

### Sample Input 1:

    67 3


### Sample Output 1:

    484
    2


### Sample Input 2:

    69 3


### Sample Output 2:

    1353
    3
# 题解

```cpp
#include <algorithm>
#include <iostream>
#include <string>

using namespace std;

int main() {
    string s;
    int n, cnt = 0;

    cin >> s >> n;

    for (; cnt < n; ++cnt) {
        string t = s;
        reverse(t.begin(), t.end());

        if (t == s) break;

        int carry = 0;
        for (auto i = s.rbegin(), j = t.rbegin(); i != s.rend(); ++i, ++j) {
            int sum = *i + *j + carry - '0' * 2;
            *i = sum % 10 + '0';
            carry = sum / 10;
        }
        if (carry != 0) s.insert(s.begin(), carry + '0');
    }
    cout << s << "\n" << cnt;
}
```

