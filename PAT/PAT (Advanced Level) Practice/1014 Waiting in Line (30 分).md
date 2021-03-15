https://pintia.cn/problem-sets/994805342720868352/problems/1038429808099098624

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
Suppose ==a bank has $N$ windows open for service==. There is ==a yellow line in front of the windows which devides the waiting area into two parts==. The rules for the customers to wait in line are:

*   ==The space inside the yellow line in front of each window is enough to contain a line with $M$ customers==. Hence ==when all the $N$ lines are full, all the customers after (and including) the $(NM+1)$st one will have to wait in a line behind the yellow line.==
*   ==Each customer will choose the shortest line to wait in when crossing the yellow line.== ==If there are two or more lines with the same length, the customer will always choose the window with the smallest number.==
*   $Customer_i$ will take $T_i$ minutes to have his/her transaction processed.
*   The first $N$ customers are assumed to be served at 8:00am.

Now given the processing time of each customer, you are supposed to tell the exact time at which a customer has his/her business done.

For example, suppose that a bank has 2 windows and each window may have 2 customers waiting inside the yellow line. There are 5 customers waiting with transactions taking 1, 2, 6, 4 and 3 minutes, respectively. At 08:00 in the morning, $customer_1$ is served at $window_1$ while $customer_2$ is served at $window_2$. $customer_3$ will wait in front of  $window_1$ and $customer_4$ will wait in front of  $window_2$. $customer_5$ will wait behind the yellow line.

At 08:01, $customer_1$ is done and $customer_5$ enters the line in front of $window_1$ since that line seems shorter now. $customer_2$ will leave at 08:02, $customer_4$ at 08:06, $customer_3$ at 08:07, and finally $customer_5$ at 08:10.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing ==4 positive integers: $N$ ($≤20$, number of windows), $M$ ($\le 10$, the maximum capacity of each line inside the yellow line), $K$ ($≤1000$, number of customers), and $Q$ ($≤1000$, number of customer queries)==.

The next line contains $K$ positive integers, which are ==the processing time of the $K$ customers==.

The last line contains $Q$ positive integers, which ==represent the customers who are asking about the time they can have their transactions done. The customers are numbered from 1 to $K$.==

### Output Specification:

For each of the $Q$ customers, print in one line the time at which his/her transaction is finished, in the format `HH:MM` where `HH` is in \[08, 17\] and `MM` is in \[00, 59\]. Note that since ==the bank is closed everyday after 17:00, for those customers who cannot be served before 17:00, you must output `Sorry` instead==.

### Sample Input:

    2 2 7 5
    1 2 6 4 3 534 2
    3 4 5 6 7


### Sample Output:

    08:07
    08:06
    08:10
    17:00
    Sorry
# 题解

银行有`n`个窗口，每个窗口可以排队`m`人。有`k`名用户，给出每名用户的服务时间。银行在8点开始服务，如果有窗口还没有排满就入队，如果已经排满了就要站在黄线外等待。如果同时有多个窗口没有排满，选择窗口号小的那个排队。如果一个用户在17点前还没有开始服务，他就不会被服务。给出q个查询，计算客户服务结束时的时间。



前`n*m`个人可以直接在窗口前排队，第`n*m+1`名及以后的客户只能等待某个窗口完成一个服务之后排到这个窗口后面。可以用优先队列来得到最先完成服务的窗口。当一个人在某个窗口前排队时，他的开始服务时间就已经是确定的了。



**priority_queue：**

我一直觉得这个东西的设计反人类。



```cpp
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```



1. 第三个参数`Comper(a, b)`的定义是，如果`a`在[弱序](https://baike.baidu.com/item/弱序关系)中先于`b`则返回`true`。
2. top()返回的是弱序中的最后一个（所以你应该叫不优先队列啊！）。



**注意：**

容器的元素不能是引用。原因是容器会使用元素的指针来组织结构，但是没有“指向引用的指针”这种东西，只有“指针的引用”。



容器的元素可以是指针，**但是要为这个指针类型指定Compare**。比如我一开始重载了`Window`的`operator>`，然后`Compare`传入`greater<Window*>`，这样写是不会报错的，**指针会按照值比较大小，重载的大于号没有作用**。

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

#define time2Minute(h, m) ((h)*60 + (m))

const int startTime = time2Minute(8, 0);
const int endTime = time2Minute(17, 0);

struct Customer {
    int process;                                    // 服务时长
    int serveTime = endTime;                        // 被服务的时刻
    int doneTime() {
        return serveTime + process;
    }  // 完成服务的时刻
} customer[1005];

struct Window {
    int index;
    int finishTime = startTime;  // 当前队列全部完成服务的时刻
    queue<Customer*> waitQ;      // 当前窗口的队列

    int nextTime() const {  // 队列中第一个用户服务完成时间
        return waitQ.empty() ? finishTime : waitQ.front()->doneTime();
    }

    void addCustomer(Customer& c) {
        waitQ.push(&c);
        c.serveTime = finishTime;
        finishTime += c.process;
    }
} window[25];

struct cmp {
    bool operator()(const Window* a, const Window* b) {
        if (a->nextTime() != b->nextTime())
            return a->nextTime() > b->nextTime();
        else
            return a->index > b->index;
    }
};

int main() {
    int n, m, k, q;
    cin >> n >> m >> k >> q;

    for (int i = 0; i < n; ++i) window[i].index = i;
    for (int i = 1; i <= k; ++i) cin >> customer[i].process;

    int nextId = 1;

    // 前 n * m 个用户直接进入黄线前等待被服务
    for (int i = 0; i < m and nextId <= k; ++i)
        for (int j = 0; j < n and nextId <= k; ++j)
            window[j].addCustomer(customer[nextId++]);

    // 优先队列用来得到最先完成服务的窗口
    priority_queue<Window*, vector<Window*>, cmp> pq;
    for (int i = 0; i < n; ++i) pq.push(&window[i]);

    // 从第 n * m + 1 个客户开始，需要等待某个窗口处理完成后才能进入黄线前排队
    while (nextId <= k) {
        Customer& c = customer[nextId++];
        Window& w = *pq.top();
        pq.pop();

        w.waitQ.pop();
        w.addCustomer(c);

        pq.push(&w);
    }

    for (int i = 0; i < q; ++i) {
        int x;
        cin >> x;

        Customer& c = customer[x];
        if (c.serveTime < endTime) {
            printf("%02d:%02d\n", c.doneTime() / 60, c.doneTime() % 60);
        }
        else
            cout << "Sorry\n";
    }
}
```

