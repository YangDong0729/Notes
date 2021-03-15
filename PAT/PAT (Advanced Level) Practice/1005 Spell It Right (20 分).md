# 题目详情

Given a non-negative integer $N$, your task is to compute the sum of all the digits of $N$, and output every digit of the sum in English.

### Input Specification:

Each input file contains one test case. Each case occupies one line which contains an $N$ ($\le 10^{100}$).

### Output Specification:

For each test case, output in one line the digits of the sum in English words. There must be one space between two consecutive words, but no extra space at the end of a line.

### Sample Input:

    12345


### Sample Output:

    one five

# 题解

给一个非负整数`N`，计算`N`的每一位相加的和，然后输出和的每一位的英文读音。

```cpp
#include <iostream>
#include <string>

using namespace std;

string arr[10] = { "zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine" };

int main() {
    string s;
    cin >> s;

    int sum = 0;
    for (int i = 0; i < s.length(); ++i)
        sum += s[i] - '0';

    string sum_str = to_string(sum);
    
    for (int i = 0; i < sum_str.length(); ++i) {
        cout << arr[sum_str[i] - '0'];
        if (i + 1 != sum_str.length()) cout << ' ';
    }
}
```

