Forbes magazine publishes every year its list of billionaires based on the annual ranking of the world's wealthiest people. ==Now you are supposed to simulate this job, but concentrate only on the people in a certain range of ages. That is, given the net worths of $N$ people, you must find the $M$ richest people in a given range of their ages.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==2 positive integers: $N (≤10^5)$ - the total number of people, and $K (≤10^3)$ - the number of queries==. Then $N$ lines follow, each contains the name (string of no more than 8 characters without space), age (integer in (0, 200]), and the net worth (integer in $[−10^6,10^6]$) of a person. ==Finally there are $K$ lines of queries==, each contains three positive integers: $M (≤100)$ - the maximum number of outputs, and [`Amin`, `Amax`] which are the range of ages. All the numbers in a line are separated by a space.

### Output Specification:
For each query, first print in a line `Case #X`: where `X` is the query number starting from 1. Then output the $M$ richest people with their ages in the range [`Amin`, `Amax`]. Each person's information occupies a line, in the format
```
Name Age Net_Worth
```
The outputs must be in non-increasing order of the net worths. In case there are equal worths, it must be in non-decreasing order of the ages. If both worths and ages are the same, then the output must be in non-decreasing alphabetical order of the names. It is guaranteed that there is no two persons share all the same of the three pieces of information. In case no one is found, output `None`.
### Sample Input:
```
12 4
Zoe_Bill 35 2333
Bob_Volk 24 5888
Anny_Cin 95 999999
Williams 30 -22
Cindy 76 76000
Alice 18 88888
Joe_Mike 32 3222
Michael 5 300000
Rosemary 40 5888
Dobby 24 5888
Billy 24 5888
Nobody 5 0
4 15 45
4 30 35
4 5 95
1 45 50
```
### Sample Output:
```
Case #1:
Alice 18 88888
Billy 24 5888
Bob_Volk 24 5888
Dobby 24 5888
Case #2:
Joe_Mike 32 3222
Zoe_Bill 35 2333
Williams 30 -22
Case #3:
Anny_Cin 95 999999
Michael 5 300000
Alice 18 88888
Cindy 76 76000
Case #4:
None
```
### 题意
给出N个人的姓名、年龄和财富。要求完成K个查询，每个查询给出最大输出人数M和年龄范围，要求输出年龄范围内财富最高的最多M个人，要求按照财富非升排序，如果财富相同则按照年龄非降排序，如果年龄也相同则按照姓名非降排序。



M不超过100，很好地约束了问题的大小，因为每个年龄最多只能输出前M个财富最高的人。所以第一步先把这些可能被输出的人筛选出来：对所有人排序，遍历一遍，记录每个年龄的人数，如果某个人所在的年龄的人数已经达到了100就可以忽略。

对于每个查询，在已经筛选出的人中遍历一次，输出年龄符合条件的即可。
```cpp
#include <algorithm>
#include <cstdio>
#include <cstring>

using namespace std;
const int max_n = 100010;

int age[max_n];

struct Person {
    int age, worths;
    char name[10];
} ps[max_n];

int main() {
    int n, k;
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i++)
        scanf("%s%d%d", ps[i].name, &ps[i].age, &ps[i].worths);

    sort(ps, ps + n,
         [](Person a, Person b) {
             if (a.worths != b.worths)
                 return a.worths > b.worths;
             else if (a.age != b.age)
                 return a.age < b.age;
             return strcmp(a.name, b.name) < 0;
         });

    int validCnt = 0;
    for (int i = 0; i < n; i++) {
        if (age[ps[i].age] < 100) {
            age[ps[i].age]++;
            ps[validCnt++] = ps[i];
        }
    }

    for (int i = 1; i <= k; i++) {
        int m, ageL, ageR, cnt = 0;
        scanf("%d%d%d", &m, &ageL, &ageR);
        printf("Case #%d:\n", i);
        for (int j = 0; j < validCnt and cnt < m; j++) {
            if (ps[j].age >= ageL and ps[j].age <= ageR) {
                printf("%s %d %d\n", ps[j].name, ps[j].age, ps[j].worths);
                cnt++;
            }
        }
        if (cnt == 0)
            printf("None\n");
    }
}
```