# 题目详情
Given a syntax tree (binary), you are supposed to output the corresponding postfix expression, with parentheses reflecting the precedences of the operators. 

### Input Specification: 

Each input file contains one test case. For each case, the first line gives a positive integer $N$ ($≤ 20$) which is the total number of nodes in the syntax tree. Then $N$ lines follow, each gives the information of a node (the $i$-th line corresponds to the $i$-th node) in the format: 
```
data left_child right_child 
```
where `data` is a string of no more than 10 characters, `left_child` and `right_child` are the indices of this node's left and right children, respectively. The nodes are indexed from 1 to N. The NULL link is represented by −1. The figures 1 and 2 correspond to the samples 1 and 2, respectively. 

![7a879aaf66eb3fa8589896b312becc09.png](.media/2019Autumn7-3.png)

### Output Specification: 
For each case, print in a line the postfix expression, with parentheses reflecting the precedences of the operators.There must be no space between any symbols. 

### Sample Input 1: 

```
8 
* 8 7 
a -1 -1 
* 4 1 
+ 2 5 
b -1 -1 
d -1 -1 
- -1 6 
c -1 -1 
```

### Sample Output 1: 

```
(((a)(b)+)((c)(-(d))*)*) 
```

### Sample Input 2: 

```
8 
2.35 -1 -1 
* 6 1 
- -1 4 
% 7 8 
+ 2 3 
a -1 -1 
str -1 -1 
871 -1 -1 
```

### Sample Output 2: 

```
(((a)(2.35)*)(-((str)(871)%))+) 
```

# 题解

给出一棵语法树，求它的后序表达式。

按照题中给出树的方式，用静态节点更好处理，但是不知道根节点是谁，所以在节点中加入父节点的信息，读入结束后从任意一个节点开始向上就能确定根节点。求后序表达式和后序遍历差不多，但是要注意单目运算符的存在，也就是如果子节点只有一个，那么此时应该先输出运算符。