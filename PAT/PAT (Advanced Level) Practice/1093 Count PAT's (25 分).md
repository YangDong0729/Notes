# 题目详情
The string `APPAPT` contains two `PAT`'s as substrings. The first one is formed by the 2nd, the 4th, and the 6th characters, and the second one is formed by the 3rd, the 4th, and the 6th characters.

Now given any string, you are supposed to tell the number of `PAT`'s contained in the string.

### Input Specification:

Each input file contains one test case. For each case, there is only one line giving a string of no more than $10^5$ characters containing only `P`, `A`, or `T`.

### Output Specification:

For each test case, print in one line the number of `PAT`'s contained in the string. Since the result may be a huge number, you only have to output the result moded by 1000000007.

### Sample Input:

    APPAPT


### Sample Output:

    2

# 题解

给出一个字符串，计算这里有多少子序列是“PAT”。



对于每一个A，能组成的PAT的个数是A左边P的个数乘以A右边T的个数。

```cpp
#include <fstream>
#include <iostream>
#include <string>

using namespace std;

int p[100005], t[100005];

int main() {
    string s;
    cin >> s;

    long long ans = 0;

    int len = s.length();
    // 计算每个位置（包括）左边的P
    if (s[0] == 'P') p[0] = 1;
    for (int i = 1; i < len; ++i) {
        p[i] = p[i - 1];
        if (s[i] == 'P') ++p[i];
    }

    // 计算每个位置（包括）右边的T
    if (s[len - 1] == 'T') t[len - 1] = 1;
    for (int i = len - 1; i >= 0; --i) {
        t[i] = t[i + 1];
        if (s[i] == 'T') ++t[i];
    }

    for (int i = 1; i < len - 1; ++i)
        if (s[i] == 'A')
            ans = (ans + p[i - 1] * t[i + 1]) % 1000000007;
    cout << ans;
}
```

