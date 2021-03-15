# 题目详情
The basic task is simple: ==given $N$ real numbers, you are supposed to calculate their average.== But what makes it complicated is that some of the input numbers might not be legal. ==A **legal** input is a real number in $[-1000, 1000] $ and is accurate up to no more than 2 decimal places. When you calculate the average, those illegal numbers must not be counted in.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N$ ($\le 100$). Then $N$ numbers are given in the next line==, separated by one space.

### Output Specification:

For each illegal input number, print in a line `ERROR: X is not a legal number` where `X` is the input. Then finally print in a line the result: `The average of K numbers is Y` where `K` is the number of legal inputs and `Y` is their average, accurate to 2 decimal places. In case the average cannot be calculated, output `Undefined` instead of `Y`. In case `K` is only 1, output `The average of 1 number is Y` instead.

### Sample Input 1:

    7
    5 -3.2 aaa 9999 2.3.4 7.123 2.35


### Sample Output 1:

    ERROR: aaa is not a legal number
    ERROR: 9999 is not a legal number
    ERROR: 2.3.4 is not a legal number
    ERROR: 7.123 is not a legal number
    The average of 3 numbers is 1.38


### Sample Input 2:

    2
    aaa -9999


### Sample Output 2:

    ERROR: aaa is not a legal number
    ERROR: -9999 is not a legal number
    The average of 0 numbers is Undefined

# 题解

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <cstdio>
#include <cstring>

using namespace std;

int main() {
	int cnt = 0, n;
	double sum = 0, x;
	char s1[233], s2[233];

	scanf("%d", &n);
	for (int i = 0; i < n; ++i) {
		scanf("%s", s1);  // 读入的字符串
		sscanf(s1, "%lf", &x);  // 按照double读入到x中
		sprintf(s2, "%.2f", x); // 按照.2f的格式打印到另一个字符串中
		s2[strlen(s1)] = '\0';

		if (x > 1000 or x < -1000 or strcmp(s1, s2) != 0)
			printf("ERROR: %s is not a legal number\n", s1);
		else {
			++cnt;
			sum += x;
		}
	}

	if (cnt == 0) printf("The average of 0 numbers is Undefined\n");
	else printf("The average of %d number%s is %.2f\n", cnt, cnt == 1 ? "" : "s", sum / cnt);
}
```

