==A gas station has to be built at such a location that the minimum distance between the station and any of the residential housing is as far away as possible. However it must guarantee that all the houses are in its service range.==

==Now given the map of the city and several candidate locations for the gas station, you are supposed to give the best recommendation. If there are more than one solution, output the one with the smallest average distance to all the houses. If such a solution is still not unique, output the one with the smallest index number.==

### Input Specification:
Each input file contains one test case. For each case, the first line contains ==4 positive integers: $N (≤10^3)$, the total number of houses; $M (≤10)$, the total number of the candidate locations for the gas stations; $K (≤10^4)$, the number of roads connecting the houses and the gas stations; and $D_S$, the maximum service range of the gas station==. It is hence assumed that all the houses are numbered from 1 to $N$, and all the candidate locations are numbered from $1$ to $M$.

Then $K$ lines follow, each describes a road in the format
```
P1 P2 Dist
```
where `P1` and `P2` are the two ends of a road which can be either house numbers or gas station numbers, and `Dist` is the integer length of the road.
### Output Specification:
==For each test case, print in the first line the index number of the best location. In the next line, print the minimum and the average distances between the solution and all the houses==. The numbers in a line must be separated by a space and be accurate up to 1 decimal place. If the solution does not exist, simply output `No Solution`.

### Sample Input 1:
```
4 3 11 5
1 2 2
1 4 2
1 G1 4
1 G2 3
2 3 2
2 G2 1
3 4 2
3 G3 2
4 G1 3
G2 G1 1
G3 G2 2
```
### Sample Output 1:
```
G1
2.0 3.3
```
### Sample Input 2:
```
2 1 2 10
1 G1 9
2 G1 20
```
### Sample Output 2:
```
No Solution
```
# 题解

从m个候选位置中选出一个位置，这个位置距离所有的住宅的距离不超过加油站的服务范围，而且到所有住宅的最短距离最大。如果答案不唯一，输出到所有住宅的平均距离最小的，如果仍然不唯一，则输出编号最小的。



首先求出这m个位置到每个住宅区的距离，判断是否有超过服务范围的住宅。如果没有，则计算到所有住宅的距离中最短的距离，从而更新答案。
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <queue>
#include <string>
#include <vector>

using namespace std;

const int INF = -1;
const int MAX_V = 1100;

int n, m, k, ds;

struct Node {
    int dist;
    bool visit;
    vector<pair<int, int>> adj;
} node[MAX_V];

void dijkstra(int s) {
    for (int i = 0; i < MAX_V; ++i) {
        node[i].dist = INF;
        node[i].visit = false;
    }

    node[s].dist = 0;

    using pair = pair<int, int>;
    priority_queue<pair, vector<pair>, greater<pair>> pq;
    pq.push({ 0, s });

    while (not pq.empty()) {
        int u = pq.top().second, uDist = pq.top().first;
        pq.pop();

        node[u].visit = true;

        if (node[u].dist < uDist)
            continue;
        for (auto& e : node[u].adj) {
            int v = e.second, uvWeight = e.first;

            if (not node[v].visit 
                and (node[v].dist == INF or node[v].dist > node[u].dist + uvWeight)) {
                node[v].dist = node[u].dist + uvWeight;
                pq.push({ node[v].dist, v });
            }
        }
    }
}

int getId(string& s) {  // 把加油站和住宅统一编号
    if (s[0] == 'G') return stoi(s.substr(1)) + n;
    else return stoi(s);
}

int main() {
    scanf("%d%d%d%d", &n, &m, &k, &ds);

    for (int i = 0; i < k; ++i) {
        int d, p1, p2;
        string sp1, sp2;
        cin >> sp1 >> sp2 >> d;

        p1 = getId(sp1);
        p2 = getId(sp2);

        node[p1].adj.push_back({ d, p2 });
        node[p2].adj.push_back({ d, p1 });
    }

    int station_dist_sum = INF, station_min_dist = 0, station_id = INF;

    for (int i = 1; i <= m; ++i) {
        dijkstra(n + i); // 计算这个加油站到所有住宅的最短路径

        bool ok = true;

        for (int j = 1; j <= n; ++j) { // 判断能否覆盖所有住宅
            if (node[j].dist > ds) {
                ok = false;
                break;
            }
        }

        if (ok) {
            int dist_sum = node[1].dist;
            int min_dist = node[1].dist;

            for (int j = 2; j <= n; ++j) { // 计算到所有住宅的距离之和和其中的最短距离
                dist_sum += node[j].dist;
                if (min_dist > node[j].dist) min_dist = node[j].dist;
            }

            if (station_min_dist < min_dist 
                or (station_min_dist == min_dist and station_dist_sum > dist_sum)) { // 更新最短距离最大的加油站位置
                station_id = i;
                station_dist_sum = dist_sum;
                station_min_dist = min_dist;
            }
        }
    }

    if (station_id != INF)
        printf("G%d\n%.1f %.1f", station_id, double(station_min_dist), double(station_dist_sum) / n);
    else
        printf("No Solution");
}
```