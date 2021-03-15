Given two strings $S_1$ and $S_2$, $S=S_1−S_2$ is defined to be the remaining string after taking all the characters in $S_2$ from $S_1$. ==Your task is simply to calculate $S_1−S_2$ for any given strings==. However, it might not be that simple to do it **fast**.

### Input Specification:
Each input file contains one test case. Each case consists of two lines which gives $S_1$ and $S_2$, respectively. The string lengths of both strings are no more than $10^4$. It is guaranteed that all the characters are visible ASCII codes and white space, and a new line character signals the end of a string.

### Output Specification:
For each test case, print $S_1−S_2$ in one line.

### Sample Input:
```
They are students.
aeiou
```
### Sample Output:
```
Thy r stdnts.
```
# 题解

读入两个字符串，输出第一个字符串，但是略过在第二个字符串中出现过的字符。



使用set。
```cpp
#include <iostream>
#include <set>
#include <string>

using namespace std;

int main() {
    string s1, s2;
    getline(cin, s1);
    getline(cin, s2);

    set<char> st;

    for (auto &i : s2)
        st.insert(i);

    for (auto &i : s1)
        if (!st.count(i))
            cout << i;
}
```