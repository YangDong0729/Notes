According to Wikipedia:


**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.


**Heap sort** divides its input into a sorted and an unsorted region, and it iteratively shrinks the unsorted region by extracting the largest element and moving that to the sorted region. it involves the use of a heap data structure rather than a linear-time search to find the maximum.


Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?
### Input Specification:

Each input file contains one test case. For each case, the first line gives ==a positive integer $N \ (≤100)$==. Then in the next line, $N$ integers are given as the initial sequence. The last line contains the partially sorted sequence of the $N$ numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.
### Output Specification:


For each test case, ==print in the first line either "Insertion Sort" or "Heap Sort" to indicate the method used to obtain the partial result==. ==Then run this method for one more iteration and output in the second line the resulting sequence==. It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.
### Sample Input 1:

```
10
3 1 2 8 7 5 9 4 6 0
1 2 3 7 8 5 9 4 6 0
```
### Sample Output 1:

```
Insertion Sort
1 2 3 5 7 8 9 4 6 0
```
### Sample Input 2:

```
10
3 1 2 8 7 5 9 4 6 0
6 4 5 1 0 3 2 7 8 9
```
### Sample Output 2:

```
Heap Sort
5 4 3 1 0 2 6 7 8 9
```
# 题解

给出两个序列，判断是插入排序还是堆排序，然后再进行一步。



插入排序不会改变未排序部分的顺序。
```cpp
#include <algorithm>
#include <iostream>

using namespace std;

int n, ori[111], mid[111];

void downadjust(int low, int high) {                            // 对heap数组在[low, high]范围进行调整
    for (int i = low, j = i * 2; j <= high; i = j, j = i * 2) { // i为欲调整节点，j为其左孩子节点
        if (j + 1 <= high and mid[j + 1] > mid[j])
            j = j + 1; // 找出左右孩子较大的一个
        if (mid[j] > mid[i])
            swap(mid[j], mid[i]); // 如果孩子节点中最大的权值比父亲节点大，交换最大权值的孩子节点与父亲节点
        else
            break; // 孩子节点的权值均比父亲节点的小，调整结束
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i)
        scanf("%d", &ori[i]);
    for (int i = 1; i <= n; ++i)
        scanf("%d", &mid[i]);

    int i = 1;
    while (i < n and mid[i] <= mid[i + 1])
        ++i;
    int j = i + 1;
    while (j < n and ori[j] == mid[j])
        ++j;

    if (j == n) {
        printf("Insertion Sort\n");
        sort(mid + 1, mid + i + 2);
    } else {
        printf("Heap Sort\n");
        int j = n;
        while (j > 1 and mid[j] >= mid[1])
            --j;
        swap(mid[1], mid[j]);
        downadjust(1, j - 1);
    }

    for (int i = 1; i <= n; ++i)
        cout << mid[i] << (i == n ? "" : " ");
}
```
