==On a broken keyboard, some of the keys are worn out. So when you type some sentences, the characters corresponding to those keys will not appear on screen.==

==Now given a string that you are supposed to type, and the string that you actually type out, please list those keys which are for sure worn out.==

### Input Specification:
Each input file contains one test case. For each case, the 1st line contains the original string, and the 2nd line contains the typed-out string. Each string contains no more than 80 characters which are either English letters [A-Z] (case insensitive), digital numbers [0-9], or `_` (representing the space). It is guaranteed that both strings are non-empty.
### Output Specification:
For each test case, print in one line the keys that are worn out, in the order of being detected. The English letters must be capitalized. Each worn out key must be printed once only. It is guaranteed that there is at least one worn out key.
### Sample Input:
```
7_This_is_a_test
_hs_s_a_es
```
### Sample Output:
```
7TI
```
# 题解

键盘上有一些键坏掉了，给出两行字符串，分别是想要打的和实际打出的，求键盘上一定坏掉的键。按照被检测出的顺序输出，字母要大写，字符不能重复。



遍历第一个字符串，判断字符在第二个字符串中是否出现。
```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string s1, s2, s3;
    cin >> s1 >> s2;
    for (auto &i : s1) 
        if (s2.find(i) == string::npos and s3.find(toupper(i)) == string::npos)
            s3 += toupper(i);
    cout << s3;
}
```