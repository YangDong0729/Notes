# 题目详情
The task of this problem is simple: ==insert a sequence of distinct positive integers into a hash table first. Then try to find another sequence of integer keys from the table and output the average search time== (the number of comparisons made to find whether or not the key is in the table). The ==hash function is defined to be $H(key) = key \% TSize$== where $TSize$ is the maximum size of the hash table. ==Quadratic probing (with positive increments only) is used to solve the collisions.==

Note that the table size is better to be prime. ==If the maximum size given by the user is not prime, you must re-define the table size to be the smallest prime number which is larger than the size given by the user.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains 3 positive numbers: ==MSize, N, and M, which are the user-defined table size, the number of input numbers, and the number of keys to be found==, respectively. All the three numbers are no more than $10^4$. Then N distinct ==positive integers== are given in the next line, followed by M positive integer keys in the next line. All the numbers in a line are separated by a space and are no more than $10^5$.

### Output Specification:

For each test case, in case it is impossible to insert some number, print in a line `X cannot be inserted.` where `X` is the input number. Finally print in a line the average search time for all the M keys, accurate up to 1 decimal place.

### Sample Input:

    4 5 4
    10 6 4 15 11
    11 4 15 2


### Sample Output:

    15 cannot be inserted.
    2.8
# 题解

```cpp
#include <iostream>

using namespace std;

bool isPrime(int x) {
    if (x <= 1) return false;
    if (x == 2) return true;
    if (x % 2 == 0) return false;
    for (int i = 3; i * i <= x; i += 2)
        if (x % i == 0) return false;
    return true;
}

int hash_table[100000];

int main() {
    int m_size, n, m, x, ans = 0;

    cin >> m_size >> n >> m;
    while (not isPrime(m_size)) ++m_size;

    for (int i = 0; i < n; ++i) {
        cin >> x;
        bool insert = false;
        for (int j = 0; j < m_size; ++j) {
            int pos = (x + j * j) % m_size;
            if (hash_table[pos] == 0) {
                hash_table[pos] = x;
                insert = true;
                break;
            }
        }
        if (not insert) cout << x << " cannot be inserted.\n";
    }

    for (int i = 0; i < m; ++i) {
        cin >> x;
        int j = 0;
        for (; j < m_size; ++j) {
            int pos = (x + j * j) % m_size;
            if (hash_table[pos] == 0 or hash_table[pos] == x) break;
        }
        ans += j + 1;
    }

    printf("%.1f", ans * 1.0 / m);
}
```

