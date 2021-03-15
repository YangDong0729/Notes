# 题目详情

To store English words, one method is to use linked lists and store a word letter by letter. To save some space, we may let the words share the same sublist if they share the same suffix. For example, `loading` and `being` are stored as showed in Figure 1.

![fig.jpg](https://images.ptausercontent.com/ef0a1fdf-3d9f-46dc-9a27-21f989270fd4.jpg)

You are supposed to find the starting position of the common suffix (e.g. the position of `i` in Figure 1).

### Input Specification:

Each input file contains one test case. For each case, the first line contains two addresses of nodes and a positive $N$ ($\le 10^5$), where the two addresses are the addresses of the first nodes of the two words, and $N$ is the total number of nodes. The address of a node is a 5-digit positive integer, and NULL is represented by $−1$.

Then $N$ lines follow, each describes a node in the format:

    Address Data Next


where`Address` is the position of the node, `Data` is the letter contained by this node which is an English letter chosen from { a-z, A-Z }, and `Next` is the position of the next node.

### Output Specification:

For each case, simply output the 5-digit starting position of the common suffix. If the two words have no common suffix, output `-1` instead.

### Sample Input 1:

    11111 22222 9
    67890 i 00002
    00010 a 12345
    00003 g -1
    12345 D 67890
    00002 n 00003
    22222 B 23456
    11111 L 00001
    23456 e 67890
    00001 o 00010


### Sample Output 1:

    67890


### Sample Input 2:

    00001 00002 4
    00001 a 10001
    10001 s -1
    00002 a 10002
    10002 t -1


### Sample Output 2:

    -1

# 题解

找出两个链表的后缀的第一个节点的位置。



使用静态链表保存节点。从第一个单词的起始位置开始遍历一次，再从第二个单词的起始位置开始遍历，如果遇到了第一次已经遍历过的节点，那么这个节点就是答案。节点存储的字符实际上没有意义，可以忽略。



还有一种思路是先确定两个链表的长度，然后利用双指针让较长的链表的指针先移动直到剩余长度等于较短的指针，然后两者一起移动直到指向同一位置。

```cpp
#include <iostream>
#include <iomanip>

using namespace std;

struct {
    int next;
    bool vis;
} node[100005];

int main() {
    int a, b, n;
    cin >> a >> b >> n;

    for (int i = 0; i < n; i++) {
        int id, ne;
        char d;
        cin >> id >> d >> ne;

        node[id].next = ne;
    }

    for (int pos = a; pos != -1; pos = node[pos].next)
        node[pos].vis = 1;

    int ans = -1;
    for (int pos = b; pos != -1; pos = node[pos].next) {
        if (node[pos].vis == 1) {
            ans = pos;
            break;
        }
    }

    if (ans == -1)
        cout << -1;
    else  // 注意格式
        cout << setw(5) << setfill('0') << ans;
}
```

