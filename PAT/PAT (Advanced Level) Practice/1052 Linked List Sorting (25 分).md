A linked list consists of a series of structures, which are not necessarily adjacent in memory. We assume that each structure contains an integer `key` and a `Next` pointer to the next structure. ==Now given a linked list, you are supposed to sort the structures according to their key values in increasing order.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==a positive $N (<10^5)$ and an address of the head node==, where $N$ is the total number of nodes in memory and the address of a node is a 5-digit positive integer. NULL is represented by −1.

Then $N$ lines follow, each describes a node in the format:
```
Address Key Next
```
where `Address` is the address of the node in memory, `Key` is an integer in $[−10^5 ,10^5]$, and `Next` is the address of the next node. It is guaranteed that all the keys are distinct and there is no cycle in the linked list starting from the head node.

### Output Specification:
For each test case, ==the output format is the same as that of the input==, where $N$ is the total number of nodes in the list and all the nodes must be sorted order.

### Sample Input:
```
5 00001
11111 100 -1
00001 0 22222
33333 100000 11111
12345 -1 33333
22222 1000 12345
```
### Sample Output:
```
5 12345
12345 -1 00001
00001 0 11111
11111 100 22222
22222 1000 33333
33333 100000 -1
```
# 题解

### 题意

对链表排序。

### 思路

输入所有的节点，由于可能存在不在链表上的节点，所以第一步要遍历并标记链表上的节点。然后排序，在排序的时候节点的标记应该是第一标尺。

**注意：**

1. 链表的大小可能为0
2. 这类题的输出格式
### 代码

```cpp
#include <algorithm>
#include <fstream>
#include <iomanip>
#include <iostream>
#include <vector>

using namespace std;

struct Node {
    int address;
    int key;
    int next;
    bool valid;

    friend bool operator<(const Node& a, const Node& b) {
        if (!a.valid or !b.valid)
            return a.valid > b.valid;
        else
            return a.key < b.key;
    }
} node[100005];

int main() {
    int n, head;
    cin >> n >> head;

    for (int i = 0; i < n; ++i) {
        int a, k, nt;
        cin >> a >> k >> nt;
        node[a] = { a, k, nt };
    }

    // 统计所有有效的节点
    int size = 0;
    for (int p = head; p != -1; p = node[p].next) {
        node[p].valid = true;
        ++size;
    }
    cout << size << " ";

    sort(node, node + 100005);

    if (!node[0].valid) // 坑点： size 可能为 0
        cout << -1 << '\n';
    else
        cout << setw(5) << setfill('0') << node[0].address << '\n';

    for (int i = 0; i < size; ++i) {
        cout << setw(5) << setfill('0') << node[i].address << " "
            << node[i].key << " ";
        if (i + 1 == size)
            cout << -1;
        else
            cout << setw(5) << setfill('0') << node[i + 1].address;
        cout << '\n';
    }
}
```