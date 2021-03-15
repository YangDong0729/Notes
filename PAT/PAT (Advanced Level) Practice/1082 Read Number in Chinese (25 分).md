Given an integer with no more than 9 digits, you are supposed to read it in the traditional Chinese way. Output `Fu` first if it is negative. For example, -123456789 is read as `Fu yi Yi er Qian san Bai si Shi wu Wan liu Qian qi Bai ba Shi jiu`. Note: zero (`ling`) must be handled correctly according to the Chinese tradition. For example, 100800 is yi Shi Wan ling ba Bai.
### Input Specification:
Each input file contains one test case, which gives an integer with no more than 9 digits.
### Output Specification:
For each test case, print in a line the Chinese way of reading the number. The characters are separated by a space and there must be no extra space at the end of the line.
### Sample Input 1:
```
-123456789
```
### Sample Output 1:
```
Fu yi Yi er Qian san Bai si Shi wu Wan liu Qian qi Bai ba Shi jiu
```
### Sample Input 2:
```
100800
```
### Sample Output 2:
```
yi Shi Wan ling ba Bai
```
# 题解

用汉语读数。这道题中“10”要读成“Yi Shi”。



汉语读数字是4位一个单元，后面再加上“万”，“亿”之类的。可以先按照4位进行分组处理。在一组的内部，连续的0只读一个。在每组之间，如果某一组的千位为0，则也要先读一个0。
```cpp
#include <algorithm>
#include <iostream>
#include <string>

using namespace std;

string num[10] = {"ling", "yi", "er", "san", "si",
                  "wu", "liu", "qi", "ba", "jiu"};
string jie_1[3] = {"Yi", "Wan"};
string jie_2[3] = {"Shi", "Bai", "Qian"};

int main() {
    int x;
    cin >> x;
    if (x < 0) { // 处理负号
        x = -x;
        cout << "Fu ";
    }
    string part[3];                // 把这个数分成3部分 x亿xxxx万xxxx
    for (int i = 2; i >= 0; --i) { // 从后到前处理这3部分
        string v = to_string(x % 10000);
        x /= 10000;
        for (int j = 0; j < v.length();) {
            part[i] += (part[i].empty() ? "" : " ") + num[v[j] - '0']; // 读数字
            if (v[j] != '0') {                                         // 这位不是0
                if (v.length() >= j + 2)                               // 把位读出来
                    part[i] += " " + jie_2[v.length() - j - 2];
            }
            if (v[++j] == '0') { // 下一位是0，则把下标移到连续的0的最后一个的位置
                while (v[j] == '0' and j < v.length())
                    ++j;
                if (j != v.length()) // 但如果直到末尾都是0，就直接结束
                    --j;
            }
        }
        if (i != 2 and part[i] != num[0])
            part[i] += " " + jie_1[i]; // 加上“Yi”或者“Wan”
    }
    int i = 0;
    while (part[i] == num[0] and i < 3) // 不读前面的0
        ++i;
    if (i == 3) // 但如果全是0就输出“ling”
        cout << num[0];
    else {
        cout << part[i] << (i == 2 ? "" : " ");
        for (++i; i < 3; ++i) {
            if (part[i] != num[0] and part[i].find(jie_2[2]) == string::npos)
                cout << num[0] << " "; // 如果这部分没有“Qian”，则还要补一个“ling”
            cout << part[i] << (i == 2 ? "" : " ");
        }
    }
}
```