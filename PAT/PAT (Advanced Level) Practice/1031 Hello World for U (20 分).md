https://pintia.cn/problem-sets/994805342720868352/problems/994805464397627392

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
==Given any string of $N$ ($\ge 5$) characters, you are asked to form the characters into the shape of `U`==. For example, `helloworld` can be printed as:

    h  d
    e  l
    l  r
    lowo


That is, ==the characters must be printed in the original order, starting top-down from the left vertical line with $n_1$ characters, then left to right along the bottom line with $n_2$ characters, and finally bottom-up along the vertical line with $n_3$ characters. And more, we would like `U` to be as squared as possible -- that is, it must be satisfied that $n_1 = n_3 = \max \{ k | k \le n_2$  for all $3 \le n_2 \le N \}$ with $n_1 + n_2 + n_3 - 2 = N$.==

### Input Specification:

Each input file contains one test case. Each case contains ==one string with no less than 5 and no more than 80 characters in a line==. The string contains no white space.

### Output Specification:

For each test case, print the input string in the shape of U as specified in the description.

### Sample Input:

    helloworld!


### Sample Output:

    h   !
    e   d
    l   l
    lowor

# 题解

将给定的字符串按照U型输出。这个U要尽可能的“圆”，就是竖边要尽可能的长，但是不能超过底边。



首先要计算出竖边和横边的长度，设字符串的长度为len，则竖边$x = (len + 2) / 3$，横边 $y = len + 2 - 2 * x$。然后一行行输出即可。

```cpp
#include <cstdio>
#include <cstring>

using namespace std;

int main() {
    char s[100];

    scanf("%s", s);
    int len = strlen(s);

    int x = (len + 2) / 3;
    int y = len + 2 - 2 * x;

    for (int i = 0; i < x - 1; ++i) {
        printf("%c", s[i]);
        for (int k = 1; k < y - 1; ++k)
            printf(" ");
        printf("%c\n", s[len - i - 1]);
    }

    for (int i = 0; i < y; ++i)
        printf("%c", s[x + i - 1]);
}
```

