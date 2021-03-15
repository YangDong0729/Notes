# 题目详情
Werewolf（狼人杀） is a game in which the players are partitioned into two parties: the werewolves and the human beings. Suppose that in a game,

*   player #1 said: "Player #2 is a werewolf.";
*   player #2 said: "Player #3 is a human.";
*   player #3 said: "Player #4 is a werewolf.";
*   player #4 said: "Player #5 is a human."; and
*   player #5 said: "Player #4 is a human.".

Given that there were 2 werewolves among them, at least one but not all the werewolves were lying, and there were exactly 2 liars. Can you point out the werewolves?

Now you are asked to solve a harder version of this problem: ==given that there were $N$ players, with 2 werewolves among them, at least one but not all the werewolves were lying, and there were exactly 2 liars. You are supposed to point out the werewolves.==

### Input Specification:

Each input file contains one test case. For each case, ==the first line gives a positive integer $N$ ($5 \le N \le 100$). Then $N$ lines follow and the $i$-th line gives the statement of the $i$-th player ($1 \le i \le N$), which is represented by the index of the player with a positive sign for a human and a negative sign for a werewolf.==

### Output Specification:

If a solution exists, print in a line in ==ascending order the indices of the two werewolves.== The numbers must be separated by exactly one space with no extra spaces at the beginning or the end of the line. ==If there are more than one solution, you must output the smallest solution sequence== -- that is, for two sequences $A = a[1], ..., a[M] $ and $B=b[1],...,b[M]$, if there exists $0 \le k < M$ such that $a[i]=b[i]$ ($i \le k$) and $a[k+1]<b[k+1]$, then $A$ is said to be smaller than $B$. ==In case there is no solution, simply print `No Solution`.==

### Sample Input 1:

    5
    -2
    +3
    -4
    +5
    +4

### Sample Output 1:

    1 4


### Sample Input 2:

    6
    +6
    +3
    +1
    -5
    -2
    +4


### Sample Output 2 (the solution is not unique):

    1 5


### Sample Input 3:

    5
    -2
    -3
    -4
    -5
    -1


### Sample Output 3:

    No Solution

# 题解

枚举狼人和骗子的所有可能（注意狼人和骗子的约束），然后检查是否符合每个人的断言。用双指针可以保证得到的第一个答案就是最小的答案。

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int arr[105], wolf[2], liar[2];

bool check(int ass) {
    if (ass < 0) return ass == -wolf[0] or ass == -wolf[1];
    else return ass != wolf[0] and ass != wolf[1];
}

int main() {
    int n;
    cin >> n;

    for (int i = 1; i <= n; ++i) cin >> arr[i];
    for (wolf[0] = 1; wolf[0] <= n; ++wolf[0]) {
        for (wolf[1] = wolf[0] + 1; wolf[1] <= n; ++wolf[1]) {
            for (int k = 0; k < 2; ++k) {
                liar[0] = wolf[k];
                for (int l = 1; l <= n; ++l) {
                    if (l == wolf[1 - k] or l == liar[0]) continue;
                    liar[1] = l;

                    bool flag = true;
                    for (int m = 1; m <= n and flag; ++m) {
                        if (m == liar[0] or m == liar[1]) flag = check(-arr[m]);
                        else flag = check(arr[m]);
                    }
                    if (flag) {
                        cout << wolf[0] << " " << wolf[1];
                        return 0;
                    }
                }
            }

        }
    }
    cout << "No Solution";
}
```

