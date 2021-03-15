# 题目详情
A **substring** is a continuous part of a string. A **subsequence** is the part of a string that might be continuous or not but the order of the elements is maintained. For example, given the string `atpaaabpabtt`, `pabt` is a substring, while `pat` is a subsequence.

Now given a string $S$ and a subsequence $P$, you are supposed to find the shortest substring of $S$ that contains $P$. If such a solution is not unique, output the left most one.

### Input Specification:

Each input file contains one test case which consists of two lines. The first line contains $S$ and the second line $P$. $S$ is non-empty and consists of no more than $10^4$ lower English letters. $P$ is guaranteed to be a non-empty subsequence of $S$.

### Output Specification:

For each case, print the shortest substring of $S$ that contains $P$. If such a solution is not unique, output the left most one.

### Sample Input:

```
atpaaabpabttpcat
pat
```

### Sample Output:

```
pabt
```