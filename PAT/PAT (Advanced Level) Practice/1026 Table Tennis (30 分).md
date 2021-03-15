# 题目详情
==A table tennis club has N tables available to the public. The tables are numbered from 1 to N.== ==For any pair of players, if there are some tables open when they arrive, they will be assigned to the available table with the smallest number.== ==If all the tables are occupied, they will have to wait in a queue.== ==It is assumed that every pair of players can play for at most 2 hours.==

==Your job is to count for everyone in queue their waiting time, and for each table the number of players it has served for the day.==

One thing that makes this procedure a bit complicated is that ==the club reserves some tables for their VIP members. When a VIP table is open, the first VIP pair in the queue will have the privilege to take it.== However, ==if there is no VIP in the queue, the next pair of players can take it.== On the other hand, ==if when it is the turn of a VIP pair, yet no VIP table is available, they can be assigned as any ordinary players.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==an integer `N` ($≤$10000) - the total number of pairs of players.== ==Then `N` lines follow, each contains 2 times and a VIP tag: `HH:MM:SS` - the arriving time, `P` - the playing time in minutes of a pair of players, and `tag` - which is 1 if they hold a VIP card, or 0 if not.== It is guaranteed that ==the arriving time is between 08:00:00 and 21:00:00 while the club is open.== It is assumed that ==no two customers arrives at the same time.== Following the players' info, there are ==2 positive integers: `K` ($≤$100) - the number of tables, and `M` ($<$ K) - the number of VIP tables.== The last line contains `M` table numbers.

### Output Specification:

For each test case, ==first print the arriving time, serving time and the waiting time for each pair of players in the format shown by the sample.== Then ==print in a line the number of players served by each table.== Notice that ==the output must be listed in chronological order of the serving time.== ==The waiting time must be rounded up to an integer minute(s).== ==If one cannot get a table before the closing time, their information must NOT be printed.==

### Sample Input:

    9
    20:52:00 10 0
    08:00:00 20 0
    08:02:00 30 0
    20:51:00 10 0
    08:10:00 5 0
    08:12:00 10 1
    20:50:00 10 0
    08:01:30 15 1
    20:53:00 10 1
    3 1
    2


### Sample Output:

    08:00:00 08:00:00 0
    08:01:30 08:01:30 0
    08:02:00 08:02:00 0
    08:12:00 08:16:30 5
    08:10:00 08:20:00 10
    20:50:00 20:50:00 0
    20:51:00 20:51:00 0
    20:52:00 20:52:00 0
    3 3 2
# 题解

桌球俱乐部有K张球桌，桌子的编号为1-K。当一对客户到达时，如果有球桌空闲，就把编号最小的球桌给这对客户使用，否则就必须排队。每对客户最多玩2个小时。



有一些球桌是vip球桌。当一个vip球桌空闲时，队列中的第一对vip客户优先使用这个vip球桌，如果队列中没有vip客户，那么下一对客户可以使用它。当没有vip球桌空闲而有一对vip客户需要被服务时，可以给他们分配一个普通球桌。



