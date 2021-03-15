https://pintia.cn/problem-sets/994805342720868352/problems/994805495863296000

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
A long-distance telephone company charges its customers by the following rules:

==Making a long-distance call costs a certain amount per minute, depending on the time of day when the call is made.== When a customer starts connecting a long-distance call, the time will be recorded, and so will be the time when the customer hangs up the phone. ==Every calendar month, a bill is sent to the customer for each minute called (at a rate determined by the time of day). Your job is to prepare the bills for each month, given a set of phone call records.==

### Input Specification:

Each input file contains one test case. ==Each case has two parts: the rate structure, and the phone call records.==

The rate structure consists of ==a line with 24 non-negative integers denoting the toll (cents/minute) from 00:00 - 01:00, the toll from 01:00 - 02:00, and so on for each hour in the day.==

The next line contains ==a positive number $N$ ($≤1000$), followed by $N$ lines of records.== Each phone call record consists of the name of ==the customer (string of up to 20 characters without space), the time and date (`mm:dd:hh:mm`), and the word `on-line` or `off-line`.==

For each test case, ==all dates will be within a single month.== ==Each `on-line` record is paired with the chronologically next record for the same customer provided it is an `off-line` record.== ==Any `on-line` records that are not paired with an `off-line` record are ignored, as are `off-line` records not paired with an `on-line` record.== It is guaranteed that at least one call is well paired in the input. You may assume that ==no two records for the same customer have the same time.== Times are recorded using a 24-hour clock.

### Output Specification:

For each test case, you must ==print a phone bill for each customer.==

==Bills must be printed in alphabetical order of customers' names.== For each customer, ==first print in a line the name of the customer and the month of the bill in the format shown by the sample.== ==Then for each time period of a call, print in one line the beginning and ending time and date (`dd:hh:mm`), the lasting time (in minute) and the charge of the call.== ==The calls must be listed in chronological order.== ==Finally, print the total charge for the month in the format shown by the sample.==

### Sample Input:

    10 10 10 10 10 10 20 20 20 15 15 15 15 15 15 15 20 30 20 15 15 10 10 10
    10
    CYLL 01:01:06:01 on-line
    CYLL 01:28:16:05 off-line
    CYJJ 01:01:07:00 off-line
    CYLL 01:01:08:03 off-line
    CYJJ 01:01:05:59 on-line
    aaa 01:01:01:03 on-line
    aaa 01:02:00:01 on-line
    CYLL 01:28:15:41 on-line
    aaa 01:05:02:24 on-line
    aaa 01:04:23:59 off-line


### Sample Output:

    CYJJ 01
    01:05:59 01:07:00 61 $12.10
    Total amount: $12.10
    CYLL 01
    01:06:01 01:08:03 122 $24.40
    28:15:41 28:16:05 24 $3.85
    Total amount: $28.25
    aaa 01
    02:00:01 04:23:59 4318 $638.80
    Total amount: $638.80
# 题解

对于账单的计算可以先统一计算从本月00:00:00到时间的费用，挂断时间和挂起时间得到的费用相减就是实际的通话费用，这种方法还可以避免通话跨天时带来的问题。时间在比较时可以统一换算成分钟。

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

struct Record {
    string name;
    int timestamp, month, day, hour, minute, status;

    bool operator<(const Record& x) const {
        if (name != x.name) return name < x.name;
        else return timestamp < x.timestamp;
    }
}record[1005];

int rate[25];

double fakeBill(Record& r) {  // 计算从本月00:00:00开始到r时所需的费用
    double bill = rate[24] * r.day * 60 + rate[r.hour] * r.minute;
    for (int i = 0; i < r.hour; ++i) bill += rate[i] * 60;
    return bill / 100;
}

int main() {
    for (int i = 0; i < 24; ++i) {
        cin >> rate[i];
        rate[24] += rate[i];  // rate[24]存放通话一整天的费用
    }

    int n;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> record[i].name;
        scanf("%d:%d:%d:%d", &record[i].month, &record[i].day, &record[i].hour, &record[i].minute);
        string status;
        cin >> status;
        record[i].timestamp = record[i].day * 24 * 60 + record[i].hour * 60 + record[i].minute;
        record[i].status = status == "on-line";
    }

    sort(record, record + n);

    // 选出合理的记录
    int validCnt = 0;
    for (int i = 1; i < n; ++i) {
        if (record[i - 1].name == record[i].name and record[i - 1].status == 1 and record[i].status == 0) {
            record[validCnt++] = record[i - 1];
            record[validCnt++] = record[i];
        }
    }

    for (int i = 0; i < validCnt;) {
        int j = i + 1;
        while (j < validCnt and record[j].name == record[i].name) ++j;

        printf("%s %02d\n", record[i].name.c_str(), record[i].month);

        double totalBill = 0;
        for (int k = i; k < j; k += 2) {
            double bill = fakeBill(record[k + 1]) - fakeBill(record[k]);
            totalBill += bill;

            printf("%02d:%02d:%02d %02d:%02d:%02d %d $%.2f\n",
                record[k].day, record[k].hour, record[k].minute,
                record[k + 1].day, record[k + 1].hour, record[k + 1].minute,
                record[k + 1].timestamp - record[k].timestamp, bill);
        }
        printf("Total amount: $%.2f\n", totalBill);
        i = j;
    }
}
```

