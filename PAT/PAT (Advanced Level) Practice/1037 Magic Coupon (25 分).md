The magic shop in Mars is offering some magic coupons. ==Each coupon has an integer $N$== printed on it, ==meaning that when you use this coupon with a product, you may get $N$ times the value of that product back==! What is more, ==the shop also offers some bonus product for free==. However, ==if you apply a coupon with a positive $N$ to this bonus product, you will have to pay the shop $N$ times the value of the bonus product==... but hey, magically, ==they have some coupons with negative N's==!

For example, given a set of coupons { 1 2 4 −1 }, and a set of product values { 7 6 −2 −3 } (in Mars dollars M\$) where a negative value corresponds to a bonus product. You can apply coupon 3 (with $N$ being 4) to product 1 (with value M\$7) to get M\$28 back; coupon 2 to product 2 to get M\$12 back; and coupon 4 to product 4 to get M\$3 back. On the other hand, if you apply coupon 3 to product 4, you will have to pay M\$12 to the shop.

==Each coupon and each product may be selected at most once. Your task is to get as much money back as possible.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==the number of coupons $N_C$==, followed by a line with $N_C$ coupon integers. Then the next line contains ==the number of products $N_P$==, followed by a line with $N_P$ product values. Here $1≤N_C,N_P≤10^5$, and it is guaranteed that ==all the numbers will not exceed $2^{30}$==.
### Output Specification:
For each test case, simply print in a line the maximum amount of money you can get back.
### Sample Input:
```
4
1 2 4 -1
4
7 6 -2 -3
```
### Sample Output:
```
43
```
# 题解

给出两个序列，从这两个序列中各取出一个数进行相乘，每个数最多能使用一次。求乘积的的和的最大值。

贪心思想：尽可能让大的数相乘。
先对这两个数组排序，对于正数，尽可能选择大的相乘；对于负数，尽可能选择小的相乘。

```cpp
#include <cstdio>
#include <algorithm>

using namespace std;

int ac[100005], ap[100005];

int main() {
    int c, p, sum = 0;
    
    scanf("%d", &c);
    for (int i = 0; i < c; ++i)
        scanf("%d", &ac[i]);
    sort(ac, ac + c);
    scanf("%d", &p);
    for (int i = 0; i < p; ++i)
        scanf("%d", &ap[i]);
    sort(ap, ap + p);
    
    for (int i = c - 1, j = p - 1;
         i >= 0 and j >= 0 and ac[i] > 0 and ap[j] > 0; --i, --j)
        sum += ac[i] * ap[j];
    for (int i = 0, j = 0;
         i < c and j < p and ac[i] < 0 and ap[j] < 0; ++i, ++j)
        sum += ac[i] * ap[j];

    printf("%d", sum);
}
```