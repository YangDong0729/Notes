# 题目详情
"Forever number" is a positive integer $A$ with $K$ digits, satisfying the following constrains:
+ the sum of all the digits of $A$ is $m$;
+ the sum of all the digits of $A+1$ is $n$; and
+ the greatest common divisor of $m$ and $n$ is a prime number which is greater than 2.

Now you are supposed to find these forever numbers.

### Input Specification:
Each input file contains one test case. For each test case, the first line contains a positive integer $N$ ($≤5$). Then $N$ lines follow, each gives a pair of $K$ ($3<K<10$) and $m$ ($1< m <90$), of which the meanings are given in the problem description.

### Output Specification:
For each pair of $K$ and $m$, first print in a line `Case X`, where `X` is the case index (starts from 1). Then print $n$ and $A$ in the following line. The numbers must be separated by a space. If the solution is not unique, output in the ascending order of $n$. If still not unique, output in the ascending order of $A$. If there is no solution, output `No Solution`.

### Sample Input:
```
2
6 45
7 80
```
### Sample Output:
```
Case 1
10 189999
10 279999
10 369999
10 459999
10 549999
10 639999
10 729999
10 819999
10 909999
Case 2
No Solution
```
### 题意
给出K和m。计算所有满足条件的数A，A有K位，A的数字和是m，A+1的数字和n与m的最大公约数是大于2的质数。输出满足条件的n和A，按照升序排列，其中n是第一标尺，A是第二标尺。
### 思路
暴力的话会超时，可以dfs并且剪枝，剪枝的依据是当前的数字和是否还有可能产生满足条件的A。