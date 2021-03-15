Being unique is so important to people on Mars that even their lottery is designed in a unique way. The rule of winning is simple: ==one bets on a number chosen from $[1,10^4]$. The first one who bets on a unique number wins==. For example, if there are 7 people betting on { 5 31 5 88 67 88 17 }, then the second one who bets on 31 wins.
### Input Specification:
Each input file contains one test case. Each case contains a line which begins with ==a positive integer $N (≤10^5)$ and then followed by $N$ bets==. The numbers are separated by a space.
### Output Specification:
For each test case, ==print the winning number in a line==. If there is no winner, print `None` instead.

### Sample Input 1:
```
7 5 31 5 88 67 88 17
```
### Sample Output 1:
```
31
```
### Sample Input 2:
```
5 888 666 666 888 888
```
### Sample Output 2:
```
None
```
# 题解

给出一列数，输出第一个只出现过一次的数，如果没有则输出None。



将数据读入时用hash数组记录出现的次数，然后遍历一遍输出第一个出现一次的数。
```cpp
#include <cstdio>

int h[10005];
int a[100005];

int main() {

    int n;
    scanf("%d", &n);

    for (int i = 0; i < n; ++i) {
        scanf("%d", &a[i]);
        h[a[i]]++;
    }

    for (int i = 0; i < n; ++i) {
        if (h[a[i]] == 1) {
            printf("%d", a[i]);
            return 0;
        }
    }

    printf("None");
}
```