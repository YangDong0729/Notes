# 题目详情
In the big cities, the subway systems always look so complex to the visitors. To give you some sense, the following figure shows the map of Beijing subway. Now you are supposed to help people with your computer skills! ==Given the starting position of your user, your task is to find the quickest way to his/her destination.==

![subwaymap.jpg](https://images.ptausercontent.com/55799c23-4bdb-4e32-af7f-6d41accfdd2b.jpg)

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==a positive integer $N$ ($\le 100$), the number of subway lines==. Then $N$ lines follow, with the $i$\-th ($i=1, \cdots , N$) line describes the ii\-th subway line in the format:

$M$ S\[1\] S\[2\] ... S\[$M$\]

where ==$M$ ($\le 100$) is the number of stops, and S\[$i$\]'s (i\=$1, \cdots , M$) are the indices of the stations (the indices are 4-digit numbers from 0000 to 9999) along the line==. It is guaranteed that ==the stations are given in the correct order -- that is, the train travels between S\[$i$\] and S\[$i$+1\] ($1, \cdots , M-1$) without any stop.==

Note: ==It is possible to have loops, but not self-loop== (no train starts from S and stops at S without passing through another station). ==Each station interval belongs to a unique subway line.== ==Although the lines may cross each other at some stations (so called "transfer stations"), no station can be the conjunction of more than 5 lines.==

After the description of the subway, another ==positive integer $K$ ($\le 10$) is given. Then $K$ lines follow, each gives a query from your user: the two indices as the starting station and the destination, respectively.==

The following figure shows the sample map.

![samplemap.jpg](https://images.ptausercontent.com/932c8f1b-7dd5-489d-a774-a91c1fabba7f.jpg)

Note: It is guaranteed that all the stations are reachable, and all the queries consist of legal station numbers.

### Output Specification:

For each query, ==first print in a line the minimum number of stops. Then you are supposed to show the optimal path in a friendly format== as the following:

    Take Line#X1 from S1 to S2.
    Take Line#X2 from S2 to S3.
    ......


where `X`ii's are the line numbers and `S`ii's are the station indices. Note: ==Besides the starting and ending stations, only the transfer stations shall be printed.==

If the quickest path is not unique, ==output the one with the minimum number of transfers, which is guaranteed to be unique.==

### Sample Input:

    4
    7 1001 3212 1003 1204 1005 1306 7797
    9 9988 2333 1204 2006 2005 2004 2003 2302 2001
    13 3011 3812 3013 3001 1306 3003 2333 3066 3212 3008 2302 3010 3011
    4 6666 8432 4011 1306
    3
    3011 3013
    6666 2001
    2004 3001


### Sample Output:

    2
    Take Line#3 from 3011 to 3013.
    10
    Take Line#4 from 6666 to 1306.
    Take Line#3 from 1306 to 2302.
    Take Line#2 from 2302 to 2001.
    6
    Take Line#2 from 2004 to 1204.
    Take Line#1 from 1204 to 1306.
    Take Line#3 from 1306 to 3001.
# 题解

单源最短路径问题，结合用深搜找出符合题意的路径。

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <map>
#include <queue>
#include <vector>

using namespace std;

#define MAX_N 10005

array<int, MAX_N> dist;
array<bool, MAX_N> visit;
map<pair<int, int>, int> line;
vector<int> pre[MAX_N], graph[MAX_N];

void dijkstra(int start) {
	dist.fill(-1);
	visit.fill(false);

	using pair = pair<int, int>;
	priority_queue<pair, vector<pair>, greater<pair>> pq;

	dist[start] = 0;
	pre[start].clear();
	pre[start].push_back(-1);
	pq.push({ 0, start });

	while (!pq.empty()) {
		int u = pq.top().second, u_dist = pq.top().first;

		pq.pop();
		visit[u] = true;

		if (dist[u] != -1 and dist[u] < u_dist)
			continue; // dist[u] != -1
		for (auto& v : graph[u]) {
			if (!visit[v] and (dist[v] == -1 or dist[v] > u_dist + 1)) {
				dist[v] = u_dist + 1;
				pq.push({ dist[v], v });
				pre[v].clear();
				pre[v].push_back(u);
			}
			else if (dist[v] == u_dist + 1)
				pre[v].push_back(u);
		}
	}
}

int min_trans_cnt = MAX_N;
vector<int> path, tmp_path;

void trace(int x, int l, int trans_cnt) {
	if (trans_cnt > min_trans_cnt)
		return;
	if (pre[x][0] == -1) {
		if (min_trans_cnt > trans_cnt) {
			path = tmp_path;
			path.push_back(x);
			min_trans_cnt = trans_cnt;
		}
		return;
	}

	tmp_path.push_back(x);
	for (auto& v : pre[x]) {
		trace(v, line[{v, x}], trans_cnt + (line[{v, x}] != l));  // 这里 (line[{v, x}] != l) 需要加括号
		// 但我之前用 unordered_map 时，由于不能用 pair ，所以哈希了一下，写成了 line[v * 10000 + x] ，这种不写括号竟然也对了
	}
	tmp_path.pop_back();
}

int main() {
	int n, m, pre_stop, k, start, dest;

	cin >> n;
	for (int i = 1; i <= n; ++i) {
		cin >> m >> pre_stop;
		for (int j = 1, stop; j < m; ++j, pre_stop = stop) {
			cin >> stop;
			line[{pre_stop, stop}] = i;
			line[{stop, pre_stop}] = i;
			graph[pre_stop].push_back(stop);
			graph[stop].push_back(pre_stop);
		}
	}

	cin >> k;
	for (int i = 0; i < k; ++i) {
		cin >> start >> dest;

		dijkstra(start);
		min_trans_cnt = MAX_N;
		trace(dest, 0, 0);
		reverse(path.begin(), path.end());

		cout << path.size() - 1 << "\n";
		int p = path[0], seg = path[0], l = line[{path[0], path[1]}];
		for (auto j = path.begin() + 1; j != path.end(); p = *(j++)) {
			if (l != line[{p, * j}]) {
				printf("Take Line#%d from %04d to %04d.\n", l, seg, p);
				l = line[{p, * j}];
				seg = p;
			}
		}
		printf("Take Line#%d from %04d to %04d.\n", l, seg, p);
	}
}
```

