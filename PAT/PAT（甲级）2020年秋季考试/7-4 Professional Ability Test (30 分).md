# 题目详情
Professional Ability Test (PAT) consists of several series of subject tests. Each test is divided into several levels. Level A is a **prerequisite (前置要求)** of Level B if one must pass Level A with a score no less than S in order to be qualified to take Level B. At the mean time, one who passes Level A with a score no less than S will receive a voucher（代金券）of D yuans (Chinese dollar) for taking Level B.

At the moment, this PAT is only in design and hence people would make up different plans. A plan is **NOT** consistent if there exists some test T so that T is a prerequisite of itself. Your job is to test each plan and tell if it is a consistent one, and at the mean time, find the easiest way (with minimum total S) to obtain the certificate of any subject test. If the easiest way is not unique, find the one that one can win the maximum total value of vouchers.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers $N (\le 1000)$ and $M$, being the total numbers of tests and prerequisite relations, respectively. Then $M$ lines follow, each describes a prerequisite relation in the following format:

```
T1 T2 S D
```

where `T1` and `T2` are the indices (from 0 to $N-1$) of the two distinct tests; `S` is the minimum score (in the range (0, 100]) required to pass `T1` in order to be qualified to take `T2`; and `D` is the value of the voucher (in the range (0, 500]) one can receive if one passes `T1` with a score no less than `S` and plan to take `T2`. It is guaranteed that at most one pair of `S` and `D` are defined for a prerequisite relation.

Then another positive integer $K (\le N)$ is given, followed by $K$ queries of tests. All the numbers in a line are separated by spaces.

### Output Specification:

Print in the first line `Okay.` if the whole plan is consistent, or `Impossible.` if not.

If the plan is consistent, for each query of test `T`, print in a line the easiest way to obtain the certificate of this test, in the format:

```
T0->T1->...->T
```

However, if `T` is the first level of some subject test (with no prerequisite), print `You may take test T directly.` instead.

If the plan is impossible, for each query of test `T`, check if one can take it directly or not. If the answer is yes, print in a line `You may take test T directly.`; or print `Error.` instead.

### Sample Input 1:

```
8 15
0 1 50 50
1 2 20 20
3 4 90 90
3 7 90 80
4 5 20 20
7 5 10 10
5 6 10 10
0 4 80 60
3 1 50 45
1 4 30 20
1 5 50 20
2 4 10 10
7 2 10 30
2 5 30 20
2 6 40 60
8
0 1 2 3 4 5 6 7
```

### Sample Output 1:

```
Okay.
You may take test 0 directly.
0->1
0->1->2
You may take test 3 directly.
0->1->2->4
0->1->2->4->5
0->1->2->6
3->7
```

### Sample Input 2:

```
4 5
0 1 1 10
1 2 2 10
3 0 4 10
3 2 5 10
2 0 3 10
2
3 1
```

### Sample Output 2:

```
Impossible.
You may take test 3 directly.
Error.
```