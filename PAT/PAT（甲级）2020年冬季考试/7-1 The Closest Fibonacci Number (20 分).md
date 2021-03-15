# 题目详情
The **Fibonacci sequence** $F_n$ is defined by $F_{n+2} = F_{n+1} + F_n$ for $n\ge 0$, with $F_0 = 0$ and $F_1 = 1$. The **closest Fibonacci number** is defined as the Fibonacci number with the smallest absolute difference with the given integer $N$.

Your job is to find the closest Fibonacci number for any given $N$.

### Input Specification:

Each input file contains one test case, which gives a positive integer $N (\le 10^8)$.

### Output Specification:

For each case, print the closest Fibonacci number. If the solution is not unique, output the smallest one.

### Sample Input:

```
305
```

### Sample Output:

```
233
```

### Hint:

Since part of the sequence is { 0, 1, 1, 2, 3, 5, 8, 12, 21, 34, 55, 89, 144, 233, 377, 610, ... }, there are two solutions: 233 and 377, both have the smallest distance 72 to 305. The smaller one must be printed out.