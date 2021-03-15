==If a machine can save only 3 significant digits, the float numbers 12300 and 12358.9 are considered equal since they are both saved as $0.123×10^5$ with simple chopping==. Now given the number of significant digits on a machine and two float numbers, you are supposed to tell if they are treated equal in that machine.

### Input Specification:

==Each input file contains one test case which gives three numbers $N$, $A$ and $B$, where $N (<100)$ is the number of significant digits, and $A$ and $B$ are the two float numbers to be compared. Each float number is non-negative, no greater than $10^{100}$, and that its total digit number is less than 100.==

### Output Specification:

For each test case, ==print in a line `YES` if the two numbers are treated equal, and then the number in the standard form `0.d[1]...d[N]*10^k` (`d[1]`>0 unless the number is 0); or `NO` if they are not treated equal, and then the two numbers in their standard form. All the terms must be separated by a space, with no extra space at the end of a line.==

Note: Simple chopping is assumed without rounding.

### Sample Input 1:

```
3 12300 12358.9
```
### Sample Output 1:
```
YES 0.123*10^5
```
### Sample Input 2:
```
3 120 128
```
### Sample Output 2:
```
NO 0.120*10^3 0.128*10^3
```
# 题解

给出两个数，输出保留N位的科学计数法。
注意题中说明保留有效数字是直接截断的（simple chopping），也就是ceil，不是四舍六入五成双。



题中的标准形式是`0.d[1]...d[N]*10^k`，记s=`d[1]...d[N]`，那么只需s和k都相等就可以断定这两个浮点数的标准形式相等。
```cpp
#include <iostream>
#include <string>

using namespace std;

int removeLeadingZero(string &s) {
    int i = 0;
    while (i < s.length() and s[i] == '0')
        ++i;
    s = s.substr(i);
    return i;
}

string deal(string a, int n, string &s, int &k) {
    k = 0;
    s = "";

    removeLeadingZero(a);
    if (a.length() > 0 and a[0] == '.') {
        a.erase(a.begin());
        k -= removeLeadingZero(a);
    } else {
        int i = 0;
        while (i < a.length() and a[i] != '.')
            ++i;
        k += i;
        if (i < a.length())
            a.erase(a.begin() + i);
    }
    if (a.length() == 0)
        k = 0;

    for (int i = 0, j = 0; i < n; ++i) {
        if (j < a.length())
            s += a[j++];
        else
            s += '0';
    }
    return s;
}

int main() {
    int k1, k2, n;
    string a, b, s1, s2;
    cin >> n >> a >> b;

    deal(a, n, s1, k1);
    deal(b, n, s2, k2);

    if (s1 == s2 and k1 == k2)
        cout << "YES 0." << s1 << "*10^" << k1 << '\n';
    else
        cout << "NO 0." << s1 << "*10^" << k1 << " 0." << s2 << "*10^" << k2 << '\n';
}
```