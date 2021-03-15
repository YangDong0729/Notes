==For any 4-digit integer except the ones with all the digits being the same, if we sort the digits in non-increasing order first, and then in non-decreasing order, a new number can be obtained by taking the second number from the first one. Repeat in this manner we will soon end up at the number `6174` -- the black hole of 4-digit numbers. This number is named Kaprekar Constant.==

For example, start from `6767`, we'll get:
```
7766 - 6677 = 1089
9810 - 0189 = 9621
9621 - 1269 = 8352
8532 - 2358 = 6174
7641 - 1467 = 6174
... ...
```
==Given any 4-digit number, you are supposed to illustrate the way it gets into the black hole.==

### Input Specification:

Each input file contains one test case which gives a positive integer N in the range $(0,10^4)$.
### Output Specification:

==If all the 4 digits of N are the same, print in one line the equation `N - N = 0000`. Else print each step of calculation in a line until `6174` comes out as the difference. All the numbers must be printed as 4-digit numbers.==

### Sample Input 1:
```
6767
```
### Sample Output 1:
```
7766 - 6677 = 1089
9810 - 0189 = 9621
9621 - 1269 = 8352
8532 - 2358 = 6174
```
### Sample Input 2:
```
2222
```
### Sample Output 2:
```
2222 - 2222 = 0000
```
# 题解

演示数字黑洞



**注意:**

1. 循环条件，输入为6147仍要进行运算，直到差值为6174。
2. 输入和输出都有可能不足4位，这时要使用`s.insert(0, 4 - s.length(), '0');`补0。
```cpp
#include <algorithm>
#include <string>
#include <iostream>

using namespace std;

int main() {
    string s;
    cin >> s;

    s.insert(0, 4 - s.length(), '0');

    do {
        string a = s, b = s;

        sort(a.begin(), a.end(), greater<int>());
        sort(b.begin(), b.end());

        int result = stoi(a) - stoi(b);

        s = to_string(result);
        s.insert(0, 4 - s.length(), '0');

        cout << a << " - " << b << " = " << s << '\n';
    } while (s != "6174" and s != "0000");
}
```