# 题目详情
Given N integers, you are supposed to find the smallest positive integer that is NOT in the given list.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N ($\le 10^5$). Then N integers are given in the next line, separated by spaces. All the numbers are in the range of **int**.

### Output Specification:

Print in a line the smallest positive integer that is missing from the input list.

### Sample Input:

    10
    5 -25 9 6 1 3 4 2 5 17


### Sample Output:

    7

# 题解

使用哈希直接映射，来存储所有的数，然后从头开始遍历，找出第一个不存在的正数。



对于这道题来说，这就是最好的方法了，但是如果**给定一个数组**，而不需要读入的话，可以有更好的方法，就是把所有的数放到值与索引相同的位置。然后再从头查找哪个位置没有。

```cpp
#include <iostream>

using namespace std;

bool c[100005];

int main() {
    int n, x;

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> x;
        if (x > 0 and x <= 100000) // 第一个缺少的正数一定是在这个范围之内的
            c[x] = true;
    }

    for (int i = 1; i <= 100000; ++i) {
        if (!c[i]) {
            cout << i;
            return 0;
        }
    }
    cout << 100001;
}
```

