# 题目详情

Sexy primes are pairs of primes of the form ($p$, $p+6$), so-named since "sex" is the Latin word for "six". (Quoted from http://mathworld.wolfram.com/SexyPrimes.html)
Now given an integer, you are supposed to tell if it is a sexy prime.
### Input Specification:

Each input file contains one test case. Each case gives a positive integer $N$ (≤$10^8$).

### Output Specification:

For each case, print in a line `Yes` if N is a sexy prime, then print in the next line the other sexy prime paired with $N$ (if the answer is not unique, output the smaller number). Or if $N$ is not a sexy prime, print `No` instead, then print in the next line the smallest sexy prime which is larger than $N$.

### Sample Input 1:

```
47
```

### Sample Output 1:

```
Yes
41
```

### Sample Input 2:

```
21
```

### Sample Output 2: 

```
No
23
```

# 题解

一个数 x 是“sexy prime”指的是 x 是质数且 x-6 或 x+6 也是质数。
给出一个数，判断这个数是不是“sexy prime”，如果是的话输出与这个数配对的另一个“sexy prime”，如果有多个就输出小的那个（也就是x-6），如果不是就输出最小的比这个数大的“sexy prime”。

注意一下判断质数的函数前面要先判断`x<=1`，如果是的话直接返回`false`。