所有的用户都会在![img](https://cdn.nlark.com/yuque/__latex/fb8fb16eadf8d757ef380c28f7a80c04.svg)之间到达。没有任何用户会在同一时间到达。



按照用户被服务的顺序输出用户的到达时刻、被服务时刻和等待时间。等待时间要被四舍五入到整数。



如果用户不能在关门之前（21:00之前）获得一张桌子，就不应该输出。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <algorithm>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

#define timestamp(h, m, s) ((h)*3600 + (m)*60 + (s))

constexpr int startTime = timestamp(8, 0, 0);
constexpr int endTime = timestamp(21, 0, 0);

int n, k, m;

struct Player {
    int arriveTime, play, serveTime = endTime, vip;  // 到达时间 游戏时间

    bool operator<(const Player& b) {
        return arriveTime > b.arriveTime;
    }

    struct Cmp {
        bool operator()(Player* a, Player* b) {
            return *a < *b;
        }
    };
} player[10005];
priority_queue<Player*, vector<Player*>, Player::Cmp> commonPlayer, vipPlayer;

struct Table {
    int id, freeTime = startTime, vip = 0, serveCnt = 0;

    bool operator<(const Table& b) {  // a的优先级 < b的优先级
        if (freeTime != b.freeTime)
            return freeTime > b.freeTime;
        else
            return id > b.id;
    }

    struct Cmp {
        bool operator()(Table* a, Table* b) {
            return *a < *b;
        }
    };
} table[105];
priority_queue<Table*, vector<Table*>, Table::Cmp> commonTable, vipTable;

Player* getPlayer() {
    auto v = vipPlayer.empty() ? nullptr : vipPlayer.top(),
        c = commonPlayer.empty() ? nullptr : commonPlayer.top();

    if (!v or !c) {
        if (v) vipPlayer.pop();
        if (c) commonPlayer.pop();
        return v ? v : c;
    }

    if (*v < *c) {
        commonPlayer.pop();
        return c;
    }
    else {
        vipPlayer.pop();
        return v;
    }
}

Table* getFreeTable(Player& p) {
    Table* c = commonTable.top();

    if (vipTable.empty()) {
        commonTable.pop();
        return c;
    }

    Table* v = vipTable.top();

    if (p.vip) {
        if (v->freeTime <= max(c->freeTime, p.arriveTime)) {
            vipTable.pop();
            return v;
        }
        else {
            commonTable.pop();
            return c;
        }
    }
    else {
        if (*c < *v and (vipPlayer.empty() or
            vipPlayer.top()->arriveTime > vipTable.top()->freeTime)) {
            vipTable.pop();
            return v;
        }
        else {
            commonTable.pop();
            return c;
        }
    }
}

void addFreeTable(Table* t) {
    if (t->vip)
        vipTable.push(t);
    else
        commonTable.push(t);
}

void printTime(int timestamp) {
    printf("%02d:%02d:%02d", timestamp / 3600, (timestamp / 60) % 60,
        timestamp % 60);
}

int main() {
    cin >> n;  // 读入客户数据
    for (int i = 0; i < n; ++i) {
        int hour, minute, second, play;

        scanf("%d:%d:%d", &hour, &minute, &second);
        player[i].arriveTime = timestamp(hour, minute, second);

        cin >> play;
        if (play > 120) play = 120;
        player[i].play = play * 60;

        cin >> player[i].vip;
        if (player[i].vip)
            vipPlayer.push(player + i);
        else
            commonPlayer.push(player + i);
    }

    cin >> k >> m;  // 读入桌子数据
    for (int i = 1; i <= k; ++i) table[i].id = i;
    for (int i = 0; i < m; ++i) {
        int id;
        cin >> id;
        table[id].vip = true;
    }

    for (int i = 1; i <= k; ++i) {
        if (table[i].vip)
            vipTable.push(table + i);
        else
            commonTable.push(table + i);
    }

    while (true) {
        Player* p = getPlayer();
        if (p == nullptr) break;
        Table* t = getFreeTable(*p);

        p->serveTime = max(t->freeTime, p->arriveTime);
        if (p->serveTime >= endTime) continue;
        t->freeTime = p->serveTime + p->play;
        t->serveCnt++;
        addFreeTable(t);
    }

    sort(player, player + n,
        [](Player& a, Player& b) { return a.serveTime < b.serveTime; });

    for (int i = 0; i < n and player[i].serveTime < endTime; ++i) {
        printTime(player[i].arriveTime);
        printf(" ");
        printTime(player[i].serveTime);
        int waitTimeStamp = player[i].serveTime - player[i].arriveTime;
        printf(" %d\n", waitTimeStamp / 60 + (waitTimeStamp % 60 >= 30));
    }
    for (int i = 1; i <= k; ++i) cout << table[i].serveCnt << (i == k ? "" : " ");
}
```

