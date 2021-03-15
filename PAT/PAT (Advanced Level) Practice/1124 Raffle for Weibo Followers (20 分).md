# 题目详情
John got a full mark on PAT. He was so happy that he decided to hold a raffle（抽奖） for his followers on Weibo -- that is, he would ==select winners from every N followers who forwarded his post, and give away gifts. Now you are supposed to help him generate the list of winners.==

### Input Specification:

Each input file contains one test case. For each case, ==the first line gives three positive integers M ($\le$ 1000), N and S, being the total number of forwards, the skip number of winners, and the index of the first winner (the indices start from 1)==. Then M lines follow, each gives the nickname (a nonempty string of no more than 20 characters, with no white space or return) of a follower who has forwarded John's post.

Note: ==it is possible that someone would forward more than once, but no one can win more than once.== Hence ==if the current candidate of a winner has won before, we must skip him/her and consider the next one.==

### Output Specification:

==For each case, print the list of winners in the same order as in the input, each nickname occupies a line. If there is no winner yet, print `Keep going...` instead.==

### Sample Input 1:

    9 3 2
    Imgonnawin!
    PickMe
    PickMeMeMeee
    LookHere
    Imgonnawin!
    TryAgainAgain
    TryAgainAgain
    Imgonnawin!
    TryAgainAgain


### Sample Output 1:

    PickMe
    Imgonnawin!
    TryAgainAgain


### Sample Input 2:

    2 3 5
    Imgonnawin!
    PickMe


### Sample Output 2:

    Keep going...

# 题解

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

vector<string> follower, winner;

int main() {
    int m, n, s;
    cin >> m >> n >> s;
    for (int i = 0; i < m; ++i) {
        string str;
        cin >> str;
        follower.push_back(str);
    }

    if (s <= m) winner.push_back(follower[s - 1]);
    for (int j = 1, p = s; p < m; ++p) {
        if (j == n) {
            bool won = false;
            for (auto& e : winner)
                if (e == follower[p]) won = true;

            if (not won) {
                j = 1;
                winner.push_back(follower[p]);
            }
        }
        else ++j;
    }

    if (winner.empty()) cout << "Keep going...\n";
    else for (auto& i : winner) cout << i << "\n";
}
```

