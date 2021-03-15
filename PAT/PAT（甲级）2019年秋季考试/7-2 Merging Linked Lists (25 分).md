# 题目详情
Given two singly linked lists $L_1=a_1→a_2→⋯→a_{n−1}→a_n$ and $L_2=b_1→b_2→⋯→b_{m−1}→b_m$. If $n≥2m$, you are supposed to reverse and merge the shorter one into the longer one to obtain a list like $a_1→a_2→b_m→a_3→a_4→b_{m−1}⋯$. For example, given one list being $6→7$ and the other one $1→2→3→4→5$, you must output $1→2→7→3→4→6→5$.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the two addresses of the first nodes of $L1$ and $L2$, plus a positive $N$ ($≤10^5$) which is the total number of nodes given. The address of a node is a 5-digit nonnegative integer, and NULL is represented by `-1`.

Then $N$ lines follow, each describes a node in the format:
```
Address Data Next
```
where `Address` is the position of the node `Data` is a positive integer no more than $10^5$, and `Next` is the position of the next node. It is guaranteed that no list is empty, and the longer list is at least twice as long as the shorter one.

### Output Specification:

For each case, output in order the resulting linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```
00100 01000 7
02233 2 34891
00100 6 00001
34891 3 10086
01000 1 02233
00033 5 -1
10086 4 00033
00001 7 -1
```
### Sample Output:

```
01000 1 02233
02233 2 00001
00001 7 34891
34891 3 10086
10086 4 00100
00100 6 00033
00033 5 -1
```
### 题解
给出两个链表，从较短的那个链表的最后一个元素开始向前逐个插入第一个链表，要求每隔两个元素就插入一个，输出插入后的链表。

先按照静态链表的方式读入这两个链表，然后遍历一遍，在遍历的时候存入vector，这时已经不需要知道每个元素的下一个的位置了，但是要存下这个元素当前的位置，然后把较小的那个vector翻转，再把他们按照题中的要求合并，这时再输出就很容易了。