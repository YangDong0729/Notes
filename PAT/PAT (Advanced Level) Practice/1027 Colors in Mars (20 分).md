# 题目详情
People in Mars represent the colors in their computers in a similar way as the Earth people. That is, ==a color is represented by a 6-digit number, where the first 2 digits are for `Red`, the middle 2 digits for `Green`, and the last 2 digits for `Blue`==. The only difference is that ==they use radix 13 (0-9 and A-C) instead of 16==. Now given a color in three decimal numbers (each between 0 and 168), you are supposed to output their Mars RGB values.

### Input Specification:

Each input file contains one test case which occupies a line containing the three decimal color values.

### Output Specification:

For each test case you should output the Mars RGB value in the following format: first output `#`, ==then followed by a 6-digit number where all the English characters must be upper-cased. If a single color is only 1-digit long, you must print a `0` to its left.==

### Sample Input:

    15 43 71


### Sample Output:

    #123456

# 题解

把三个在[0, 168]范围内的10进制数转换成13进制数。



由于10进制数的范围，转换成的13进制数一定最多是两位数，进制转换就变得很容易了。

```cpp
#include <iostream>

using namespace std;

char s[] = "0123456789ABC";

int main() {
    cout << '#';
    for (int i = 0; i < 3; ++i) {
        int c;
        cin >> c;
        cout << s[c / 13] << s[c % 13];
    }
}
```

