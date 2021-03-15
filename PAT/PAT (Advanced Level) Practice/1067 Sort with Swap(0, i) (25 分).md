# 题目详情
==Given any permutation of the numbers {0, 1, 2,..., $N−1$}, it is easy to sort them in increasing order. But what if `Swap(0, *)` is the ONLY operation that is allowed to use?== For example, to sort {4, 0, 2, 1, 3} we may apply the swap operations in the following way:

    Swap(0, 1) => {4, 1, 2, 0, 3}
    Swap(0, 3) => {4, 1, 2, 3, 0}
    Swap(0, 4) => {0, 1, 2, 3, 4}

==Now you are asked to find the minimum number of swaps need to sort the given permutation of the first $N$ nonnegative integers.==

### Input Specification:

==Each input file contains one test case, which gives a positive $N$ ($\le 10^5$) followed by a permutation sequence of {0, 1, ..., $N−1$}. All the numbers in a line are separated by a space.==

### Output Specification:

For each case, simply print in a line the minimum number of swaps need to sort the given permutation.

### Sample Input:

    10
    3 5 7 2 6 4 9 0 8 1


### Sample Output:

    9

# 题解

给出0~n-1的一个排列a，仅能使用`swap(a[0], a[x])`的方式将其变为有序，求最小的交换次数。



如果没有仅能使用`swap(a[0], a[x])`的限制：由于已经确定了需要排序的元素就是0~n-1，所以可以确定每个元素的位置（也就是`a[i]=i`）。如果`a[x]!=x`，进行一次`swap(a[x], a[a[x]])`，元素`a[x]`一定会归位（位置`a[x]`），但是如果`a[a[x]]`恰好等于`x`，那么元素`x`也会被归位，在这种情况下一次交换可以让两个元素归位，那么为了获得最小的交换次数，是否需要先执行这种情况的交换呢？不需要，因为要把所有的元素排序，即使不先进行这种交换，序列中的这种环结构的总个数也是不会改变的，减少这种环的唯一方式就是进行一次上述的特殊交换。所以只要每次交换都是有意义的，那么交换的总次数就只由最初的排列唯一决定，和交换的具体的先后次序无关。

现在仅能使用`swap(a[0], a[x])`，可以用`swap(a[0], a[x]), swap(a[0], a[y]), swap(a[0], a[x])`来实现`swap(a[x], a[y])`，可以用`swap(a[0], a[x]), swap(a[0], a[y]), swap(a[0], a[z]), swap(a[0], a[x])`来实现`swap(a[x], a[y]), swap(a[x], a[z])`，可以看出，原来需要k步的操作，现在需要k+2步。但是如果x=0，则还是k步。所以要让x=0时交换尽可能多次。也就是一定要让
```cpp
while (a[0] != 0) {
    swap(a[0], a[a[0]]);
}
```
循环尽可能多次，所以这个一定要最先执行，其他的交换的顺序实际上无所谓。

```cpp
#include <algorithm>
#include <cstdio>

using namespace std;

int a[100005];

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i)
        scanf("%d", &a[i]);

    int cnt = 0;
    for (int i = 0; i < n; ++i) {
        if (a[i] != i) {
            if (i > 0)
                cnt += 2;
            while (a[i] != i) {
                swap(a[i], a[a[i]]);
                ++cnt;
            }
        }
    }

    printf("%d", cnt);
}
```

