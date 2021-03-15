# 题目详情
When register on a social network, you are always asked to specify your hobbies in order to find some potential friends with the same hobbies. ==A **social cluster** is a set of people who have some of their hobbies in common. You are supposed to find all the clusters.==

### Input Specification:

Each input file contains one test case. For each test case, the first line contains ==a positive integer $N$ ($\le 1000$), the total number of people in a social network==. Hence the people are numbered from 1 to $N$. Then $N$ lines follow, each gives the hobby list of a person in the format:

$$K_i: h_i[1] h_i[2] ... h_i[K_i]$$

where $K_i (>0)$ is the number of hobbies, and $h_i[j]$ is the index of the $j$-th hobby, which is an integer in \[1, 1000\].

### Output Specification:

==For each case, print in one line the total number of clusters in the network==. ==Then in the second line, print the numbers of people in the clusters in non-increasing order==. The numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

    8
    3: 2 7 10
    1: 4
    2: 5 3
    1: 4
    1: 3
    1: 4
    4: 6 8 1 5
    1: 4


### Sample Output:

    3
    4 3 1
# 题解

```cpp
#include <cstdio>
#include <algorithm>
#include <functional>  // 使用greater

using namespace std;

const int SIZE = 1010;

int father[SIZE];  // 存放父节点
int isRoot[SIZE];  // 记录每个节点是否作为某个集合的根节点
int hobby[SIZE];

int findFather(int x) {  // 查找x所在集合的根节点
    int a = x;
    while (x != father[x]) {
        x = father[x];
    }

    // 路径压缩
    while (a != father[a]) {
        int z = a;
        a = father[a];
        father[z] = x;
    }

    return x;
}

void unin(int a, int b) {  // 合并a和b所在的集合
    int faA = findFather(a);
    int faB = findFather(b);

    father[faA] = faB;
}

void init(int n) {  // 初始化father[i]为i，且flag[i]为false
    for (int i = 1; i <= n; ++i) {
        father[i] = i;
        isRoot[i] = false;
    }
}

int main() {
    int n, k, h;
    scanf("%d", &n);

    init(n);

    for (int i = 1; i <= n; ++i) {
        scanf("%d:", &k);
        for (int j = 0; j < k; ++j) {
            scanf("%d", &h);
            if (hobby[h] == 0) {
                hobby[h] = i;
            }
            unin(i, findFather(hobby[h]));
        }
    }

    for (int i = 1; i <= n; ++i) {
        ++isRoot[findFather(i)];
    }

    int ans = 0;
    for (int i = 1; i <= n; ++i) {
        if (isRoot[i] != 0) ++ans;
    }

    printf("%d\n", ans);
    sort(isRoot + 1, isRoot + n + 1, greater<int>());
    for (int i = 1; i <= ans; ++i) {
        printf("%d", isRoot[i]);
        if (i < ans) printf(" ");
    }
}
```

