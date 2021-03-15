About 900 years ago, a Chinese philosopher Sima Guang wrote a history book in which he talked about people's talent and virtue. According to his theory, ==a man being outstanding in both talent and virtue must be a "sage（圣人）"; being less excellent but with one's virtue outweighs talent can be called a "nobleman（君子）"; being good in neither is a "fool man（愚人）"; yet a fool man is better than a "small man（小人）" who prefers talent than virtue.==

Now given the grades of talent and virtue of a group of people, you are supposed to rank them according to Sima Guang's theory.

### Input Specification:
Each input file contains one test case. Each case first gives ==3 positive integers in a line: $N (≤10^5)$, the total number of people to be ranked; $L (≥60)$, the lower bound of the qualified grades -- that is, only the ones whose grades of talent and virtue are both not below this line will be ranked; and $H (<100)$, the higher line of qualification -- that is, those with both grades not below this line are considered as the "sages", and will be ranked in non-increasing order according to their total grades. Those with talent grades below $H$ but virtue grades not are cosidered as the "noblemen", and are also ranked in non-increasing order according to their total grades, but they are listed after the "sages". Those with both grades below $H$, but with virtue not lower than talent are considered as the "fool men". They are ranked in the same way but after the "noblemen". The rest of people whose grades both pass the $L$ line are ranked after the "fool men".==

Then $N$ lines follow, each gives the information of a person in the format:
```
ID_Number Virtue_Grade Talent_Grade
```
where `ID_Number` is an 8-digit number, and both grades are integers in [0, 100]. All the numbers are separated by a space.

### Output Specification:
==The first line of output must give $M (≤N)$, the total number of people that are actually ranked.== ==Then $M$ lines follow, each gives the information of a person in the same format as the input, according to the ranking rules. If there is a tie of the total grade, they must be ranked with respect to their virtue grades in non-increasing order. If there is still a tie, then output in increasing order of their ID's.==

### Sample Input:

```
14 60 80
10000001 64 90
10000002 90 60
10000011 85 80
10000003 85 80
10000004 80 85
10000005 82 77
10000006 83 76
10000007 90 78
10000008 75 79
10000009 59 90
10000010 88 45
10000012 80 100
10000013 90 99
10000014 66 60
```
### Sample Output:
```
12
10000013 90 99
10000012 80 100
10000003 85 80
10000011 85 80
10000004 80 85
10000007 90 78
10000006 83 76
10000005 82 77
10000002 90 60
10000014 66 60
10000008 75 79
10000001 64 90
```
### 题意
给出N个人的德分和才分，要求将这些人排序。

1. 按照圣人、君子、愚人、小人的顺序排序。
2. 德才都不低于L的才会被排序。
2. 德才都不低于H的是圣人
3. 只有才低于H的是君子
4. 德才都低于H，但是德不低于才的是愚人
5. 剩下的德才均高于L的是小人。
6. 每种人内部都按照总分非升排序，如果总分相同则比较德分，如果德分也相同则比较ID。

### 思路
先将这些人分类，然后再排序。
### 代码
```cpp
#include <algorithm>
#include <cstdio>
#include <cstring>

using namespace std;

struct Person {
    char id[10];
    int v, t, cls, sum;
} person[100005];

int main() {
    int n, l, h;
    scanf("%d %d %d\n", &n, &l, &h);

    int m = 0;
    for (int i = 0; i < n; ++i) {
        scanf("%s %d %d", person[m].id, &person[m].v, &person[m].t);
        if (!(person[m].v < l or person[m].t < l)) {
            person[m].sum = person[m].v + person[m].t;
            if (person[m].v >= h and person[m].t >= h)
                person[m].cls = 1;
            else if (person[m].v >= h and person[m].t < h)
                person[m].cls = 2;
            else if (person[m].v < h and person[m].t < h and person[m].t <= person[m].v)
                person[m].cls = 3;
            else
                person[m].cls = 4;
            ++m;
        }
    }

    sort(person, person + m, [](Person& a, Person& b) {
        if (a.cls != b.cls)
            return a.cls < b.cls;
        else if (a.sum != b.sum)
            return a.sum > b.sum;
        else if (a.v != b.v)
            return a.v > b.v;
        else
            return strcmp(a.id, b.id) < 0;
        });

    printf("%d\n", m);
    for (int i = 0; i < m; ++i)
        printf("%s %d %d\n", person[i].id, person[i].v, person[i].t);
}
```