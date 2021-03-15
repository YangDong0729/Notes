https://pintia.cn/problem-sets/994805342720868352/problems/994805495863296000

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
A **reversible prime** in any number system is a prime whose "reverse" in that number system is also a prime. For example in the decimal system 73 is a reversible prime because its reverse 37 is also a prime.

Now given any two positive integers $N$ ($< 10^5$) and $D$ ($1 < D \le 10$), you are supposed to tell if $N$ is a reversible prime with radix $D$.

### Input Specification:

The input file consists of several test cases. Each case occupies a line which contains two integers $N$ and $D$. The input is finished by a negative $N$.

### Output Specification:

For each test case, print in one line `Yes` if $N$ is a reversible prime with radix $D$, or `No` if not.

### Sample Input:

    73 10
    23 2
    23 10
    -2


### Sample Output:

    Yes
    Yes
    No
# 题解

给出正数`N`和进制`radix`，如果`N`**在十进制下**是素数，且`N`转换为`radix`进制并且反转后的数**在十进制下**也是素数，则输出`Yes`，否则输出`No`。

```cpp
#include <cstdio>
#include <cmath>

bool isPrime(int n) {
    if (n <= 1) return false;
    for (int i = 2, sqr = sqrt(n); i <= sqr; ++i)
        if (n % i == 0) return false;
    return true;
}

int num[111];

int main() {
    int n, radix;
    while (scanf("%d", &n) != EOF) {
        if (n < 0) break;  // n 是负数时退出循环
        scanf("%d", &radix);

        if (not isPrime(n))  // n 不是素数，输出 No，结束
            printf("No\n");
        else {
            int len = 0;

            do {  // 转换为raidx进制，同时逆序
                num[len++] = n % radix;
                n /= radix;
            } while (n != 0);

            for (int i = 0; i < len; ++i)  // 计算这个数在十进制下的值
                n = n * radix + num[i];

            if (isPrime(n)) printf("Yes\n");
            else printf("No\n");
        }
    }
}
```

