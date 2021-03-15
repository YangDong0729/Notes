# 题目详情
With highways available, driving a car from Hangzhou to any other city is easy. But since the tank capacity of a car is limited, we have to find gas stations on the way from time to time. Different gas station may give different price. ==You are asked to carefully design the cheapest route to go.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 positive numbers: ==$C_{max}$ ($\le$ 100), the maximum capacity of the tank==; ==$D$ ($\le$30000), the distance between Hangzhou and the destination city==; ==$D_{=avg}$ ($\le$20), the average distance per unit gas that the car can run==; and ==$N$ ($\le$ 500), the total number of gas stations==. Then $N$ lines follow, each contains a pair of non-negative numbers: ==$P_i$, the unit gas price==, and ==$D_i$ ($\le D$), the distance between this station and Hangzhou==, for $i=1,\cdots ,N$. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the cheapest price in a line, accurate up to 2 decimal places. It is assumed that the tank is empty at the beginning. If it is impossible to reach the destination, print `The maximum travel distance = X` where `X` is the maximum possible distance the car can run, accurate up to 2 decimal places.

### Sample Input 1:

    50 1300 12 8
    6.00 1250
    7.00 600
    7.00 150
    7.10 0
    7.20 200
    7.50 400
    7.30 1000
    6.85 300


### Sample Output 1:

    749.17


### Sample Input 2:

    50 1300 12 2
    7.10 0
    7.00 600


### Sample Output 2:

    The maximum travel distance = 1200.00

# 题解

### 题意

从起点到终点上有许多加油站。车的油箱有最大油量。假设开始没有油，给出每个加油站距离起点的距离和油价，计算从起点到终点所需的最小的油量，如果不能到达就计算能走的最远距离。

### 思路

有一种特殊情况：第一个加油站不在起点，那么无法行走。
贪心思想：总是在尽可能便宜的加油站加尽可能多的油。在终点设置一个油价为0的加油站，排除上述特殊情况，问题变为从第1个加油站走到最后一个加油站。在每个加油站时，遍历所有能到达的加油站，维持油价最低的那个。

- 如果遇到了一个油价不高于当前的加油站，那么必到达这个加油站。而且尽量不加油，保证能够行驶到这个加油站就可以。
- 如果遍历完还没有油价不超过当前的加油站，那么就要在当前加油站加满油，然后行驶到油价最低的加油站。

### 代码

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

struct Station {
	double price, dist;
} station[600];

int main() {
	int n;
	double tank_cap, dist, unit_dist, dist_can_go, gone = 0, left_fuel = 0, cost = 0;

	cin >> tank_cap >> dist >> unit_dist >> n;
	dist_can_go = tank_cap * unit_dist;

	for (int i = 0; i < n; ++i) cin >> station[i].price >> station[i].dist;
	sort(station, station + n, [](Station& a, Station& b) { return a.dist < b.dist; });

	if (station[0].dist != 0) {
		cout << "The maximum travel distance = 0.00";
		return 0;
	}

	station[n].dist = dist;

	for (Station* iter = station, *next; iter != station + n;) {
		if ((iter + 1)->dist > gone + dist_can_go) {
			printf("The maximum travel distance = %.2f", gone + dist_can_go);
			return 0;
		}

		next = iter + 1;
		for (auto i = next; i <= station + n and i->dist <= gone + dist_can_go; ++i) {
			if (i->price <= next->price) {
				next = i;
				if (i->price <= iter->price) break;
			}
		}

		double len = next->dist - iter->dist, fuel = len / unit_dist;
		if (next->price <= iter->price) {
			left_fuel -= fuel;
			if (left_fuel < fuel) {
				cost += (-left_fuel) * iter->price;
				left_fuel = 0;
			}
		}
		else {
			cost += (tank_cap - left_fuel) * iter->price;
			left_fuel = tank_cap - len / unit_dist;
		}

		gone += len;
		iter = next;
	}
	printf("%.2f", cost);
}
```

