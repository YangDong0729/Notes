# 题目详情

==A graph which is connected and acyclic can be considered a tree.== ==The height of the tree depends on the selected root. Now you are supposed to find the root that results in a highest tree. Such a root is called **the deepest root**.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==a positive integer $N$ ($\le 10^4$) which is the number of nodes, and hence the nodes are numbered from 1 to $N$. Then $N-1$ lines follow, each describes an edge by given the two adjacent nodes' numbers.==

### Output Specification:

For each test case, print each of the deepest roots in a line. ==If such a root is not unique, print them in increasing order of their numbers. In case that the given graph is not a tree, print `Error: K components` where `K` is the number of connected components in the graph.==

### Sample Input 1:

    5
    1 2
    1 3
    1 4
    2 5

### Sample Output 1:

    3
    4
    5


### Sample Input 2:

    5
    1 3
    1 4
    2 5
    3 4


### Sample Output 2:

    Error: 2 components

# 题解

给出N个节点和N-1条边，问他们能否形成一颗N个节点的树，如果能则从中选出节点作为树根，使得整棵树的高度最大，输出所有满足要求的可以作为树根的节点。

有N个节点，边数为N-1的图只要连通就一定是树，所以只要判断是否连通即可，可以用dfs或者并查集解决。在确定是树后，还要找出所有“最深的根”。比较好想的方法是从每个节点开始进行一次dfs（在PAT上能够通过），就能找到所有的答案。但是效率较高的方法只需要两次dfs。

**称最长的路径为直径，那么任意两条直径一定相交（或有一段公共路径）。**

不然的话，假设有两条不相交的直径（像这样`| |`），在这两条路径上各选择一个点，由于这是个连通图，这两个点连通，把这两个点连起来（变成了这样`H`），这个H型的图可以组成4条路径，其中至少有一条比原来的两条直径中的某一条长，矛盾。这样就可以继续推出所有的直径一定交于同一点或有一段公共路径。

 **从任意节点出发，得到的最深节点的集合A一定是所求根节点集合的一部分。**

假设从a节点出发到达b的路径最远，而b不是“最深的根”，假设一条直径是cd，设c和d的在a是根节点时的公共祖先是o，根据树的性质cd=co+od，而且一定有ob>=max(oc, od)（因为ab=ao+ob，而且ab是以a为根时最长的路径，即ab>=max(ac, ad)），那么max(cb, db)>=cd，又由于cd是直径，所以max(cb, db)==cd，ab是直径，矛盾。

**从b出发得到的最深节点的集合B与A的并集就是答案。**

由于所有的直径一定有一段公共路径或者是一个公共点，那么这段公共路径R（或者公共点R）就可以把所有的最深的根分成两部分，分别设为S、T。显然S中的点满足到R的距离相同，T也一样。任取一点，dfs得到的最长路径的另一个端点的集合一定等于S或T，再从S或T中的一点开始dfs，就可以得到T或S，而S+T就是最终的答案。

 这道题由于之前要用dfs判断是否连通，所以在寻找“最深的根节点”时只需要再dfs一次即可。

```cpp
#include <cstdio>
#include <set>
#include <vector>
#include <algorithm>

using namespace std;

int maxH = 0;
bool visit[10010];
set<int> sub, ans;
vector<int> graph[10010];

void dfs(int u, int height) {
    visit[u] = true;

    if (height > maxH) {  // 如果获得了更大的树高
        sub.clear();
        sub.insert(u);
        maxH = height;
    }
    else if (height == maxH)
        sub.insert(u);

    for (int i = 0; i < graph[u].size(); ++i)
        if (!visit[graph[u][i]]) dfs(graph[u][i], height + 1);
}

int main() {
    int a, b, n;
    scanf("%d", &n);

    for (int i = 1; i < n; ++i) {
        scanf("%d%d", &a, &b);
        graph[a].push_back(b);
        graph[b].push_back(a);
    }

    int block = 0;
    for (int i = 1; i <= n; ++i) {
        if (!visit[i]) {
            ++block;
            dfs(i, 1);
        }
    }

    if (block > 1) printf("Error: %d components\n", block);
    else {
        ans = sub;
        fill_n(visit + 1, n, false);
        dfs(*sub.begin(), 1);
        for (auto& e : sub) ans.insert(e);
        for (auto& e : ans) printf("%d\n", e);
    }
}
```
