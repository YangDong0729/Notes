# 题目详情
Telefraud（电信诈骗） remains a common and persistent problem in our society. In some cases, unsuspecting victims lose their entire life savings. To stop this crime, you are supposed to write a program to detect those suspects from a huge amount of phone call records.
A person must be detected as a suspect if he/she makes more than $K$ short phone calls to **different** people everyday, but no more than 20% of these people would call back. And more, if two suspects are calling each other, we say they might belong to the same gang. $A$ makes a short phone call to $B$ means that the total duration of the calls from $A$ to $B$ is no more than 5 minutes.
### Input Specification:
Each input file contains one test case. For each case, the first line gives 3 positive integers $K$ (≤500, the threshold（阈值） of the amount of short phone calls), $N$ (≤$10^3$, the number of different phone numbers), and $M$ (≤$10^5$, the number of phone call records). Then M lines of one day's records are given, each in the format:
```
caller receiver duration
```
where `caller` and `receiver` are numbered from 1 to $N$, and duration is no more than 1440 minutes in a day.
### Output Specification:
Print in each line all the detected suspects in a gang, in ascending order of their numbers. The gangs are printed in ascending order of their first members. The numbers in a line must be separated by exactly 1 space, and there must be no extra space at the beginning or the end of the line.
If no one is detected, output `None` instead.
### Sample Input 1:
```
5 15 31
1 4 2
1 5 2
1 5 4
1 7 5
1 8 3
1 9 1
1 6 5
1 15 2
1 15 5
3 2 2
3 5 15
3 13 1
3 12 1
3 14 1
3 10 2
3 11 5
5 2 1
5 3 10
5 1 1
5 7 2
5 6 1
5 13 4
5 15 1
11 10 5
12 14 1
6 1 1
6 9 2
6 10 5
6 11 2 6 12 1
6 13 1
```
### Sample Output 1:
```
3 5
6
```
Note: In sample 1, although `1` had 9 records, but there were 7 distinct receivers, among which `5` and `15` both had conversations lasted more than 5 minutes in total.
Hence `1` had made 5 short phone calls and didn't exceed the threshold 5, and therefore is not a suspect.
### Sample Input 2:
```
5 7 8
1 2 1
1 3 1
1 4 1
1 5 1
1 6 1
1 7 1
2 1 1
3 1 1
```
### Sample Output 2:
```
None
```
# 题解
给某个人的短电话是指与这个人的总共时长不超过5分钟。嫌疑人是那些每天打出超过K个短电话，而且这些人中不超过20%打回来的人。如果两个嫌疑人互相通话，就认为他们属于一个团伙。给出通话记录，计算所有的嫌疑人和团伙。

用结构体表示一人的所有记录，包括这个人给其他人打出的电话总时间和所有打给他的电话，这个要支持快速查找，可以使用set。第一步先按照题意找出所有可疑的人。然后对于每一对嫌疑人判断他们是否互相打过电话，用并查集建立团伙信息。最后对团伙进行排序。
注意并查集合并时的写法。