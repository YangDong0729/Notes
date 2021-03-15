It is said that in 2011, there are about 100 graduate schools ready to proceed over 40,000 applications in Zhejiang Province. It would help a lot if you could write a program to automate the admission procedure.

Each applicant will have to provide two grades: ==the national entrance exam grade $G_E$, and the interview grade $G_I$. The final grade of an applicant is $(G_E+G_I)/2$==. The admission rules are:

+ ==The applicants are ranked according to their final grades, and will be admitted one by one from the top of the rank list.==
+ ==If there is a tied final grade, the applicants will be ranked according to their national entrance exam grade $G_E$. If still tied, their ranks must be the same.==
+ ==Each applicant may have $K$ choices and the admission will be done according to his/her choices: if according to the rank list, it is one's turn to be admitted; and if the quota of one's most preferred shcool is not exceeded, then one will be admitted to this school, or one's other choices will be considered one by one in order. If one gets rejected by all of preferred schools, then this unfortunate applicant will be rejected.==
+ ==If there is a tied rank, and if the corresponding applicants are applying to the same school, then that school must admit all the applicants with the same rank, even if its quota will be exceeded.==
### Input Specification:
Each input file contains one test case.

Each case starts with a line containing ==three positive integers: $N (≤40,000)$, the total number of applicants; $M (≤100)$, the total number of graduate schools; and $K (≤5)$, the number of choices an applicant may have.==

==In the next line, separated by a space, there are $M$ positive integers. The $i$-th integer is the quota of the $i$-th graduate school respectively.==

==Then $N$ lines follow, each contains $2+K$ integers separated by a space. The first 2 integers are the applicant's $G_E$ and $G_I$, respectively. The next $K$ integers represent the preferred schools==. For the sake of simplicity, ==we assume that the schools are numbered from 0 to $M−1$, and the applicants are numbered from 0 to $N−1$.==

### Output Specification:
For each test case you should ==output the admission results for all the graduate schools. The results of each school must occupy a line, which contains the applicants' numbers that school admits. The numbers must be in increasing order and be separated by a space==. There must be no extra space at the end of each line. ==If no applicant is admitted by a school, you must output an empty line correspondingly.==

### Sample Input:

```
11 6 3
2 1 2 2 2 3
100 100 0 1 2
60 60 2 3 5
100 90 0 3 4
90 100 1 2 0
90 90 5 1 3
80 90 1 0 2
80 80 0 1 2
80 80 0 1 2
80 70 1 3 2
70 80 1 2 3
100 100 0 2 4
```
### Sample Output:
```
0 10
3
5 6 7
2 8

1 4
```
# 题解

处理入学申请。根据学生的排名一个一个录取。学生的排名由总分决定，总分相同的学生看笔试成绩。每名同学可以选择K个学校，依次处理。如果在轮到某名学生时，他想去的学校的录取人数还没有超过拟录取人数，那么他就会被录取，或者这所学校录取的上一名学生的排名与他的排名相同，那么他也必须被录取。

按照学校的顺序输出所有被录取的学生ID，学生ID按照升序排序。



先把所有学生按照成绩排序（注意这道题不用算出排名，排名的题做太多了……），然后一个一个处理，记录下被录取的学校，然后按照学校的ID和学生ID再排序一次。
```cpp
#include <algorithm>
#include <cstdio>
#include <vector>

using namespace std;

struct Applicant {
    int id, ge, gi, gf, school, preferred_school[5];
} applicant[40005];

int admit_cnt = 0;   // 被录取的总人数
int quota[105];      // 每所学校拟录取人数
int cnt[105];        // 每所学校已录取的学生人数
int last_admit[105]; // 每所学校最后一个录取的学生ID

int main() {
    int n, m, k;
    scanf("%d %d %d", &n, &m, &k);
    for (int i = 0; i < m; ++i)
        scanf("%d", &quota[i]);
    for (int i = 0; i < n; ++i) {
        scanf("%d %d", &applicant[i].ge, &applicant[i].gi);
        for (int j = 0; j < k; ++j)
            scanf("%d", &applicant[i].preferred_school[j]);
        applicant[i].id = i;
        applicant[i].gf = applicant[i].ge + applicant[i].gi;
    }

    sort(applicant, applicant + n, [](Applicant& a, Applicant& b) {
        if (a.gf != b.gf) return a.gf > b.gf;
        else return a.ge > b.ge;
        });

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < k; ++j) {
            int school = applicant[i].preferred_school[j];
            auto last = cnt[school] ? &applicant[last_admit[school]] : nullptr;
            if (cnt[school] < quota[school] or
                (applicant[i].gi == last->gi and applicant[i].ge == last->ge)) {
                ++cnt[school];
                applicant[i].school = school;
                applicant[admit_cnt] = applicant[i];
                last_admit[school] = admit_cnt++;
                break;
            }
        }
    }

    sort(applicant, applicant + admit_cnt, [](Applicant& a, Applicant& b) {
        if (a.school != b.school) return a.school < b.school;
        else return a.id < b.id;
        });

    for (int i = 0, k = 0; i < m; ++i) {
        for (int j = 0; j < cnt[i]; ++j)
            printf("%s%d", j == 0 ? "" : " ", applicant[k++].id);
        printf("\n");
    }
}
```