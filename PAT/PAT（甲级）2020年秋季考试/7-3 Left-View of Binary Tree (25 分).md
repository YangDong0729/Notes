# 题目详情
The **left-view** of a binary tree is a list of nodes obtained by looking at the tree from left hand side and from top down. For example, given a tree shown by the figure, its left-view is { 1, 2, 3, 4, 5 }

![fig.JPG](https://images.ptausercontent.com/d995df0a-31ea-4659-9f26-7bcd03ee23da.JPG)

Given the inorder and preorder traversal sequences of a binary tree, you are supposed to output its left-view.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer $N (\le 20)$, which is the total number of nodes in the tree. Then given in the following 2 lines are the inorder and preorder traversal sequences of the tree, respectively. All the keys in the tree are distinct positive integers in the range of **int**.

### Output Specification:

For each case, print in a line the left-view of the tree. All the numbers in a line are separated by exactly 1 space, and there must be no extra space at the beginning or the end of the line.

### Sample Input:

```
8
2 3 1 5 4 7 8 6
1 2 3 6 7 4 5 8
```

### Sample Output:

```
1 2 3 4 5
```