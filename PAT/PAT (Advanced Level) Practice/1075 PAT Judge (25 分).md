The ranklist of PAT is generated from the status list, which shows the scores of the submissions. This time you are supposed to generate the ranklist for PAT.
### Input Specification:
Each input file contains one test case. For each case, the first line contains ==3 positive integers, $N (≤10^4)$, the total number of users, $K (≤5)$, the total number of problems, and $M (≤10^5)$, the total number of submissions.== It is then assumed that the user id's are 5-digit numbers from 00001 to $N$, and the problem id's are from 1 to $K$. The next line contains $K$ positive integers `p[i]` (`i`=1, ..., $K$), where `p[i]` corresponds to the full mark of the i-th problem. Then $M$ lines follow, each gives the information of a submission in the following format:
```
user_id problem_id partial_score_obtained
```
==where `partial_score_obtained` is either −1 if the submission cannot even pass the compiler, or is an integer in the range [0, `p[problem_id]`]. All the numbers in a line are separated by a space.==

### Output Specification:
For each test case, you are supposed to output the ranklist in the following format:
```
rank user_id total_score s[1] ... s[K]
```
==where `rank` is calculated according to the `total_score`, and all the users with the same `total_score` obtain the same `rank`; and `s[i]` is the partial score obtained for the `i`-th problem. If a user has never submitted a solution for a problem, then "-" must be printed at the corresponding position. If a user has submitted several solutions to solve one problem, then the highest score will be counted.==

The ranklist must be printed in non-decreasing order of the ranks. ==For those who have the same rank, users must be sorted in nonincreasing order according to the number of perfectly solved problems. And if there is still a tie, then they must be printed in increasing order of their id's.== ==For those who has never submitted any solution that can pass the compiler, or has never submitted any solution, they must NOT be shown on the ranklist. It is guaranteed that at least one user can be shown on the ranklist.==
### Sample Input:

```
7 4 20
20 25 25 30
00002 2 12
00007 4 17
00005 1 19
00007 2 25
00005 1 20
00002 2 2
00005 1 15
00001 1 18
00004 3 25
00002 2 25
00005 3 22
00006 4 -1
00001 2 18
00002 1 20
00004 1 15
00002 4 18
00001 3 4
00001 4 2
00005 2 -1
00004 2 0
```
### Sample Output:
```
1 00002 63 20 25 - 18
2 00005 42 20 0 22 -
2 00007 42 - 25 - 17
2 00001 42 18 18 4 2
5 00004 40 15 0 25 -
```
### 题意
用户数：N，题目数：K，提交数：M。给出每道题的分数和所有的提交记录。输出排名列表，其中排名由总分计算。每道题的得分取所有提交的最大值。如果排名相同，根据获得满分的题目的个数排序，如果仍然无法区分，则根据用户id排序。没有提交过任何能通过编译的代码的用户不出现在排名列表中。
### 思路
在读入时计算用户每道题的得分和总分，还需要一个变量来记录用户是否出现在排名列表中。排序后在计算排名并输出。
### 代码
```cpp
#include <algorithm>
#include <cstdio>

using namespace std;

struct User {
    int user_id;
    int final_score[6] = { -1, -1, -1, -1, -1, -1 }; // -1代表用户没提交过
    int total_score = 0;
    int full_mark_cnt = 0;
    bool show = false;
} users[10005];

int full_mark[5];

int main() {
    int n, k, m;
    scanf("%d %d %d\n", &n, &k, &m);

    for (int i = 1; i <= n; ++i) // 设置用户id
        users[i].user_id = i;
    for (int i = 1; i <= k; ++i) // 读入每道题的满分
        scanf("%d", &full_mark[i]);
    for (int i = 0; i < m; ++i) {
        int user_id, problem_id, obtained_score;
        scanf("%d %d %d\n", &user_id, &problem_id, &obtained_score); // 读入一个提交

        User& user = users[user_id];
        int& final_score = user.final_score[problem_id];
        final_score = max(final_score, 0);  // 对于应该show的人，即使没编译通过的题目也要显示0，没提交过的才显示-
        if (obtained_score >= 0)  // 至少有一道题通过了编译
            user.show = true;
        if (final_score < obtained_score) { // 更新用户的分数
            user.total_score += obtained_score - final_score;
            if (obtained_score == full_mark[problem_id])
                user.full_mark_cnt++;
            final_score = obtained_score;
        }
    }

    sort(users + 1, users + 1 + n, [](User& a, User& b) {
        if (a.show != b.show)
            return a.show > b.show;
        else if (a.total_score != b.total_score)
            return a.total_score > b.total_score;
        else if (a.full_mark_cnt != b.full_mark_cnt)
            return a.full_mark_cnt > b.full_mark_cnt;
        else
            return a.user_id < b.user_id;
        });

    int cnt = 0, rank = 0, last_sum = -1;
    for (int i = 1; i <= n and users[i].show; ++i) {
        ++cnt;
        if (last_sum != users[i].total_score) {
            rank = cnt;
            last_sum = users[i].total_score;
        }
        printf("%d %05d %d ", rank, users[i].user_id, users[i].total_score);
        for (int j = 1; j <= k; ++j) {
            if (users[i].final_score[j] >= 0)
                printf("%d", users[i].final_score[j]);
            else
                printf("-");
            if (j != k)
                printf(" ");
        }
        printf("\n");
    }
}
```