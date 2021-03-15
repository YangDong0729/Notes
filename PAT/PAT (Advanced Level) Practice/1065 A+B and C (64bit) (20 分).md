==Given three integers $A$, $B$ and $C$ in $[−2^{63},2^{63}]$, you are supposed to tell whether $A+B>C$.==

### Input Specification:
The first line of the input gives the positive number of test cases, $T (≤10)$. Then $T$ test cases follow, each consists of a single line containing three integers $A$, $B$ and $C$, separated by single spaces.
### Output Specification:
For each test case, output in one line `Case #X: true` if $A+B>C$, or `Case #X: false` otherwise, where X is the case number (starting from 1).
### Sample Input:
```
3
1 2 3
2 3 4
9223372036854775807 -9223372036854775808 0
```
### Sample Output:

```
Case #1: false
Case #2: true
Case #3: false
```
# 题解

给出三个数A，B和C，判断A+B和C的大小，A、B和C在$[−2^{63},2^{63}]$内。



关键就是 C 也在$[−2^{63},2^{63}]$内，所以用 long long 存储 A 和 B ，如果 A+B 溢出，再根据A和B的符号就可以确定 A+B 和 C 的大小关系。



这道题有点问题，因为 long long 不包括 $2^{63}$ 。
