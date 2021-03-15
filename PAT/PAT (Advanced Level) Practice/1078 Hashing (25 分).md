The task of this problem is simple: insert a sequence of distinct positive integers into a hash table, and output the positions of the input numbers. The hash function is defined to be $H(key)=key%TSize$ where $TSize$ is the maximum size of the hash table. Quadratic probing (with positive increments only) is used to solve the collisions.

Note that the table size is better to be prime. If the maximum size given by the user is not prime, you must re-define the table size to be the smallest prime number which is larger than the size given by the user.

### Input Specification:
Each input file contains one test case. For each case, the first line contains two positive numbers: $MSize (≤10^4)$ and $N (≤MSize)$ which are the user-defined table size and the number of input numbers, respectively. Then $N$ distinct positive integers are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:
==For each test case, print the corresponding positions (index starts from 0) of the input numbers in one line. All the numbers in a line are separated by a space, and there must be no extra space at the end of the line. In case it is impossible to insert the number, print "-" instead.==

### Sample Input:
```
4 4
10 6 4 15
```
### Sample Output:
```
0 1 4 -
```
# 题解

给出散列表长 T 和将要插入的元素，将这些元素按照读入的顺序插入散列表中。散列采用除留取余法，解决冲突采用正向增加的二次探查法。如果 T 不是素数，需要寻找第一个大于 T 的素数。
```cpp
#include <cmath>
#include <iostream>
#include <vector>

using namespace std;

bool isPrime(int n) {
    if (n <= 1)
        return false;
    for (int i = 2; i * i <= n; ++i)
        if (n % i == 0)
            return false;
    return true;
}

bool hashTable[10005];

void insert(int key, int size) {
    for (int step = 0; step < size; ++step) {
        int pos = (key + step * step) % size;
        if (not hashTable[pos]) {
            hashTable[pos] = true;
            cout << pos;
            return;
        }
    }
    cout << "-";
}

int main() {
    int n, size, key;
    cin >> size >> n;
    while (not isPrime(size))
        ++size;
    for (int i = 0; i < n; ++i) {
        cin >> key;
        if (i != 0)
            cout << " ";
        insert(key, size);
    }
}
```