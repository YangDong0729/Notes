# 题目详情
A registration card number of PAT consists of 4 parts:

*   the 1st letter represents ==the test level==, namely, `T` for the top level, `A` for advance and `B` for basic;
*   the 2nd - 4th digits are ==the test site number==, ranged from 101 to 999;
*   the 5th - 10th digits give the ==test date==, in the form of `yymmdd`;
*   finally the 11th - 13th digits are ==the testee's number==, ranged from 000 to 999.

Now given a set of registration card numbers and the scores of the card owners, you are supposed to ==output the various statistics according to the given queries.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==two positive integers $N (≤10^4)$ and $M (≤100)$, the numbers of cards and the queries==, respectively.

==Then $N$ lines follow, each gives a card number and the owner's score (integer in $[0,100]$), separated by a space.==

After the info of testees, there are $M$ lines, each gives a query in the format `Type Term`, where

*   `Type` being 1 means to ==output all the testees on a given level, in non-increasing order of their scores.== The corresponding `Term` will be the letter which specifies the level;
*   `Type` being 2 means to ==output the total number of testees together with their total scores in a given site.== The corresponding `Term` will then be the site number;
*   `Type` being 3 means to ==output the total number of testees of every site for a given test date.== The corresponding `Term` will then be the date, given in the same format as in the registration card.

### Output Specification:

For each query, ==first print in a line `Case #: input`, where `#` is the index of the query case, starting from 1;== and `input` is a copy of the corresponding input query. Then output as requested:

*   for a type 1 query, ==the output format is the same as in input==, that is, `CardNumber Score`. If there is a tie of the scores, output in increasing alphabetical order of their card numbers (uniqueness of the card numbers is guaranteed);
*   for a type 2 query, ==output in the format `Nt Ns` where `Nt` is the total number of testees and `Ns` is their total score;==
*   for a type 3 query, ==output in the format `Site Nt` where `Site` is the site number and `Nt` is the total number of testees at `Site`.== ==The output must be in non-increasing order of `Nt`'s, or in increasing order of site numbers if there is a tie of `Nt`.==

If the result of a query is empty, simply print `NA`.

### Sample Input:

    8 4
    B123180908127 99
    B102180908003 86
    A112180318002 98
    T107150310127 62
    A107180908108 100
    T123180908010 78
    B112160918035 88
    A107180908021 98
    1 A
    2 107
    3 180908
    2 999


### Sample Output:

    Case 1: 1 A
    A107180908108 100
    A107180908021 98
    A112180318002 98
    Case 2: 2 107
    3 260
    Case 3: 3 180908
    107 2
    123 2
    102 1
    Case 4: 2 999
    NA
# 题解

用三个结构体表示三种类型的数据，把所有的查询都算出来。

```cpp
#include <algorithm>
#include <iostream>
#include <sstream>
#include <string>
#include <unordered_map>
#include <vector>

using namespace std;

struct type1 {
    string card;
    int grade;

    bool operator>(const type1 &x) const {
        if (grade != x.grade)
            return grade > x.grade;
        else
            return card < x.card;
    }
};

struct type2 {
    int count = 0;
    int totalScore = 0;
};

struct type3 {
    int site;
    int cnt;

    bool operator>(const type3 &x) const {
        if (cnt != x.cnt)
            return cnt > x.cnt;
        else
            return site < x.site;
    }
};

int main() {
    int n, m;
    cin >> n >> m;

    unordered_map<char, vector<type1>> type_1;
    unordered_map<int, type2> type_2;
    unordered_map<int, vector<type3>> type_3;
    unordered_map<int, unordered_map<int, int>> type_3_raw;

    for (int i = 0; i < n; ++i) {
        int score;
        string card;
        cin >> card >> score;

        char level = card[0];

        int site, date;
        sscanf(card.substr(1, 3).c_str(), "%d", &site);
        sscanf(card.substr(4, 6).c_str(), "%d", &date);

        type_1[level].push_back({card, score});
        type_2[site].count += 1;
        type_2[site].totalScore += score;
        type_3_raw[date][site] += 1;
    }

    for (auto &e : type_1) { // 按照成绩排序
        sort(e.second.begin(), e.second.end(), greater<type1>());
    }

    for (auto &e : type_3_raw) {
        int date = e.first;
        for (auto &e : e.second) {
            type_3[date].push_back({e.first, e.second});
        }
    }

    for (auto &e : type_3) {
        auto &info = e.second;
        sort(info.begin(), info.end(), greater<type3>());
    }

    cin.get(); // 吃掉多余的空格

    for (int i = 0; i < m; ++i) {
        string input;
        getline(cin, input);

        cout << "Case " << i + 1 << ": " << input << "\n";

        stringstream ssm;
        ssm << input;

        int type;
        ssm >> type;

        if (type == 1) {
            char level;
            ssm >> level;

            if (type_1[level].empty())
                cout << "NA\n";
            else {
                for (auto &e : type_1[level]) {
                    cout << e.card << " " << e.grade << "\n";
                }
            }
        } else if (type == 2) {
            int site;
            ssm >> site;

            if (!type_2.count(site))
                cout << "NA\n";
            else
                cout << type_2[site].count << " " << type_2[site].totalScore << "\n";
        } else if (type == 3) {
            int date;
            ssm >> date;

            if (type_3[date].empty())
                cout << "NA\n";
            else {
                for (auto &e : type_3[date]) {
                    cout << e.site << " " << e.cnt << "\n";
                }
            }
        }
    }
}
```

