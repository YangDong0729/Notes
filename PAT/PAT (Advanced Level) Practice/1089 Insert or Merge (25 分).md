# 题目详情
According to Wikipedia:

**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.

**Merge sort** works as follows: Divide the unsorted list into N sublists, each containing 1 element (a list of 1 element is considered sorted). Then repeatedly merge two adjacent sublists to produce new sorted sublists until there is only 1 sublist remaining.

==Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?==

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N$ ($\le 100$). Then in the next line, $N$ integers are given as the initial sequence. The last line contains the partially sorted sequence of the $N$ numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

### Output Specification:

==For each test case, print in the first line either "Insertion Sort" or "Merge Sort" to indicate the method used to obtain the partial result==. ==Then run this method for one more iteration and output in the second line the resuling sequence.== It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input 1:

    10
    3 1 2 8 7 5 9 4 6 0
    1 2 3 7 8 5 9 4 6 0


### Sample Output 1:

    Insertion Sort
    1 2 3 5 7 8 9 4 6 0


### Sample Input 2:

    10
    3 1 2 8 7 5 9 4 0 6
    1 3 2 8 5 7 4 9 0 6


### Sample Output 2:

    Merge Sort
    1 2 3 8 4 5 7 9 0 6
# 题解

给出一个序列和这个序列在排序过程中的中间结果，判断是插入排序还是归并排序，并给出下一步的结果。



插入排序的中间结果有一个明显的区分已排序部分和未排序部分的界限，而归并排序没有。可以据此判断排序的种类。使用sort可以轻松地获得插入排序下一步的结果。归并排序稍微复杂一些，要使用sort模拟归并排序的过程，得出当前归并的长度，然后再使用sort归并一次。

```cpp
#include <algorithm>
#include <iostream>

using namespace std;

int main() {
    int n, original[100], intermediate[100], i, j;

    // 读入数组
    cin >> n;
    for (i = 0; i < n; i++)
        cin >> original[i];
    for (i = 0; i < n; i++)
        cin >> intermediate[i];

    // 找到一个有序前缀
    for (i = 0; i < n - 1 and intermediate[i] <= intermediate[i + 1]; i++)
        ;
    // 判断之后的序列是否相同
    for (j = i + 1; original[j] == intermediate[j] and j < n; j++)
        ;

    if (j == n) {
        cout << "Insertion Sort" << endl;
        sort(original, original + i + 2);
    } else {
        cout << "Merge Sort" << endl;
        int len = 1, different = 1;
        while (different) {
            different = 0;
            // 判断是否到达了中间结果
            for (i = 0; i < n; i++) {
                if (original[i] != intermediate[i])
                    different = 1;
            }
            len = len * 2;
            for (i = 0; i < n / len; i++)
                sort(original + i * len, original + (i + 1) * len);
            sort(original + n / len * len, original + n);
        }
    }

    for (j = 0; j < n; j++) {
        if (j != 0)
            printf(" ");
        printf("%d", original[j]);
    }
}
```

