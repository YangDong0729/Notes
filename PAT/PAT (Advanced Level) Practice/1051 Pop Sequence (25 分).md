Given a stack which can keep $M$ numbers at most. Push $N$ numbers in the order of 1, 2, 3, ..., $N$ and pop randomly. ==You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack==. For example, if $M$ is 5 and $N$ is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==3 numbers (all no more than 1000): $M$ (the maximum capacity of the stack), $N$ (the length of push sequence), and $K$ (the number of pop sequences to be checked)==. Then $K$ lines follow, each contains a pop sequence of $N$ numbers. All the numbers in a line are separated by a space.

### Output Specification:

For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

### Sample Input:
```
5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
```

### Sample Output:
```
YES
NO
NO
YES
NO
```
# 题解

有一个容量限制为M的栈，先分别把1，2，3，...，n依次入栈，在入栈的过程中可以随机出栈。给出一系列出栈顺序，问这些出栈顺序是否可能。



模拟
```cpp
#include <iostream>
#include <stack>
#include <vector>

using namespace std;

int main() {
    int m, n, k;
    cin >> m >> n >> k;

    for (int i = 0; i < k; ++i) {
        stack<int> stk;
        vector<int> target(n);

        for (int j = 0; j < n; ++j)
            cin >> target[j];

        for (int j = 1, pos = 0; j <= n; ++j) {
            if (stk.size() == m)
                break;

            stk.push(j);

            while (not stk.empty() and stk.top() == target[pos]) {
                stk.pop();
                ++pos;
            }
        }

        if (stk.empty())
            cout << "YES\n";
        else
            cout << "NO\n";
    }
}
```