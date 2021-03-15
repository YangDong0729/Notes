# 题目详情
Zhejiang University has 8 campuses and a lot of gates. From each gate we can collect the in/out times and the plate numbers of the cars crossing the gate. ==Now with all the information available, you are supposed to tell, at any specific time point, the number of cars parking on campus, and at the end of the day find the cars that have parked for the longest time period.==

### Input Specification:

Each input file contains one test case. Each case starts with ==two positive integers $N$ ($\le 10^4$), the number of records, and $K$ ($\le 8\times 10^4$) the number of queries.== Then $N$ lines follow, each gives a record in the format:

    plate_number hh:mm:ss status


where `plate_number` is a string of 7 English capital letters or 1-digit numbers; `hh:mm:ss` represents the time point in a day by hour:minute:second, with the earliest time being `00:00:00` and the latest `23:59:59`; and `status` is either `in` or `out`.

Note that ==all times will be within a single day. Each `in` record is paired with the chronologically next record for the same car provided it is an `out` record.== ==Any `in` records that are not paired with an `out` record are ignored, as are `out` records not paired with an `in` record.== It is guaranteed that at least one car is well paired in the input, and no car is both `in` and `out` at the same moment. Times are recorded using a 24-hour clock.

==Then $K$ lines of queries follow, each gives a time point in the format `hh:mm:ss`. Note: the queries are given in **ascending** order of the times.==

### Output Specification:

==For each query, output in a line the total number of cars parking on campus.== The last line of output is supposed to ==give the plate number of the car that has parked for the longest time period, and the corresponding time length==. ==If such a car is not unique, then output all of their plate numbers in a line in alphabetical order, separated by a space.==

### Sample Input:

    16 7
    JH007BD 18:00:01 in
    ZD00001 11:30:08 out
    DB8888A 13:00:00 out
    ZA3Q625 23:59:50 out
    ZA133CH 10:23:00 in
    ZD00001 04:09:59 in
    JH007BD 05:09:59 in
    ZA3Q625 11:42:01 out
    JH007BD 05:10:33 in
    ZA3Q625 06:30:50 in
    JH007BD 12:23:42 out
    ZA3Q625 23:55:00 in
    JH007BD 12:24:23 out
    ZA133CH 17:11:22 out
    JH007BD 18:07:01 out
    DB8888A 06:30:50 in
    05:10:00
    06:30:50
    11:00:00
    12:23:42
    14:00:00
    18:00:00
    23:59:00


### Sample Output:

    1
    4
    5
    2
    1
    0
    1
    JH007BD ZD00001 07:20:09
# 题解

给出一些车牌号和出入校园的时间点，计算在某个时间校园内有多少辆车。给出一天之内在校园内时间最长的车。



读入所有的记录，然后筛选出有效记录，计算每辆车的总停靠时间。查询是按照时间顺序进行的，所以可以在处理查询之间对所有的有效记录根据时间排序，然后对每个查询的时间点累计校园中车的数量。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <algorithm>
#include <iostream>
#include <map>
#include <string>

using namespace std;

const int IN = 0, OUT = 1;
map<string, int> parkTime;

struct Record {
    string plate;
    int timeStamp, status;
} record[10005];

int timeStamp(string& time) {
    int h, m, s;
    sscanf(time.c_str(), "%d:%d:%d", &h, &m, &s);
    return h * 3600 + m * 60 + s;
}

bool cmp(Record& a, Record& b) {
    if (a.timeStamp != b.timeStamp)
        return a.timeStamp < b.timeStamp;
    else
        return a.status < b.status;
}

int main() {
    int n, k;  // 读入所有记录
    cin >> n >> k;
    for (int i = 0; i < n; ++i) {
        string time, status;
        cin >> record[i].plate >> time >> status;
        record[i].timeStamp = timeStamp(time);
        record[i].status = status[0] == 'o';
    }

    int validCnt = 0; // 筛选出有效的记录
    sort(record, record + n, [](Record& a, Record& b) {
        if (a.plate != b.plate)
            return a.plate < b.plate;
        else
            return cmp(a, b);
        });

    for (int i = 0; i < n; ++i) {
        while (i < n and record[i].status != IN) ++i;
        if (i + 1 < n and record[i + 1].status == OUT and
            record[i + 1].plate == record[i].plate)
            for (int j = 0; j < 2; ++j) record[validCnt++] = record[i + j];
    }

    int longestPark = 0;  // 计算在校园内的最长时间
    for (int i = 0; i < validCnt; i += 2) {
        parkTime[record[i].plate] += record[i + 1].timeStamp - record[i].timeStamp;
        longestPark = max(longestPark, parkTime[record[i].plate]);
    }

    sort(record, record + validCnt, cmp);  // query
    for (int i = 0, j = 0, cnt = 0; i < k; ++i) {
        string time;
        cin >> time;

        for (; j < validCnt and timeStamp(time) >= record[j].timeStamp; ++j) {
            if (record[j].status == 0) ++cnt;
            else --cnt;
        }

        printf("%d\n", cnt);
    }

    for (auto& e : parkTime) {  // map内部会按照车牌排序
        if (longestPark == e.second)
            cout << e.first << " ";
    }
    printf("%02d:%02d:%02d", longestPark / 3600, (longestPark % 3600) / 60, longestPark % 60);
}
```

