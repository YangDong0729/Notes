If you are a fan of Harry Potter, you would know the world of magic has its own currency system -- as Hagrid explained it to Harry, "==Seventeen silver Sickles to a Galleon and twenty-nine Knuts to a Sickle==, it's easy enough." ==Your job is to write a program to compute $A+B$ where $A$ and $B$ are given in the standard form of `Galleon.Sickle.Knut` (`Galleon` is an integer in $[0,10^7]$, `Sickle` is an integer in [0, 17), and `Knut` is an integer in [0, 29)).==

### Input Specification:
Each input file contains one test case which occupies a line with $A$ and $B$ in the standard form, separated by one space.

### Output Specification:
For each test case you should output the sum of $A$ and $B$ in one line, with the same format as the input.

### Sample Input:
```
3.2.1 10.16.27
```
### Sample Output:
```
14.1.28
```
### 题意
给出两组数代表一定的钱，求这两组钱的和。
### 代码
```cpp
#include <cstdio>

struct m {
    int g, s, k;
};

int main() {
    m a, b;

    scanf("%d.%d.%d %d.%d.%d",
          &a.g, &a.s, &a.k,
          &b.g, &b.s, &b.k);

    a.g += b.g;
    a.s += b.s;
    a.k += b.k;

    if (a.k >= 29) {
        a.s += a.k / 29;
        a.k %= 29;
    }

    if (a.s >= 17) {
        a.g += a.s / 17;
        a.s %= 17;
    }

    printf("%d.%d.%d", a.g, a.s, a.k);
}
```