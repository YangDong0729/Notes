# 题目详情
For a student taking the online course "Data Structures" on China University MOOC ([http://www.icourse163.org/](http://www.icourse163.org/)), to be qualified for a certificate, he/she must ==first obtain no less than 200 points from the online programming assignments==, and then ==receive a final grade no less than 60 out of 100==. The final grade is calculated by ==$G = (G_{mid-term}\times 40\% + G_{final}\times 60\%)$ if $G_{mid-term} > G_{final}$, or $G_{final}$ will be taken as the final grade $G$==. Here $G_{mid-term}$ and $G_{final}$ are the student's scores of the mid-term and the final exams, respectively.

The problem is that ==different exams have different grading sheets==. Your job is to write a program to merge all the grading sheets into one.

### Input Specification:

Each input file contains one test case. For each case, the first line gives three positive integers: ==P , the number of students having done the online programming assignments;== ==M, the number of students on the mid-term list;== and ==N, the number of students on the final exam list.== ==All the numbers are no more than 10,000.==

Then three blocks follow. The first block contains ==P online programming scores $G_p$'s==; the second one contains ==M mid-term scores $G_{mid-term}$'s;== and the last one contains ==N final exam scores $G_{final}$'s​==. Each score occupies a line with the format: `StudentID Score`, where `StudentID` is a string of no more than 20 English letters and digits, and `Score` is a nonnegative integer (the maximum score of the online programming is 900, and that of the mid-term and final exams is 100).

### Output Specification:

For each case, ==print the list of students who are qualified for certificates.== Each student occupies a line with the format:

`StudentID` $G_p$ $G_{mid-term}$ $G_{final}$ $G$

==If some score does not exist, output "$-1$" instead.== The output must be ==sorted in descending order of their final grades== (==$G$ must be rounded up to an integer==). ==If there is a tie, output in ascending order of their `StudentID`'s.== It is guaranteed that the `StudentID`'s are all distinct, and there is at least one qullified student.

### Sample Input:

    6 6 7
    01234 880
    a1903 199
    ydjh2 200
    wehu8 300
    dx86w 220
    missing 400
    ydhfu77 99
    wehu8 55
    ydjh2 98
    dx86w 88
    a1903 86
    01234 39
    ydhfu77 88
    a1903 66
    01234 58
    wehu8 84
    ydjh2 82
    missing 99
    dx86w 81


### Sample Output:

    missing 400 -1 99 99
    ydjh2 200 98 82 88
    dx86w 220 88 81 84
    wehu8 300 55 84 84
# 题解

这里有一个大坑，round up的意思是**四舍五入**。



```cpp
#include <iostream>
#include <string>
#include <vector>
#include <cmath>
#include <algorithm>
#include <unordered_map>

using namespace std;

struct st {
    string name;
    int gp, gm, gf, g;
};

vector<st> v;
unordered_map<string, int> name2index;

int main() {
    string s;
    int p, m, n, x;

    cin >> p >> m >> n;
    for (int i = 0; i < p; ++i) {
        cin >> s >> x;
        if (x >= 200) {  // 低于200的没资格
            name2index[s] = v.size();
            v.push_back(st{ s, x, -1, -1, -1 });
        }
    }
    for (int i = 0; i < m; ++i) {
        cin >> s >> x;
        if (name2index.count(s))
            v[name2index[s]].gm = x;
    }
    for (int i = 0; i < n; ++i) {
        cin >> s >> x;
        if (name2index.count(s)) {
            int index = name2index[s];
            v[index].gf = x;
            v[index].g = v[index].gm > v[index].gf ? 
                round(v[index].gm * 0.4 + v[index].gf * 0.6) : v[index].gf;
        }
    }

    sort(v.begin(), v.end(), [](st& a, st& b) {
        if (a.g != b.g) return a.g > b.g;
        else return a.name < b.name;
        }
    );

    for (auto i = v.begin(); i != v.end() and i->g >= 60; ++i) {
        cout << i->name << " " << i->gp << " " << i->gm << " " << i->gf << " " << i->g << "\n";
    }
}
```

