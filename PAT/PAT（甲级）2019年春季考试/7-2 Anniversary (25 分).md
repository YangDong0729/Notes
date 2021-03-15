# 题目详情

Zhejiang University is about to celebrate her 122th anniversary in 2019. To prepare for the celebration, the alumni association （校友会） has gathered the ID's of all her alumni. Now your job is to write a program to count the number of alumni
 among all the people who come to the celebration.

### Input Specification:

Each input file contains one test case. For each case, the first part is about the information of all the alumni. Given in the first line is a positive integer $N$ (≤105).
Then $N$ lines follow, each contains an ID number of an alumnus. An ID number is a string of 18 digits or the letter `X`. It is guaranteed that all the ID's are distinct.
The next part gives the information of all the people who come to the celebration.
Again given in the first line is a positive integer $M$(≤105). Then $M$ lines follow, each contains an ID number of a guest. It is guaranteed that all the ID's are distinct. 

### Output Specification:

First print in a line the number of alumni among all the people who come to the celebration. Then in the second line, print the ID of the oldest alumnus -- notice that the 7th - 14th digits of the ID gives one's birth date. If no alumnus comes, output the ID of the oldest guest instead. It is guaranteed that such an alumnus or guest is unique.

### Sample Input:

```
5
372928196906118710
610481197806202213
440684198612150417
13072819571002001X
150702193604190912
6
530125197901260019
150702193604190912
220221196701020034
610481197806202213
440684198612150417
370205198709275042
```

### Sample Output:

```
3
150702193604190912
```

# 题解

给出所有校友会成员的id和所有参加庆祝客人的id，计算参加庆祝仪式的人中有多少是校友会的成员，并输出年龄最大的那个校友会成员的id，如果没有校友会成员参加，则输出客人中年龄最大的人。

用结构体表示一个人的id和出生日期。可以用set来解决某个客人是否在校友会中这个问题。那么这个结构体就要重载小于号。这里有一个问题，set根据小于号判断某个元素是否已经属于集合。我第一次写成了这样：

```cpp
bool operator<(const person &x) const {
        return birth < x.birth;
}
```

这会导致set认为两个birth相同的人是同一个人，但这是不对的，题中只说了最老的人是唯一的，没有说明其他人的birth不会重复。
年龄最大的人很好获得，因为set内部是有序的。
注意年龄最大的人是出生日期最小的人。