# 题目详情
Some scientists took pictures of thousands of birds in a forest. ==Assume that all the birds appear in the same picture belong to the same tree==. You are supposed to help the scientists to ==count the maximum number of trees in the forest, and for any pair of birds, tell if they are on the same tree.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==a positive number $N$ ($\le 10^4$) which is the number of pictures==. Then $N$ lines follow, each describes a picture in the format:

$K$ $B_1$ $B_2$ $...$ $B_K$

where ==$K$ is the number of birds in this picture, and $B_i$'s are the indices of birds==. It is guaranteed that the birds in all the pictures are numbered continuously from 1 to some number that is no more than $10^4$.

After the pictures there is a positive number $Q$ ($\le 10^4$) which is the number of queries. Then $Q$ lines follow, each contains the indices of two birds.

### Output Specification:

For each test case, first output in a line the maximum possible number of trees and the number of birds. Then for each query, print in a line `Yes` if the two birds belong to the same tree, or `No` if not.

### Sample Input:

    4
    3 10 1 2
    2 3 4
    4 1 5 7 8
    3 9 6 4
    2
    10 5
    3 7


### Sample Output:

    2 10
    Yes
    No
# 题解

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int father[10005];

int findF(int x) {
    if (father[x] != x)
        father[x] = findF(father[x]);
    return father[x];
}

void merge(int a, int b) {
    if (findF(a) != findF(b))
        father[father[b]] = a;
}

int main() {
    for (int i = 0; i < 10005; ++i)
        father[i] = i;

    int n, k, t1, t2, q, bird_num = 0;
    cin >> n;

    for (int i = 0; i < n; ++i) {
        cin >> k >> t1;
        bird_num = max(bird_num, t1);
        for (int j = 1; j < k; ++j) {
            cin >> t2;
            bird_num = max(bird_num, t2);
            merge(t1, t2);
        }
    }

    int cnt = 0;
    for (int i = 1; i <= bird_num; ++i) {
        if (father[i] == i) ++cnt;
    }

    cout << cnt << " " << bird_num << "\n";

    cin >> q;
    for (int i = 0; i < q; ++i) {
        cin >> t1 >> t2;
        if (findF(t1) == findF(t2)) cout << "Yes\n";
        else cout << "No\n";
    }
}
```

