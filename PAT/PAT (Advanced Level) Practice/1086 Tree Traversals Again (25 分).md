# 题目详情
An inorder binary tree traversal can be implemented in a non-recursive way with a stack. For example, suppose that when a 6-node binary tree (with the keys numbered from 1 to 6) is traversed, ==the stack operations are: push(1); push(2); push(3); pop(); pop(); push(4); pop(); pop(); push(5); push(6); pop(); pop(). Then a unique binary tree (shown in Figure 1) can be generated from this sequence of operations. Your task is to give the postorder traversal sequence of this tree.==

![](https://images.ptausercontent.com/30)  
Figure 1

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==a positive integer $N$ ($\le 30$) which is the total number of nodes in a tree (and hence the nodes are numbered from 1 to $N$). Then $2N$ lines follow, each describes a stack operation in the format: "Push X" where X is the index of the node being pushed onto the stack; or "Pop" meaning to pop one node from the stack.==

### Output Specification:

For each test case, print the postorder traversal sequence of the corresponding tree in one line. A solution is guaranteed to exist. All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

    6
    Push 1
    Push 2
    Push 3
    Pop
    Pop
    Push 4
    Pop
    Pop
    Push 5
    Push 6
    Pop
    Pop


### Sample Output:

    3 4 2 6 5 1

# 题解

有一个二叉树，给出这个二叉树使用非递归的算法进行中序遍历时栈的操作，求这个二叉树的后序遍历。



这道题首先要知道如何使用非递归的方式完成中序遍历：

1. 将根结点入栈。
2. 如果栈顶的结点有左孩子，则将左孩子入栈，直到栈顶结点没有左孩子。
3. 弹出栈顶结点，如果该结点有右孩子，则将右孩子入栈。
4. 如果栈非空，返回步骤2，否则结点出栈的顺序就是中序遍历。

而使用非递归的方式完成前序遍历的方式与上述过程基本相同，唯一的区别是结点入栈的顺序才是前序遍历。



题目给出了入栈和出栈的操作顺序，实际上就是给出了前序遍历和中序遍历，所以就可以根据前序遍历和中序遍历构建二叉树。

```cpp
#include <iostream>
#include <stack>
#include <vector>

using namespace std;

vector<int> inOrder, preOrder;

// preL：前序遍历起始位置，inL：中序遍历起始位置，len：长度
void create(int preL, int inL, int len) {
    int root;  // 当前子树的根节点在中序遍历中的位置
    for (root = inL; root < inL + len; ++root) {
        if (inOrder[root] == preOrder[preL])
            break;
    }

    int lLen = root - inL;            // 当前子树的左子树的中序遍历长度
    int rLen = inL + len - root - 1;  // 当前子树的右子树的中序遍历长度

    if (lLen > 0) {  // 递归地构建左子树
        create(preL + 1, inL, lLen);
        cout << " ";
    }

    if (rLen > 0) {  // // 递归地构建右子树
        create(preL + len - rLen, root + 1, rLen);
        cout << " ";
    }

    cout << preOrder[preL];
}

int main() {
    int n;
    cin >> n;

    stack<int> stk;  // 用这个栈去执行一遍给定的操作，得到前序遍历和中序遍历

    for (int i = 0; i < 2 * n; ++i) {
        string op;
        cin >> op;

        if (op[1] == 'u') {
            int val;
            cin >> val;

            stk.push(val);
            preOrder.push_back(val);
        }
        else {
            inOrder.push_back(stk.top());
            stk.pop();
        }
    }

    create(0, 0, n);
}
```

