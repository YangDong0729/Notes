# 题目详情
==Given a singly linked list, you are supposed to rearrange its elements so that all the negative values appear before all of the non-negatives, and all the values in \[0, K\] appear before all those greater than K==. ==The order of the elements inside each class must not be changed.== For example, given the list being 18→7→-4→0→5→-6→10→11→-2 and K being 10, you must output -4→-6→-2→7→0→5→10→18→11.

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==the address of the first node==, ==a positive N ($\le 10^5$) which is the total number of nodes==, and ==a positive K ($\le 10^3$).== ==The address of a node is a 5-digit nonnegative integer, and NULL is represented by$-1$.==

Then N lines follow, each describes a node in the format:

    Address Data Next


where ==`Address` is the position of the node==, ==`Data` is an integer in $[-10^5, 10^5]$==, and ==`Next` is the position of the next node==. It is guaranteed that the list is not empty.

### Output Specification:

For each case, output in order (from beginning to the end of the list) the resulting linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

    00100 9 10
    23333 10 27777
    00000 0 99999
    00100 18 12309
    68237 -6 23333
    33218 -4 00000
    48652 -2 -1
    99999 5 68237
    27777 11 48652
    12309 7 33218


### Sample Output:

    33218 -4 68237
    68237 -6 48652
    48652 -2 12309
    12309 7 00000
    00000 0 99999
    99999 5 23333
    23333 10 00100
    00100 18 27777
    27777 11 -1
# 题解

读入链表后遍历三次，找出每一部分的元素。

```cpp
#include <iostream>
#include <vector>

using namespace std;

struct node {
    int addr, data;
} list[100000];

vector<node> ans;

int main() {
    int root, n, k, addr = 0;
    cin >> root >> n >> k;

    for (int i = 0; i < n; ++i)
        cin >> addr >> list[addr].data >> list[addr].addr;

    for (int i = root; i != -1; i = list[i].addr)
        if (list[i].data < 0)
            ans.push_back({i, list[i].data});
    for (int i = root; i != -1; i = list[i].addr)
        if (list[i].data >= 0 and list[i].data <= k)
            ans.push_back({i, list[i].data});
    for (int i = root; i != -1; i = list[i].addr)
        if (list[i].data > k)
            ans.push_back({i, list[i].data});

    for (auto i = ans.begin(); i + 1 != ans.end(); ++i)
        printf("%05d %d %05d\n", i->addr, i->data, (i + 1)->addr);
    printf("%05d %d -1\n", ans.rbegin()->addr, ans.rbegin()->data);
}
```

