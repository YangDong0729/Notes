==Given a singly linked list $L$ with integer keys, you are supposed to remove the nodes with duplicated absolute values of the keys==. That is, ==for each value $$K$$, only the first node of which the value or absolute value of its key equals $$K$$ will be kept==. At the mean time, all the removed nodes must be kept in a separate list. For example, given $L$ being $$21→-15→-15→-7→15$$, you must output $$21→-15→-7$$, and the removed list $$-15→15$$.

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==the address of the first node, and a positive $$N \ (≤10^5)$$ which is the total number of nodes==. The address of a node is a 5-digit nonnegative integer, and NULL is represented by −1.
Then $$N$$ lines follow, each describes a node in the format:


```
Address Key Next
```


where `Address` is the position of the node, `Key` is an integer of which absolute value is no more than $10^4$, and `Next` is the position of the next node.
### Output Specification:

For each case, output the resulting linked list first, then the removed list. Each node occupies a line, and is printed in the same format as in the input.
### Sample Input:

```
00100 5
99999 -7 87654
23854 -15 00000
87654 15 -1
00000 -15 99999
00100 21 23854
```
### Sample Output:

```
00100 21 23854
23854 -15 99999
99999 -7 -1
00000 -15 87654
87654 15 -1
```
# 题解

给出一个链表，删除这个链表中的一些结点，使得每个结点的值的绝对值是唯一的。输出修改后的链表和删除的结点组成的链表。



遍历一遍链表，将删除的结点按照顺序放到 `removed` 中，保留的放入`keep`中。
```cpp
#include <iostream>
#include <vector>
#include <iomanip>
#include <set>

using namespace std;

struct Node {
    int address, data, next;
} node[100005];

set<int> values;
vector<Node> removed, keep;

void printList(vector<Node>& list) {
    for (int i = 0, size = list.size(); i < size; ++i) {
        printf("%05d %d ", list[i].address, list[i].data);
        if (i == size - 1) printf("-1\n");
        else printf("%05d\n", list[i + 1].address);
    }
}

int main() {
    int head, n, addr;
    cin >> head >> n;

    for (int i = 0; i < n; ++i) {
        cin >> addr;
        node[addr].address = addr;
        cin >> node[addr].data >> node[addr].next;
    }

    for (int iter = head, pre = -1; iter != -1; iter = node[iter].next) {
        if (values.count(abs(node[iter].data))) {
            removed.push_back(node[iter]);
        }
        else {
            keep.push_back(node[iter]);
            values.insert(abs(node[iter].data));
        }
    }

    if (head == -1) printf("-1\n");
    else {
        printList(keep);
        printList(removed);
    }
}
```
