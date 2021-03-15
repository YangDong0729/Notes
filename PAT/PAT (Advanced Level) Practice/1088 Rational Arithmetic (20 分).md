# 题目详情
==For two rational numbers, your task is to implement the basic arithmetics, that is, to calculate their sum, difference, product and quotient.==

### Input Specification:

Each input file contains one test case, which gives in one line the two rational numbers in the format `a1/b1 a2/b2`. ==The numerators and the denominators are all in the range of long int==. ==If there is a negative sign, it must appear only in front of the numerator==. The denominators are guaranteed to be non-zero numbers.

### Output Specification:

For each test case, print in ==4 lines the sum, difference, product and quotient of the two rational numbers==, respectively. The format of each line is `number1 operator number2 = result`. Notice that ==all the rational numbers must be in their simplest form `k a/b`, where `k` is the integer part, and `a/b` is the simplest fraction part==. ==If the number is negative, it must be included in a pair of parentheses. If the denominator in the division is zero, output `Inf` as the result. It is guaranteed that all the output integers are in the range of **long int**.==

### Sample Input 1:

    2/3 -4/2


### Sample Output 1:

    2/3 + (-2) = (-1 1/3)
    2/3 - (-2) = 2 2/3
    2/3 * (-2) = (-1 1/3)
    2/3 / (-2) = (-1/3)


### Sample Input 2:

    5/3 0/6


### Sample Output 2:

    1 2/3 + 0 = 1 2/3
    1 2/3 - 0 = 1 2/3
    1 2/3 * 0 = 0
    1 2/3 / 0 = Inf
# 题解

实现分数的四则运算。



分数的四则运算很好实现，只要完成假分数的化简即可。

```cpp
#include <cmath>
#include <iostream>
#include <string>

using namespace std;
typedef long long ll;

ll gcd(ll a, ll b) { // 计算两个正数的最大公约数
    if (b == 0)
        return a;
    else
        return gcd(b, a % b);
}

string reduction(ll a, ll b) { // 化简一个假分数
    string ret;
    bool neg = false;

    if (a == 0)
        return "0";

    if ((a ^ b) < 0) { // 分数为负
        neg = true;
        ret += "(-";
        a = abs(a);
        b = abs(b);
    }

    int g = gcd(a, b); // 约分
    a /= g;
    b /= g;

    ll k = a / b;
    if (k != 0) {
        ret += to_string(k);
        if ((a %= b) != 0)
            ret += " ";
    }

    if (a != 0) {
        ret += to_string(a) + "/" + to_string(b);
    }

    if (neg)
        ret += ")";
    return ret;
}

int main() {
    ll a1, b1, a2, b2;
    scanf("%lld/%lld %lld/%lld", &a1, &b1, &a2, &b2);

    ll g = gcd(abs(a1), abs(b1));
    a1 /= g;
    b1 /= g;
    g = gcd(abs(a2), abs(b2));
    a2 /= g;
    b2 /= g;

    string out1 = reduction(a1, b1), out2 = reduction(a2, b2);
    cout << out1 << " + " << out2 << " = " << reduction(a1 * b2 + a2 * b1, b1 * b2) << "\n";
    cout << out1 << " - " << out2 << " = " << reduction(a1 * b2 - a2 * b1, b1 * b2) << "\n";
    cout << out1 << " * " << out2 << " = " << reduction(a1 * a2, b1 * b2) << "\n";
    cout << out1 << " / " << out2 << " = " << (out2 != "0" ? reduction(a1 * b2, a2 * b1) : "Inf") << "\n";
}
```

