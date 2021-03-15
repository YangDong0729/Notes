# 题目详情
The land is for sale in CyberCity, and is divided into several pieces. Here it is assumed that each piece of land has exactly two neighboring pieces, except the first and the last that have only one. One can buy several contiguous（连续的） pieces at a time. Now given the list of prices of the land pieces, your job is to tell a customer in how many different ways that he/she can buy with a certain amount of money.

### Input Specification:

Each input file contains one test case. Each case first gives in a line two positive integers: $N (\le 10^4)$, the number of pieces of the land (hence the land pieces are numbered from 1 to N in order), and $M (\le 10^9)$, the amount of money that your customer has.

Then in the next line, N positive integers are given, where the i\-th one is the price of the i\-th piece of the land.

It is guaranteed that the total price of the land is no more than $10^9$.

### Output Specification:

For each test case, print the number of different ways that your customer can buy. Notice that the pieces must be contiguous.

### Sample Input:

```
5 85
38 42 15 24 9
```

### Sample Output:

```
11
```

### Hint:

The 11 different ways are:

```
38
42
15
24
9
38 42
42 15
42 15 24
15 24
15 24 9
24 9
```