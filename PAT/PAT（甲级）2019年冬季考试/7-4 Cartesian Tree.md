# 题目详情
A **Cartesian tree** is a binary tree constructed from a sequence of distinct numbers. The tree is heap-ordered, and an inorder traversal returns the original sequence. For example, given the sequence { 8, 15, 3, 4, 1, 5, 12, 10, 18, 6 }, the min-heap Cartesian tree is shown by the figure.

![CTree.jpg](https://images.ptausercontent.com/6a99f68a-6578-46e0-9232-fbf0adf3691f.jpg)

Your job is to output the level-order traversal sequence of the min-heap Cartesian tree.

### Input Specification:

Each input file contains one test case. Each case starts from giving a positive integer $N (\le 30)$, and then $N$ distinct numbers in the next line, separated by a space. All the numbers are in the range of **int**.

### Output Specification:

For each test case, print in a line the level-order traversal sequence of the min-heap Cartesian tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the beginning or the end of the line.

### Sample Input:

    10
    8 15 3 4 1 5 12 10 18 6


### Sample Output:

    1 3 5 8 4 6 15 10 12 18

# 题解

这种树我没见过，我想大多数参加考试的人也都没见过。于是就要想到它和已知的树有什么关系。

这种树有一个性质：中序遍历是原来的序列。二叉搜索树的中序遍历是递增的。如果给输入序列中的每个数按照位置赋一个 order ，再按照二叉搜索树的规则插入，就能保证中序是原来的输入序列。

那么如何再保证它是一个最小堆呢？只要插入时按照数的大小插入就行，因为插入的话树一定是向下生长的，那不就一定是个最小堆了嘛。