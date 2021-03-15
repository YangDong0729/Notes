# 题目详情
![panda.jpg](https://images.ptausercontent.com/6049de68-39dd-4707-b952-a73f5049153c.jpg)

PP milk （盆盆奶）is Pandas' favorite. They would line up to enjoy it as show in the picture. On the other hand, they could drink in peace only if they believe that the amount of PP milk is fairly distributed, that is, ==fatter panda can have more milk, and the ones with equal weight may have the same amount==. Since they are lined up, each panda can only compare with its neighbor(s), and if it thinks this is unfair, the panda would fight with its neighbor.

Given that the minimum amount of milk a panda must drink is 200 ml. It is only when another bowl of milk is at least 100 ml more than its own that a panda can sense the difference.

Now given the weights of a line of pandas, your job is to help the breeder（饲养员）to decide the minimum total amount of milk that he/she must prepare, provided that the pandas are lined up in the given order.

### Input Specification:

Each input file contains one test case. For each case, first a positive integer $n (\le 10^4)$ is given as the number of pandas. Then in the next line, $n$ positive integers are given as the weights (in kg) of the pandas, each no more than 200. the numbers are separated by spaces.

### Output Specification:

For each test case, print in a line the minimum total amount of milk that the breeder must prepare, to make sure that all the pandas can drink in peace.

### Sample Input:

```
10
180 160 100 150 145 142 138 138 138 140
```

### Sample Output:

```
3000
```

### Hint:

The distribution of milk is the following:

```
400 300 200 500 400 300 200 200 200 300
```

# 题解