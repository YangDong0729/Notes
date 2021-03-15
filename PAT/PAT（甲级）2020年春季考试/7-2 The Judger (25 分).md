# 题目详情
A game of numbers has the following rules: at the beginning, two distinct positive integers are given by the judge. Then each player in turn must give a number to the judge. The number must be the difference of two numbers that are previously given, and must not be duplicated to any of the existed numbers. The game will run for several rounds. The one who gives a duplicate number or even a wrong number will be kicked out.

Your job is to write a judger program to judge the players' numbers and to determine the final winners.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two distinct positive integers to begin with. Both numbers are in $[1, 10^5]$.

In the second line, two numbers are given: $N (2\le N\le 10)$, the number of players, and $M (2\le M\le 10^3)$, the number of rounds.

Then $N$ lines follow, each contains $M$ positive integers. The i-th line corresponds to the i-th player $(i=1, \cdots , N)$. The game is to start from the 1st player giving his/her 1st number, followed by everybody else giving their 1st numbers in the 1st round; then everyone give their 2nd numbers in the 2nd round, and so on so forth.

### Output Specification:

If the `i`\-th player is kicked out in the `k`\-th round, print in a line `Round #k: i is out.`. The rest of the numbers given by the one who is out of the game will be ignored. If more than one player is out in the same round, print them in increasing order of their indices. When the game is over, print in the last line `Winner(s): W1 W2 ... Wn`, where `W1 ... Wn` are the indices of the winners in increasing order. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the beginning or the end of the line. If there is no winner, print `No winner.` instead.

### Sample Input 1:

```
101 42
4 5
59 34 67 9 7
17 9 8 50 7
25 92 43 26 37
76 51 1 41 40
```

### Sample Output 1:

```
Round #4: 1 is out.
Round #5: 3 is out.
Winner(s): 2 4
```

### Sample Input 2:

```
42 101
4 5
59 34 67 9 7
17 9 18 50 49
25 92 58 1 39
102 32 2 6 41
```

### Sample Output 2:

```
Round #1: 4 is out.
Round #3: 2 is out.
Round #4: 1 is out.
Round #5: 3 is out.
No winner.
```