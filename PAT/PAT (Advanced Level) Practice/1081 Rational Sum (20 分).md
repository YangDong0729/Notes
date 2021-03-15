==Given $N$ rational numbers in the form `numerator/denominator`, you are supposed to calculate their sum.==

### Input Specification:
Each input file contains one test case. Each case starts with a positive integer $N (≤100)$, followed in the next line $N$ rational numbers `a1/b1 a2/b2 ...` where all the numerators and denominators are in the range of **long int**. If there is a negative number, then the sign must appear in front of the numerator.

Output Specification:
For each test case, output the sum in the simplest form `integer numerator/denominator` where `integer` is the integer part of the sum, `numerator` < `denominator`, and the numerator and the denominator have no common factor. You must output only the fractional part if the integer part is 0.
### Sample Input 1:
```
5
2/5 4/15 1/30 -2/60 8/3
```
### Sample Output 1:
```
3 1/3
```
### Sample Input 2:
```
2
4/3 2/3
```
### Sample Output 2:
```
2
```
### Sample Input 3:
```
3
1/3 -1/6 1/8
```
### Sample Output 3:
```
7/24
```
# 题解

给出n个分数，计算它们的和，结果以假分数的形式表示。



实现分数的加法和约分。注意题中已经说明，如果有负号则一定在分子上。
```cpp
#include <algorithm>
#include <cstdio>

using namespace std;

int gcd(int a, int b) {
    if (b == 0)
        return a;
    else
        return gcd(b, a % b);
}

struct fraction {
    int up, down;
};

fraction reduction(fraction &result) {
    int d = gcd(abs(result.up), abs(result.down));
    result.up /= d;
    result.down /= d;
    return result;
}

fraction add(fraction &f1, fraction &f2) {
    fraction result;
    result.up = f1.up * f2.down + f2.up * f1.down;
    result.down = f1.down * f2.down;
    return reduction(result);
}

int main() {
    int n;
    scanf("%d", &n);
    fraction sum{0, 1}, temp;
    for (int i = 0; i < n; ++i) {
        scanf("%d/%d", &temp.up, &temp.down);
        sum = add(sum, temp);
    }
    if (sum.down == 1)
        printf("%d\n", sum.up);     // 整数
    else if (abs(sum.up) > sum.down) { // 假分数
        printf("%d %d/%d\n", sum.up / sum.down, abs(sum.up) % sum.down, sum.down);
    } else { // 真分数
        printf("%d/%d\n", sum.up, sum.down);
    }
}
```