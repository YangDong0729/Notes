# 题目详情

==Given a sequence of $K$ integers { $N_1,\ N_2,\ ...,\ N_K$ }. A continuous subsequence is defined to be { $N_i, \ N_{i+1},\ ...,\ N_j$ } where $1 \le i \le j \le K$.== ==The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements.== For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

==Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.==

### Input Specification:

Each input file contains one test case. Each case occupies two lines. The first line contains ==a positive integer $K$ ($\le 10000$).== The second line contains ==$K$ numbers==, separated by a space.

### Output Specification:

For each test case, ==output in one line the largest sum, together with the first and the last numbers of the maximum subsequence==. The numbers must be separated by one space, but there must be no extra space at the end of a line. ==In case that the maximum subsequence is not unique, output the one with the smallest indices $i$ and $j$== (as shown by the sample case). ==If all the $K$ numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.==

### Sample Input:

    10
    -10 1 2 3 4 -5 -23 3 7 -21

### Sample Output:

    10 1 4

# 题解

求最大连续子序列之和，输出最大的和以及这个子序列的首尾元素。如果所有数都小于0，那么认为最大和为0，并且输出整个序列的首尾元素。如果有多组答案，输出下标最小的那组。



在读入时累加和到`nowSum`，如果`nowSum`为负，说明一组正数的和加上若干个负数的和为负数，答案的子序列不能包括这部分，此时应把`nowSum`置0，代表子序列至少应该从下一个元素开始。`nowSum`的最大值就是答案，用另一个变量`sum`来记录。将`sum`初值设为-1，在更新时就不会漏掉最大子序列和为0的情况了，如果全为负数，在读入完成时，`sum`应该还是-1，这种情况也就可以处理了。

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int arr[10010];

int main() {
    int n;
    cin >> n;

    int left = 0, right = n - 1, sum = -1, nowSum = 0, nowLeft = 0;
    for (int i = 0; i < n; ++i) {
        cin >> arr[i];
        nowSum += arr[i];

        if (nowSum < 0) {
            nowSum = 0;
            nowLeft = i + 1;
        }
        else if (nowSum > sum) {
            sum = nowSum;
            left = nowLeft;
            right = i;
        }
    }

    cout << max(sum, 0) << " " << arr[left] << " " << arr[right];
}
```

