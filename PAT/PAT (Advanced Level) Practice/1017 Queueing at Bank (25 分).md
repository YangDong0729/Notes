# 题目详情
Suppose ==a bank has $K$ windows open for service. There is a yellow line in front of the windows which devides the waiting area into two parts.== ==All the customers have to wait in line behind the yellow line, until it is his/her turn to be served and there is a window available.== It is assumed that ==no window can be occupied by a single customer for more than 1 hour.==

Now given the arriving time $T$ and the processing time $P$ of each customer, you are supposed to tell the average waiting time of all the customers.

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==2 numbers: $N ($$\le 10^4$) - the total number of customers, and $K$ ($\le 100$) - the number of windows.== Then $N$ lines follow, each contains 2 times: ==`HH:MM:SS` - the arriving time, and $P$ - the processing time in minutes of a customer==. Here `HH` is in the range \[00, 23\], `MM` and `SS` are both in \[00, 59\]. It is assumed that ==no two customers arrives at the same time==.

Notice that ==the bank opens from 08:00 to 17:00==. ==Anyone arrives early will have to wait in line till 08:00, and anyone comes too late (at or after 17:00:01) will not be served nor counted into the average.==

### Output Specification:

For each test case, print in one line the average waiting time of all the customers, in minutes and accurate up to 1 decimal place.

### Sample Input:

    7 3
    07:55:00 16
    17:00:01 2
    07:59:59 15
    08:01:00 60
    08:00:00 30
    08:00:02 2
    08:03:00 10


### Sample Output:

    8.2

# 题解

银行有`k`个窗口，有`n`名用户，给出每名用户的到达时间和服务时间，但是服务时间上限是1小时。银行在8点开始服务，如果有窗口空闲就会进行服务，如果没有窗口空闲就站在黄线外等待。如果一个用户在17点前还没有开始服务，他就不会被服务。计算客户的平均等待时间。



首先按照客户的到达顺序排序。当有客户到达时，选择最先空闲的窗口服务这名客户。如果在客户到达时有多个窗口空闲，选择哪个都可以，这并不影响平均等待时间。

```cpp
#include <iostream>
#include <queue>
#include <algorithm>

using namespace std;

#define timestamp(h, m, s) (((h) * 60 + (m)) * 60 + (s))

struct Customer {
    int arriveTime, processTime;

    bool operator<(const Customer& x) const {
        return arriveTime < x.arriveTime;
    }
}customer[10005];

int n, k;

int main() {
    scanf("%d %d\n", &n, &k);
    for (int i = 0; i < n; ++i) {
        int h, m, s;
        scanf("%d:%d:%d %d", &h, &m, &s, &customer[i].processTime);

        customer[i].arriveTime = timestamp(h, m, s);
        if (customer[i].processTime > 60) customer[i].processTime = 3600;
        else customer[i].processTime *= 60;
    }

    sort(customer, customer + n);

    int startTime = timestamp(8, 0, 0);
    int endTime = timestamp(17, 0, 0);

    // 用优先队列得到最先空闲的窗口
    priority_queue<int, vector<int>, greater<int>> pq;
    for (int i = 0; i < k; ++i) pq.push(startTime);

    int serveCnt = 0;
    double totalWaitTime = 0;
    for (int i = 0; i < n; ++i) {
        if (customer[i].arriveTime > endTime) break;

        int availableTime = pq.top();
        pq.pop();

        ++serveCnt;

        int serveTime = max(availableTime, customer[i].arriveTime);
        totalWaitTime += serveTime - customer[i].arriveTime;
        availableTime = serveTime + customer[i].processTime;

        pq.push(availableTime);
    }

    if (serveCnt == 0) printf("0.0\n");
    else printf("%.1f", totalWaitTime / serveCnt / 60);
}
```

