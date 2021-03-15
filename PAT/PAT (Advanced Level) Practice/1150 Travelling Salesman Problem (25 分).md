# 题目详情
The "travelling salesman problem" asks the following question: "Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city and returns to the origin city?" It is an NP-hard problem in combinatorial optimization, important in operations research and theoretical computer science. (Quoted from "[https://en.wikipedia.org/wiki/Travelling\_salesman\_problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem)".)

==In this problem, you are supposed to find, from a given list of cycles, the one that is the closest to the solution of a travelling salesman problem.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains ==2 positive integers $N (2<N≤200)$, the number of cities, and $M$, the number of edges in an undirected graph.== Then $M$ lines follow, each describes ==an edge in the format `City1 City2 Dist`, where the cities are numbered from 1 to $N$ and the distance `Dist` is positive and is no more than 100.== The next line gives ==a positive integer $K$ which is the number of paths, followed by $K$ lines of paths==, each in the format:

$$n \ C_1 \ C_2 \ ... \ C_n$$

where ==$n$ is the number of cities in the list, and $C_i$'s are the cities on a path.==

### Output Specification:

For each path, print in a line `Path X: TotalDist (Description)` where `X` is the index (starting from 1) of that path, `TotalDist` its total distance (==if this distance does not exist, output `NA` instead==), and `Description` is one of the following:

*   `TS simple cycle` if it is ==a simple cycle that visits every city;==
*   `TS cycle` if it is ==a cycle that visits every city, but not a simple cycle;==
*   `Not a TS cycle` if it is ==NOT a cycle that visits every city.==

Finally print in a line `Shortest Dist(X) = TotalDist` where `X` is the index of the cycle that is the closest to the solution of a travelling salesman problem, and `TotalDist` is its total distance. It is guaranteed that such a solution is unique.

### Sample Input:

    6 10
    6 2 1
    3 4 1
    1 5 1
    2 5 1
    3 1 8
    4 1 6
    1 6 1
    6 3 1
    1 2 1
    4 5 1
    7
    7 5 1 4 3 6 2 5
    7 6 1 3 4 5 2 6
    6 5 1 4 3 6 2
    9 6 2 1 6 3 4 5 2 6
    4 1 2 5 1
    7 6 1 2 5 4 3 1
    7 6 3 2 5 4 1 6


### Sample Output:

    Path 1: 11 (TS simple cycle)
    Path 2: 13 (TS simple cycle)
    Path 3: 10 (Not a TS cycle)
    Path 4: 8 (TS cycle)
    Path 5: 3 (Not a TS cycle)
    Path 6: 13 (Not a TS cycle)
    Path 7: NA (Not a TS cycle)
    Shortest Dist(4) = 8
# 题解

先判断路径是否存在，再判断是否是经过了所有城市的环，最后判断是否是简单环。

```CPP
#include <iostream>
#include <set>
#include <algorithm>
#include <unordered_set>

using namespace std;


struct node {
    int v, w;
};

int graph[205][205], path[205];
unordered_set<int> visitedCity;
set<pair<int, int>> visitedEdge;

int main() {

    int n, m, u, v, w, k;

    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        cin >> u >> v >> w;
        graph[u][v] = graph[v][u] = w;
    }

    cin >> k;
    int minWeight = 1e9, minIndex;
    for (int i = 1; i <= k; ++i) {
        // 读入路径并判断路径是否存在
        bool isExist = true;
        int pathLen, totalWeight = 0;

        cin >> pathLen;
        visitedCity.clear();
        for (int j = 0; j < pathLen; ++j) {
            cin >> path[j];
            if (isExist) {
                if (j > 0) {
                    if (graph[path[j]][path[j - 1]] == 0) isExist = false;
                    else totalWeight += graph[path[j]][path[j - 1]];
                }
                visitedCity.insert(path[j]);
            }
        }

        cout << "Path " << i << ": ";

        if (not isExist) {
            cout << "NA (Not a TS cycle)\n";
            continue;
        }

        // 判断是否是经过了所有城市的环
        cout << totalWeight << " ";
        if (not (visitedCity.size() == n and path[0] == path[pathLen - 1])) {
            cout << "(Not a TS cycle)\n";
            continue;
        }

        if (minWeight > totalWeight) {
            minWeight = totalWeight;
            minIndex = i;
        }

        // 判断是否是简单环
        bool isSimple = true;

        visitedEdge.clear();
        for (int j = 0; j < pathLen - 1 and isSimple; ++j) {
            int u = path[j];
            int v = path[j + 1];
            if (u > v) swap(u, v);
            if (visitedEdge.count({ u, v }) == 0) visitedEdge.insert({ u, v });
            else if (visitedEdge.count({ u, v }) == 1) isSimple = false;
        }

        if (isSimple) cout << "(TS simple cycle)\n";
        else cout << "(TS cycle)\n";
    }
    cout << "Shortest Dist(" << minIndex << ") = " << minWeight;
}
```

