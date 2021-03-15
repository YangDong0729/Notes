# 题目详情
Recommendation system predicts the preference that a user would give to an item. Now you are asked to program ==a very simple recommendation system that rates the user's preference by the number of times that an item has been accessed by this user.==

### Input Specification:

Each input file contains one test case. For each test case, the first line contains ==two positive integers: N ($\le$ 50,000), the total number of queries==, and ==K ($\le$ 10), the maximum number of recommendations the system must show to the user.== ==Then given in the second line are the indices of items that the user is accessing -- for the sake of simplicity, all the items are indexed from 1 to N. All the numbers in a line are separated by a space.==

### Output Specification:

For each case, process the queries one by one. Output the recommendations for each query in a line in the format:

    query: rec[1] rec[2] ... rec[K]


where ==`query` is the item that the user is accessing, and `rec[i]` (`i`\=1, ... K) is the `i`\-th item that the system recommends to the user.== ==The first K items that have been accessed most frequently are supposed to be recommended in non-increasing order of their frequencies.== ==If there is a tie, the items will be ordered by their indices in increasing order.==

Note: ==there is no output for the first item since it is impossible to give any recommendation at the time. It is guaranteed to have the output for at least one query.==

### Sample Input:

    12 3
    3 5 7 5 5 3 2 1 8 3 8 12


### Sample Output:

    5: 3
    7: 3 5
    5: 3 5 7
    5: 5 3 7
    3: 5 3 7
    2: 5 3 7
    1: 5 3 2
    8: 5 3 1
    3: 5 3 1
    8: 3 5 1
    12: 3 5 8
# 题解

用一个map存储每个元素的访问次数times。用一个set存储recommend。当访问一个元素时，如果其已经在recommend中，先将其从中删除，再修改times，最后考察是否需要重新插入recommend，维持recommend的大小最大是k，内部根据题意描述的优先级排序。

```cpp
#include <iostream>
#include <set>
#include <unordered_map>

using namespace std;

struct RecommendItem {
    int id, time;

    bool operator<(const RecommendItem &r) const {
        if (time != r.time)
            return time < r.time;
        else
            return id > r.id;
    }
};

unordered_map<int, int> times;
set<RecommendItem> recommend;

int main() {
    int n, k, x;
    cin >> n >> k;

    for (int i = 1; i <= n; ++i) {
        cin >> x;

        if (i > 1) {
            cout << x << ":";
            for (auto j = recommend.rbegin(); j != recommend.rend(); ++j)
                cout << " " << j->id;
            cout << "\n";
        }

        recommend.erase({x, times[x]});
        ++times[x];

        auto item = RecommendItem{x, times[x]};

        if (recommend.size() < k or *recommend.begin() < item) {
            if (recommend.size() == k)
                recommend.erase(recommend.begin());
            recommend.insert(item);
        }
    }
}
```

