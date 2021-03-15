# 题目详情
This time your job is to ==fill a sequence of $N$ positive integers into a **spiral matrix** in non-increasing order==. ==A spiral matrix is filled in from the first element at the upper-left corner, then move in a clockwise spiral.== The matrix has $m$ rows and $n$ columns, where $m$ and $n$ satisfy the following: $m\times n$ must be equal to $N$; $m\ge n$; and $m-n$ is the minimum of all the possible values.

### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N$==. Then the next line contains $N$ positive integers to be filled into the spiral matrix. All the numbers are no more than $10^4$. The numbers in a line are separated by spaces.

### Output Specification:

For each test case, output the resulting matrix in $m$ lines, each contains $n$ numbers. There must be exactly 1 space between two adjacent numbers, and no extra space at the end of each line.

### Sample Input:

    12
    37 76 20 98 76 42 53 95 60 81 58 93


### Sample Output:

    98 95 93
    42 37 81
    53 20 76
    58 60 76
# 题解

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <array>
#include <cmath>

using namespace std;

int main() {
    int num;
    cin >> num;

    vector<int> arr(num);
    for (int i = 0; i < num; ++i) cin >> arr[i];
    sort(arr.begin(), arr.end(), greater<int>());

    int m = 0, n = 0;
    for (int i = 1; i * i <= num; ++i) {
        if (num % i == 0) {
            n = i;
            m = num / n;
        }
    }

    vector<vector<int>> spiral(m, vector<int>(n));
    for (int i = 0, j = 0; j < num; ++i) {
        for (int a = i; a < n - i and j < num; ++a) spiral[i][a] = arr[j++];
        for (int b = i + 1; b < m - i and j < num; ++b) spiral[b][n - i - 1] = arr[j++];
        for (int c = n - i - 2; c >= i and j < num; --c) spiral[m - i - 1][c] = arr[j++];
        for (int d = m - i - 2; d > i and j < num; --d) spiral[d][i] = arr[j++];
    }


    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            cout << spiral[i][j];
            if (j != n - 1) cout << " ";
        }
        cout << "\n";
    }
}
```

