# 题目详情

**Mice and Rice** is the name of a programming contest in which ==each programmer must write a piece of code to control the movements of a mouse in a given map. The goal of each mouse is to eat as much rice as possible in order to become a FatMouse.==

==First the playing order is randomly decided for $N_P$ programmers==. ==Then every $N_G$ programmers are grouped in a match==. ==The fattest mouse in a group wins and enters the next turn==. ==All the losers in this turn are ranked the same==. ==Every $N_G$ winners are then grouped in the next match until a final winner is determined.==

==For the sake of simplicity, assume that the weight of each mouse is fixed once the programmer submits his/her code==. ==Given the weights of all the mice and the initial playing order, you are supposed to output the ranks for the programmers.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==2 positive integers: $N_P$ and $N_G (≤1000)$, the number of programmers and the maximum number of mice in a group==, respectively. ==If there are less than $N_G$ mice at the end of the player's list, then all the mice left will be put into the last group==. ==The second line contains $N_P$ distinct non-negative numbers $W_i (i=0,⋯,N_P−1)$== where each $W_i$ is the weight of the i-th mouse respectively. ==The third line gives the initial playing order which is a permutation of $0,⋯,N_P−1$ (assume that the programmers are numbered from $0$ to $N_P−1$).== All the numbers in a line are separated by a space.

### Output Specification:
==For each test case, print the final ranks in a line.== The $i$-th number is the rank of the $i$-th programmer, and all the numbers must be separated by a space, with no extra space at the end of the line.

### Sample Input:
```
11 3
25 18 0 46 37 3 19 22 57 56 10
6 0 8 7 10 5 9 1 4 2 3
```
### Sample Output:
```
5 5 5 2 5 5 5 3 1 3 5
```
# 题解

给出Np只老鼠，将这些老鼠按照重量排名。排名的规则比较复杂：这些老鼠有一个初始顺序，按照这个顺序将这些老鼠分成Ng大小的组，在每一组中挑出重量最大的老鼠进行下一轮比较，其他的老鼠排在后面且排名全部相同。



这个问题有递归的性质，可以据此来编写代码。
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

struct Mouse {
    int rank = -1;
    int weight = 0;
};

void set_rank(vector<Mouse>& mouse, vector<int> seq, int ng) {
    if (seq.size() == 1) { // 剩下一只老鼠，排名为1
        mouse[seq[0]].rank = 1;
        return;
    }

    int max_idx = seq[0]; // 重量最大的老鼠的序号
    vector<int> next_seq; // 下一轮比较的顺序
    for (int i = 1, j = 1, sz = seq.size(); i < sz; ++i, j = (j + 1) % ng) {
        if (mouse[max_idx].weight < mouse[seq[i]].weight)
            max_idx = seq[i];
        if (j == ng - 1 or i == sz - 1) {
            next_seq.push_back(max_idx);
            if (i + 1 < sz)
                max_idx = seq[i + 1];
        }
    }

    set_rank(mouse, next_seq, ng); // 先进行下一轮比较

    int rank = next_seq.size() + 1; // 本轮被淘汰的老鼠的排名
    for (int i = 0, sz = seq.size(); i < sz; ++i)
        if (mouse[seq[i]].rank == -1)
            mouse[seq[i]].rank = rank;
}

int main() {
    int np, ng;
    cin >> np >> ng;
    vector<Mouse> mouse(np);
    for (int i = 0; i < np; ++i)
        cin >> mouse[i].weight;
    vector<int> seq(np);
    for (int i = 0; i < np; ++i)
        cin >> seq[i];

    set_rank(mouse, seq, ng);

    for (int i = 0, sz = mouse.size(); i < sz; ++i) {
        cout << mouse[i].rank;
        if (i != sz - 1)
            cout << " ";
    }
}
```