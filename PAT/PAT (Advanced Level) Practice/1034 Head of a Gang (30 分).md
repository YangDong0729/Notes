# 题目详情

One way that the police finds the head of a gang is to check people's phone calls. ==If there is a phone call between $A$ and $B$, we say that $A$ and $B$ is related==. ==The weight of a relation is defined to be the total time length of all the phone calls made between the two persons==. ==A "Gang" is a cluster of more than 2 persons who are related to each other with total relation weight being greater than a given threshold $K$==. ==In each gang, the one with maximum total weight is the head==. Now given a list of phone calls, you are supposed to find the gangs and the heads.

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==two positive numbers $N$ and $K$ (both less than or equal to 1000), the number of phone calls and the weight threthold==, respectively. Then $N$ lines follow, each in the following format:

    Name1 Name2 Time


where `Name1` and `Name2` are the names of people at the two ends of the call, and `Time` is the length of the call. A name is a string of three capital letters chosen from `A`\-`Z`. A time length is a positive integer which is no more than 1000 minutes.

### Output Specification:

For each test case, first print in a line the total number of gangs. Then for each gang, print in a line the name of the head and the total number of the members. It is guaranteed that the head is unique for each gang. The output must be sorted according to the alphabetical order of the names of the heads.

### Sample Input 1:

    8 59
    AAA BBB 10
    BBB AAA 20
    AAA CCC 40
    DDD EEE 5
    EEE DDD 70
    FFF GGG 30
    GGG HHH 20
    HHH FFF 10


### Sample Output 1:

    2
    AAA 3
    GGG 3


### Sample Input 2:

    8 70
    AAA BBB 10
    BBB AAA 20
    AAA CCC 40
    DDD EEE 5
    EEE DDD 70
    FFF GGG 30
    GGG HHH 20
    HHH FFF 10


### Sample Output 2:

    0

# 题解

给出一些通话记录，两个人如果有通话，就代表两人之间有关联，权重是两人之间通话的总时长。团伙定义为超过两个人的连通分量，且总通话时长大于K（原文是“A “Gang” is a cluster of more than 2 persons who are related to each other with total relation weight being greater than a given threshold K.”，这里的“each other”可能会引发歧义），团伙的头领定义为团伙中通话时间最长的人。求所有的团伙，输出头领的姓名和人数，按照头领的姓名排序。



1. 给每个名字分配一个id，所有通话信息就可以用邻接矩阵表示。

1. 在输入时就可以统计每个人的总通话时间。

1. 用dfs统计每个联通分量的总通话时间和人数。vis数组记录已经访问过的人，为了正确计算联通分量的总权重，在把边累加之后应该清零（注意在累加边权的时候不能使用vis来判断）。

1. 用map建立头领到人数的映射，map内部会根据头领排序，可以直接输出。

```cpp
#include <iostream>
#include <string>
#include <map>
#include <unordered_map>
#include <algorithm>

using namespace std;

int peoCnt = 0;
string id2name[2000];
unordered_map<string, int> name2id;

int getId(string name) {
    int id;
    if (name2id.count(name) == 0) {  // 给新名字分配一个id
        id = peoCnt++;
        name2id[name] = id;
        id2name[id] = name;
    }
    else id = name2id[name];  // 返回已有的id

    return id;
}

bool vis[2000];
int weight[2000];  // 点权
int netWeight[2000][2000];  // 边权

void dfs(int now, int& head, int& member_cnt, int& net_weight) {
    ++member_cnt;
    vis[now] = true;
    if (weight[now] > weight[head]) head = now;

    for (int i = 0; i < peoCnt; ++i) {
        if (netWeight[now][i] > 0) {
            net_weight += netWeight[now][i];
            netWeight[now][i] = netWeight[i][now] = 0;
            if (not vis[i]) dfs(i, head, member_cnt, net_weight);
        }
    }
}

map<string, int> gangs; // head -> cnt

int main() {
    int n, k;
    cin >> n >> k;

    for (int i = 0; i < n; ++i) {
        int w;
        string a, b;
        cin >> a >> b >> w;

        int ida = getId(a);
        int idb = getId(b);

        weight[ida] += w;
        weight[idb] += w;

        netWeight[ida][idb] += w;
        netWeight[idb][ida] += w;
    }

    for (int i = 0; i < peoCnt; ++i) {
        if (not vis[i]) {
            int head = i, member_cnt = 0, net_weight = 0;
            dfs(i, head, member_cnt, net_weight);
            if (net_weight > k and member_cnt > 2)
                gangs[id2name[head]] = member_cnt;
        }
    }

    cout << gangs.size() << "\n";
    for (auto& g : gangs)
        cout << g.first << " " << g.second << "\n";
}
```

