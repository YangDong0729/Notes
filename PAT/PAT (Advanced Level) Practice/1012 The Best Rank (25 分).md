https://pintia.cn/problem-sets/994805342720868352/problems/1038429808099098624

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
To evaluate the performance of our first year CS majored students, we consider their grades of three courses only: `C` - C Programming Language, `M` - Mathematics (Calculus or Linear Algrbra), and `E` - English. At the mean time, we encourage students by emphasizing on their best ranks -- that is, ==among the four ranks with respect to the three courses and the average grade, we print the best rank for each student.==

For example, The grades of `C`, `M`, `E` and `A` - Average of 4 students are given as the following:

    StudentID  C  M  E  A
    310101     98 85 88 90
    310102     70 95 88 84
    310103     82 87 94 88
    310104     91 91 91 91


Then the best ranks for all the students are No.1 since the 1st one has done the best in C Programming Language, while the 2nd one in Mathematics, the 3rd one in English, and the last one in average.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing ==2 numbers $N$ and $M$ ($≤2000$), which are the total number of students, and the number of students who would check their ranks==, respectively. ==Then $N$ lines follow, each contains a student ID which is a string of 6 digits, followed by the three integer grades (in the range of \[0, 100\]) of that student in the order of `C`, `M` and `E`.== Then there are $M$ lines, each containing a student ID.

### Output Specification:

For each of the $M$ students, print in one line the best rank for him/her, and the symbol of the corresponding rank, separated by a space.

The priorities of the ranking methods are ordered as `A` \>\> `C` \>\> `M` \>\> `E`. Hence if there are two or more ways for a student to obtain the same best rank, output the one with the highest priority.

If a student is not on the grading list, simply output `N/A`.

### Sample Input:

    5 6
    310101 98 85 88
    310102 70 95 88
    310103 82 87 94
    310104 91 91 91
    310105 85 90 90
    310101
    310102
    310103
    310104
    310105
    999999


### Sample Output:

    1 C
    1 M
    1 E
    1 A
    3 A
    N/A
# 题解

给出n个考生的三门成绩，对于每一个查询，输出这个考生的最好排名，包括三门功课的排名和平均分的排名。



要对四个成绩分别排序一次，可以用一个`flag`控制`cmp`，使代码简洁。注意名次可能会并列。

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int flag;
int id2pos[1000000];
char course[5] = "ACME";

struct stu {
    int id, best = 0, grade[4], rank[4];
};

stu st[2005];

bool cmp(const stu& s1, const stu& s2) {
    return s1.grade[flag] > s2.grade[flag];
}

int main() {
    int n, m;
    cin >> n >> m;

    for (int i = 0; i < n; ++i) {
        cin >> st[i].id >> st[i].grade[1] >> st[i].grade[2] >> st[i].grade[3];
        st[i].grade[0] = (st[i].grade[1] + st[i].grade[2] + st[i].grade[3]) / 3;
    }

    for (flag = 0; flag < 4; ++flag) {
        sort(st, st + n, cmp);  // 排序
        st[0].rank[flag] = 1;
        for (int i = 1; i < n; ++i) {  // 计算排名
            if (st[i].grade[flag] == st[i - 1].grade[flag])
                st[i].rank[flag] = st[i - 1].rank[flag];
            else st[i].rank[flag] = i + 1;
        }
    }

    for (int i = 0; i < n; ++i) {
        id2pos[st[i].id] = i + 1;  // id2pos[id] == 0 代表id不存在
        int bestRank = st[i].rank[0];  // 计算最好成绩
        for (int j = 1; j < 4; ++j) {
            if (st[i].rank[j] < bestRank) {
                bestRank = st[i].rank[j];
                st[i].best = j;
            }
        }
    }

    for (int i = 0; i < m; ++i) {
        int x;
        cin >> x;
        if (id2pos[x]) {
            int pos = id2pos[x] - 1;
            int best = st[pos].best;
            cout << st[pos].rank[best] << " " << course[best] << "\n";
        }
        else cout << "N/A\n";
    }
}
```

