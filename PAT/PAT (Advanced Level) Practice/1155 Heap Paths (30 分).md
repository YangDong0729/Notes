# 题目详情

In computer science, a **heap** is a specialized tree-based data structure that satisfies the heap property: ==if P is a parent node of C, then the key (the value) of P is either greater than or equal to (in a max heap) or less than or equal to (in a min heap) the key of C==. A common implementation of a heap is the binary heap, in which the tree is a complete binary tree. (Quoted from Wikipedia at [https://en.wikipedia.org/wiki/Heap\_(data\_structure](https://en.wikipedia.org/wiki/Heap_(data_structure)))

One thing for sure is that ==all the keys along any path from the root to a leaf in a max/min heap must be in non-increasing/non-decreasing order==.

Your job is to check every path in a given complete binary tree, in order to tell if it is a heap or not.

### Input Specification:

Each input file contains one test case. For each case, ==the first line gives a positive integer $N (1<N≤1,000)$, the number of keys in the tree==. Then ==the next line contains $N$  distinct integer keys (all in the range of **int**), which gives the level order traversal sequence of a complete binary tree.==

### Output Specification:

For each given tree, first print ==all the paths from the root to the leaves.== Each path occupies a line, with all the numbers separated by a space, and no extra space at the beginning or the end of the line. ==The paths must be printed in the following order: for each node in the tree, all the paths in its right subtree must be printed before those in its left subtree.==

Finally print in a line `Max Heap` if it is a max heap, or `Min Heap` for a min heap, or `Not Heap` if it is not a heap at all.

### Sample Input 1:

    8
    98 72 86 60 65 12 23 50


### Sample Output 1:

    98 86 23
    98 86 12
    98 72 65
    98 72 60 50
    Max Heap


### Sample Input 2:

    8
    8 38 25 58 52 82 70 60


### Sample Output 2:

    8 25 70
    8 25 82
    8 38 52
    8 38 58 60
    Min Heap


### Sample Input 3:

    8
    10 28 15 12 34 9 8 56


### Sample Output 3:

    10 15 8
    10 15 9
    10 28 34
    10 28 12 56
    Not Heap
# 题解

通过深搜遍历每条路径，在深搜的过程中判断这棵完全二叉树是不是堆。

```cpp
#include <iostream>
#include <vector>

using namespace std;

int n, a[1005], flag;

bool check(int p, int c) {
    if (flag == 1)
        return p >= c;
    if (flag == 2)
        return p <= c;
    else
        return false;
}

vector<int> path;

void print() {
    for (auto i = path.begin(); i != path.end(); ++i) {
        cout << *i;
        if (i + 1 != path.end())
            cout << " ";
    }
    cout << "\n";
}

void dfs(int s) {
    if (s * 2 <= n and !check(a[s], a[s * 2]))
        flag = 0;
    if (s * 2 + 1 <= n and !check(a[s], a[s * 2 + 1]))
        flag = 0;

    path.push_back(a[s]);
    if (s * 2 + 1 <= n)
        dfs(s * 2 + 1);
    if (s * 2 <= n)
        dfs(s * 2);
    else
        print();
    path.pop_back();
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];

    if (a[1] >= a[2])
        flag = 1;
    else if (a[1] <= a[2])
        flag = 2;

    dfs(1);

    if (flag == 1)
        cout << "Max Heap";
    else if (flag == 2)
        cout << "Min Heap";
    else
        cout << "Not Heap";
}
```

