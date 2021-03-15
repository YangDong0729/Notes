# 题目详情

![FP.JPG](https://images.ptausercontent.com/1cb58ce0-f614-4616-b2a2-f18fc3f4fa34.JPG)

The figure shows the tree view of directories in Windows File Explorer. When a file is selected, there is a file path shown in the above navigation bar. Now given a tree view of directories, your job is to print the file path for any selected file.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N (\le 10^3)$, which is the total number of directories and files. Then $N$ lines follow, each gives the unique 4-digit ID of a file or a directory, starting from the unique root ID `0000`. The format is that the files of depth $d$ will have their IDs indented by $d$ spaces. It is guaranteed that there is no conflict in this tree structure.

Then a positive integer $K (\le 100)$ is given, followed by $K$ queries of IDs.

### Output Specification:

For each queried `ID`, print in a line the corresponding path from the root to the file in the format: `0000->ID1->ID2->...->ID`. If the `ID` is not in the tree, print `Error: ID is not found.` instead.

### Sample Input:

```
14
0000
 1234
  2234
   3234
    4234
    4235
    2333
   5234
   6234
    7234
     9999
  0001
   8234
 0002
4 9999 8234 0002 6666
```

### Sample Output:

```
0000->1234->2234->6234->7234->9999
0000->1234->0001->8234
0000->0002
Error: 6666 is not found.
```