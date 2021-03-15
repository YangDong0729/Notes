The Japanese language is notorious for its sentence ending particles. Personal preference of such particles can be considered as a reflection of the speaker's personality. Such a preference is called "Kuchiguse" and is often exaggerated artistically in Anime and Manga. For example, the artificial sentence ending particle "nyan~" is often used as a stereotype for characters with a cat-like personality:

+ Itai nyan~ (It hurts, nyan~)
+ Ninjin wa iyada nyan~ (I hate carrots, nyan~)

==Now given a few lines spoken by the same character, can you find her Kuchiguse?==

### Input Specification:
Each input file contains one test case. For each case, the first line is an integer $N (2≤N≤100)$. Following are $N$ file lines of 0~256 (inclusive) characters in length, each representing a character's spoken line. The spoken lines are case sensitive.
### Output Specification:
For each test case, print in one line the kuchiguse of the character, i.e., the longest common suffix of all N lines. If there is no such suffix, write `nai`.
### Sample Input 1:
```
3
Itai nyan~
Ninjin wa iyadanyan~
uhhh nyan~
```
### Sample Output 1:
```
nyan~
```
### Sample Input 2:
```
3
Itai!
Ninjinnwaiyada T_T
T_T
```
### Sample Output 2:
```
nai
```
# 题解

求多个字符串的共同后缀。



可以先倒转字符串，然后转化成求多个字符串的共同前缀。
```cpp
#include <algorithm>
#include <iostream>
#include <string>

using namespace std;

int main() {
    int n;
    cin >> n;
    cin.get();  // 吃掉回车！！

    string s, ans;
    for (int i = 0; i < n; ++i) {
        getline(cin, s);
        reverse(s.begin(), s.end());
        if (i == 0) {
            ans = s;
        }
        else {
            int min_len = min(s.length(), ans.length());
            for (int j = 0; j < min_len; ++j) {
                if (s[j] != ans[j]) {
                    ans = ans.substr(0, j);
                    break;
                }
            }
        }
    }

    reverse(ans.begin(), ans.end());
    if (ans.length() == 0) cout << "nai";
    else cout << ans;
}
```