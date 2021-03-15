Zhejiang University has 40,000 students and provides 2,500 courses. ==Now given the registered course list of each student, you are supposed to output the student name lists of all the courses.==
### Input Specification:
Each input file contains one test case. For each case, the first line contains ==2 numbers: $N (≤40,000)$, the total number of students, and $K (≤2,500)$, the total number of courses==. Then $N$ lines follow, each contains a student's name (3 capital English letters plus a one-digit number), a positive number $C (≤20)$ which is the number of courses that this student has registered, and then followed by $C$ course numbers. For the sake of simplicity, the courses are numbered from 1 to $K$.
### Output Specification:

For each test case, print the student name lists of all the courses in increasing order of the course numbers. For each course, first print in one line the course number and the number of registered students, separated by a space. Then output the students' names in alphabetical order. Each name occupies a line.
### Sample Input:
```
10 5
ZOE1 2 4 5
ANN0 3 5 2 1
BOB5 5 3 4 2 1 5
JOE4 1 2
JAY9 4 1 2 5 4
FRA8 3 4 2 5
DON2 2 4 5
AMY7 1 5
KAT3 3 5 4 2
LOR6 4 2 4 1 5
```
### Sample Output:
```
1 4
ANN0
BOB5
JAY9
LOR6
2 7
ANN0
BOB5
FRA8
JAY9
JOE4
KAT3
LOR6
3 1
BOB5
4 7
BOB5
DON2
FRA8
JAY9
KAT3
LOR6
ZOE1
5 9
AMY7
ANN0
BOB5
DON2
FRA8
JAY9
KAT3
LOR6
ZOE1
```
# 题解

给出选课人数和课程数目，然后再给出每个人的选课情况。输出每门课程的选课人数以及所有选该课的学生的姓名，学生姓名按照字典序排序。



和1039正好是反的。用`vector<string> course2Names[2505]`来存储选择每门课的学生，输出前排序即可。
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

vector<string> course2Names[2505];

int main () {
    cin.tie(nullptr);
    cout.tie(nullptr);
    ios::sync_with_stdio(false);

    string name;
    int n, k, p, courseNum;

    cin >> n >> k;
    for (int i = 0; i < n; ++i) {
        cin >> name >> courseNum;
        for (int j = 0; j < courseNum; ++j) {
            cin >> p;
            course2Names[p].push_back(name);
        }
    }

    for (int i = 1; i <= k; ++i) {
        cout << i << " " << course2Names[i].size() << "\n";
        sort(course2Names[i].begin(), course2Names[i].end());
        for (auto &e: course2Names[i])
            cout << e << "\n";
    }
}
```