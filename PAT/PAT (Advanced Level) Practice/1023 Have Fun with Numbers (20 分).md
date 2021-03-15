# 题目详情
Notice that the number 123456789 is a 9-digit number consisting exactly the numbers from 1 to 9, with no duplication. Double it we will obtain 246913578, which happens to be another 9-digit number consisting exactly the numbers from 1 to 9, only in a different permutation. Check to see the result if we double it again!

Now you are suppose to check if there are more numbers with this property. That is, double a given number with $k$ digits, you are to tell if the resulting number consists of only a permutation of the digits in the original number.

### Input Specification:

Each input contains one test case. Each case contains ==one positive integer with no more than 20 digits.==

### Output Specification:

For each test case, ==first print in a line "Yes" if doubling the input number gives a number that consists of only a permutation of the digits in the original number, or "No" if not.== Then in the next line, print the doubled number.

### Sample Input:

    1234567899


### Sample Output:

    Yes
    2469135798

# 题解

判断一个整数乘2后的值是否是原来的数的各位数字的一个排列。

### 代码

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int cnt[500];

int main() {
    string s1, s2, s3;
    cin >> s1;
    s2 = s1;

    int carry = 0;
    for (auto i = s2.rbegin(); i != s2.rend(); ++i) {
        int d = (*i - '0') * 2 + carry;
        *i = d % 10 + '0';
        carry = d / 10;
    }
    if (carry) s2.insert(s2.begin(), carry + '0');

    s3 = s2;
    sort(s1.begin(), s1.end());
    sort(s3.begin(), s3.end());

    cout << (s1 == s3 ? "Yes" : "No") << "\n" << s2;
}
```

