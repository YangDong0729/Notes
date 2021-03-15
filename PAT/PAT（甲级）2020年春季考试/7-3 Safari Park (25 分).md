# 题目详情
A safari park（野生动物园）has K species of animals, and is divided into N regions. The managers hope to spread the animals to all the regions, but not the same animals in the two neighboring regions. Of course, they also realize that this is an NP complete problem, you are not expected to solve it. Instead, they have designed several distribution plans. Your job is to write a program to help them tell if a plan is feasible.

### Input Specification:

Each input file contains one test case. For each case, the first line gives 3 integers: $N (0<N\le 500)$, the number of regions; $R (\ge 0)$, the number of neighboring relations, and $K (0 < K \le N)$, the number of species of animals. The regions and the species are both indexed from 1 to N.

Then R lines follow, each gives the indices of a pair of neighboring regions, separated by a space.

Finally there is a positive $M (\le 20)$ followed by $M$ lines of distribution plans. Each plan gives $N$ indices of species in a line (the i\-th index is the animal in the i\-th rigion), separated by spaces. It is guaranteed that any pair of neighboring regions must be different, and there is no duplicated neighboring relations.

### Output Specification:

For each plan, print in a line `Yes` if no animals in the two neighboring regions are the same, or `No` otherwise. However, if the number of species given in a plan is not K, you must print `Error: Too many species.` or `Error: Too few species.` according to the case.

### Sample Input:

```
6 8 3
2 1
1 3
4 6
2 5
2 4
5 4
5 6
3 6
5
1 2 3 3 1 2
1 2 3 4 5 6
4 5 6 6 4 5
2 3 4 2 3 4
2 2 2 2 2 2
```

### Sample Output:

```
Yes
Error: Too many species.
Yes
No
Error: Too few species.
```