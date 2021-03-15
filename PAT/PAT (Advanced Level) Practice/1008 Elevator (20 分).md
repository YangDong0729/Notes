https://pintia.cn/problem-sets/994805342720868352/problems/994805511923286016

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
The highest building in our city has only one elevator. A request list is made up with $N$ positive numbers. The numbers denote at which floors the elevator will stop, in specified order. ==It costs 6 seconds to move the elevator up one floor, and 4 seconds to move down one floor. The elevator will stay for 5 seconds at each stop.==

For a given request list, you are to compute the total time spent to fulfill the requests on the list. The elevator is on the 0th floor at the beginning and does not have to return to the ground floor when the requests are fulfilled.

### Input Specification:

Each input file contains one test case. Each case contains a positive integer $N$, followed by $N$ positive numbers. All the numbers in the input are less than 100.

### Output Specification:

For each test case, print the total time on a single line.

### Sample Input:

    3 2 3 1


### Sample Output:

    41

# 题解

电梯初始状态在第0层，上升的速度是6秒/层，下降的速度是4秒/层，每个楼层需要停留5秒。给出该电梯依次按照顺序停的楼层数，计算总共花费的时间。

```cpp
#include <iostream>

using namespace std;

int main() {
    int n, now = 0, t = 0;

    cin >> n;
    while (n--) {
        int tmp, c;
        cin >> tmp;

        c = tmp - now;
        if (c >= 0) t += 6 * c + 5;
        else if (c < 0) t += -4 * c + 5;
        now = tmp;
    }
    cout << t;
}
```

