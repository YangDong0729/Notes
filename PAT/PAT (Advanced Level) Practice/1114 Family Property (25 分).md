# 题目详情
This time, you are supposed to help us collect the data for family-owned property. ==Given each person's family members, and the estate（房产）info under his/her own name, we need to know the size of each family, and the average area and number of sets of their real estate.==

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N$ ($\le 1000$). Then $N$ lines follow, each gives the infomation of a person who owns estate in the format:==

`ID`  `Father`  `Mother`  $k$ $Child_1 \cdots Child_k$ $M_{estate}$ `Area`

where `ID` is a unique 4-digit identification number for each person; `Father` and `Mother` are the `ID`'s of this person's parents (if a parent has passed away, `-1` will be given instead); $k$ ($0\le k\le 5$) is the number of children of this person; $Child_i$'s are the `ID`'s of his/her children; $M_{estate}$ is the total number of sets of the real estate under his/her name; and `Area` is the total area of his/her estate.

### Output Specification:

For each case, ==first print in a line the number of families (all the people that are related directly or indirectly are considered in the same family)==. Then output the family info in the format:

`ID` `M` $AVG_{sets}$ $AVG_{area}$

where  ==`ID` is the smallest ID in the family; `M` is the total number of family members;== $AVG_{sets}$ is the average number of sets of their real estate; and $AVG_{area}$ is the average area. The average numbers must be accurate up to 3 decimal places. ==The families must be given in descending order of their average areas, and in ascending order of the ID's if there is a tie.==

### Sample Input:

    10
    6666 5551 5552 1 7777 1 100
    1234 5678 9012 1 0002 2 300
    8888 -1 -1 0 1 1000
    2468 0001 0004 1 2222 1 500
    7777 6666 -1 0 2 300
    3721 -1 -1 1 2333 2 150
    9012 -1 -1 3 1236 1235 1234 1 100
    1235 5678 9012 0 1 50
    2222 1236 2468 2 6661 6662 1 300
    2333 -1 3721 3 6661 6662 6663 1 100


### Sample Output:

    3
    8888 1 1.000 1000.000
    0001 15 0.600 100.000
    5551 4 0.750 100.000
# 题解

### DFS

```cpp
#include <algorithm>
#include <cstdio>
#include <iostream>
#include <vector>

using namespace std;

struct {
    int estate = 0, area = 0, visit = 1;
    vector<int> children, parents;
} person[10005];

struct Family {
    int memberCnt = 0, estateCnt = 0, area = 0, id = 10000;

    double avgArea() const {
        return area * 1.0 / memberCnt;
    }

    bool operator<(const Family &x) const {
        if (avgArea() != x.avgArea())
            return avgArea() > x.avgArea();
        else
            return id < x.id;
    }
};

vector<Family> family;

void dfs(int id, Family &f) {
    person[id].visit = 1;
    f.memberCnt += 1;
    f.estateCnt += person[id].estate;
    f.area += person[id].area;
    f.id = min(f.id, id);

    for (auto &c : person[id].children)
        if (not person[c].visit)
            dfs(c, f);
    for (auto &p : person[id].parents)
        if (not person[p].visit)
            dfs(p, f);
}

void addParent(int id, int parentId) {
    person[id].visit = 0;
    if (parentId != -1) {
        person[parentId].visit = 0;
        person[id].parents.push_back(parentId);
        person[parentId].children.push_back(id);
    }
}

int main() {
    int n, id, k, c, father, mother;
    cin >> n;

    for (int i = 0; i < n; ++i) {
        cin >> id >> father >> mother >> k;
        addParent(id, father);
        addParent(id, mother);
        for (int j = 0; j < k; ++j) {
            cin >> c;
            addParent(c, id);
        }
        cin >> person[id].estate >> person[id].area;
    }

    for (int i = 0; i < 10005; ++i) {
        if (not person[i].visit) {
            Family f;
            dfs(i, f);
            family.push_back(f);
        }
    }

    sort(family.begin(), family.end());

    cout << family.size() << "\n";
    for (auto &f : family)
        printf("%04d %d %.3f %.3f\n", f.id, f.memberCnt,
               f.estateCnt * 1.0 / f.memberCnt, f.area * 1.0 / f.memberCnt);
}
```

### 并查集

```cpp
#include <cstdio>
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>

using namespace std;

#define MAX 10005

struct Person {
    int estate = 0, area = 0, root, valid, id;
} person[MAX];

void init(int id) {
    if (id == -1) return;
    if (not person[id].valid) {
        person[id].valid = true;
        person[id].root = id;
    }
}

int findRoot(int id) {
    Person& p = person[id];
    if (p.root != id)
        p.root = findRoot(p.root);
    return p.root;
}

void merge(int a, int b) {
    if (a == -1 or b == -1) return;
    int rootA = findRoot(a);
    int rootB = findRoot(b);
    person[rootA].root = min(rootA, rootB);
    person[rootB].root = min(rootA, rootB);
}

struct Family {
    int memberCnt = 0, estate = 0, area = 0, valid, id = MAX;

    double avgArea() const {
        return area * 1.0 / memberCnt;
    }

    double avgEstate() const {
        return estate * 1.0 / memberCnt;
    }

    bool operator<(const Family& x) const {
        if (valid != x.valid)  return valid > x.valid;
        else if (avgArea() != x.avgArea())
            return avgArea() > x.avgArea();
        else return id < x.id;
    }
} family[MAX];

int main() {
    for (int i = 0; i < MAX; ++i) person[i].id = i;

    int n, id, relative;

    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> id;
        init(id);

        int k = 2;
        for (int j = 0; j < 2; ++j) {
            if (j) cin >> k;
            for (int l = 0; l < k; ++l) {
                cin >> relative;
                init(relative);
                merge(id, relative);
            }
        }

        cin >> person[id].estate >> person[id].area;
    }

    for (int i = 0; i < MAX; ++i) {
        Person& p = person[i];
        if (not p.valid) continue;
        int root = findRoot(i);
        Family& f = family[root];

        f.id = min(f.id, p.id);
        f.valid = true;
        f.memberCnt++;
        f.estate += p.estate;
        f.area += p.area;
    }

    sort(family, family + MAX);

    int familyCnt = 0;
    while (family[familyCnt].valid) familyCnt++;

    cout << familyCnt << "\n";
    for (int i = 0; i < familyCnt; ++i) {
        Family& f = family[i];
        printf("%04d %d %.3f %.3f\n", f.id, f.memberCnt, f.avgEstate(), f.avgArea());
    }
}
```

