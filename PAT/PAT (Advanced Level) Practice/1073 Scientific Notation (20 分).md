==Scientific notation is the way that scientists easily handle very large numbers or very small numbers. The notation matches the regular expression `[+-][1-9].[0-9]+E[+-][0-9]+` which means that the integer portion has exactly one digit, there is at least one digit in the fractional portion, and the number and its exponent's signs are always provided even when they are positive.==

Now given a real number $A$ in scientific notation, you are supposed to print $A$ in the conventional notation while keeping all the significant figures.
### Input Specification:
Each input contains one test case. For each case, there is one line containing the real number $A$ in scientific notation. ==The number is no more than 9999 bytes in length and the exponent's absolute value is no more than 9999.==
### Output Specification:
For each test case, print in one line the input number $A$ in the conventional notation, with all the significant figures kept, including trailing zeros.
### Sample Input 1:
```
+1.23400E-03
```
### Sample Output 1:
```
0.00123400
```
### Sample Input 2:
```
-1.2E+10
```
### Sample Output 2:
```
-12000000000
```
# 题解

给出一个用科学计数法表示的数，输出这个数的普通形式，所有有意义的位都要保留，包括前后的0。



先找到字符E的位置，然后就能获得获得阶和小数部分。题中的小数是定点小数，所以最终小数点的位置由阶确定。如果阶是正数，那就先输出小数部分，再输出0；否则先输出0，再输出小数部分。
```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string s;
    cin >> s;

    if (s[0] == '-') cout << '-';

    int e = s.find("E");
    string base = s.substr(1, e - 1);
    int exp = stoi(s.substr(e + 1));

    if (exp >= 0) {
        cout << base[0];
        for (int i = 0; i + 2 < base.length() or i < exp; ++i) {
            if (i == exp) cout << '.';
            if (i + 2 < base.length()) cout << base[i + 2];
            else cout << '0';
        }
    }
    else if (exp < 0) {
        cout << "0.";
        for (int i = 1; i < -exp; ++i)
            cout << '0';
        for (auto& e : base)
            if (e != '.') cout << e;
    }
}
```