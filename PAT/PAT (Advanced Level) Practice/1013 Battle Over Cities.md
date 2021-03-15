# 题目详情
It is vitally important to have all the cities connected by highways in a war. ==If a city is occupied by the enemy, all the highways from/toward that city are closed. We must know immediately if we need to repair any other highways to keep the rest of the cities connected.== ==Given the map of cities which have all the remaining highways marked, you are supposed to tell the number of highways need to be repaired==, quickly.

For example, if we have 3 cities and 2 highways connecting $city_1-city_2$ and $city_1-city_3$. Then if $city_1$ is occupied by the enemy, we must have 1 highway repaired, that is the highway $city_2-city_3$.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 3 numbers ==$N$ ($<1000$), $M$ and $K$, which are the total number of cities, the number of remaining highways, and the number of cities to be checked==, respectively. Then $M$ lines follow, each describes a highway by 2 integers, which are the numbers of the cities the highway connects. The cities are numbered from 1 to $N$. Finally there is a line containing $K$ numbers, which represent the cities we concern.

### Output Specification:

==For each of the $K$ cities, output in a line the number of highways need to be repaired if that city is lost.==

### Sample Input:

    3 2 3
    1 2
    1 3
    1 2 3


### Sample Output:

    1
    0
    0
# 题解

### 深度优先搜索

```cpp
#include <algorithm>
#include <cstdio>
#include <vector>

using namespace std;

const int N = 1111;

vector<int> g[N];  // 邻接表
bool vis[N];       // 访问标志
int deleteP;       // 需要删除的顶点

void dfs(int v) {
  vis[v] = true;
  for (int i = 0; i < g[v].size(); ++i)
    if (not vis[g[v][i]] and g[v][i] != deleteP) 
        dfs(g[v][i]);
}

int n, m, k;

int main() {
  scanf("%d%d%d", &n, &m, &k);
  for (int i = 0; i < m; ++i) {
    int a, b;
    scanf("%d%d", &a, &b);
    g[a].push_back(b);
    g[b].push_back(a);
  }

  for (int q = 0; q < k; ++q) {
    scanf("%d", &deleteP);

    fill_n(vis, N, false);

    int block = 0;
    for (int i = 1; i <= n; ++i) {
      if (i != deleteP and not vis[i]) {
        dfs(i);
        ++block;
      }
    }

    printf("%d\n", block - 1);
  }
}
```

### 并查集

```cpp
#include <cstdio>
#include <vector>

using namespace std;

const int N = 1111;
vector<int> g[N];

int father[N];
bool vis[N];

int findFather(int x) {
    int a = x;
    while (x != father[x]) x = father[x];
    while (a != father[a]) {
        int t = a;
        a = father[a];
        father[t] = x;
    }
    return x;
}

void merge(int a, int b) {
    int faA = findFather(a);
    int faB = findFather(b);
    if (faA != faB) father[faA] = faB;
}

void init() {
    for (int i = 1; i < N; ++i) {
        father[i] = i;
        vis[i] = false;
    }
}

int n, m, k;
int main() {
    scanf("%d%d%d", &n, &m, &k);
    for (int i = 0; i < m; ++i) {
        int a, b;
        scanf("%d%d", &a, &b);
        g[a].push_back(b);
        g[b].push_back(a);
    }

    int currentPoint; // 当前需要删除的顶点编号
    for (int q = 0; q < k; ++q) {
        init();
        scanf("%d", &currentPoint);
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < g[i].size(); ++j) {
                int u = i, v = g[i][j];
                if (u == currentPoint or v == currentPoint) continue;
                merge(u, v);
            }
        }
        int block = 0;
        for (int i = 1; i <= n; ++i) {
            if (i == currentPoint) continue;
            int fa_i = findFather(i);
            if (not vis[fa_i]) {
                ++block;
                vis[fa_i] = true;
            }
        }
        printf("%d\n", block - 1);
    }
}
```