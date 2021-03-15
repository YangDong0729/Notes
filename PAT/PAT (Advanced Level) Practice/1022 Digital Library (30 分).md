https://pintia.cn/problem-sets/994805342720868352/problems/994805346063728640

> 千名教师建设，万道高质量题目，百万用户拼题的程序设计实验辅助教学平台

# 题目详情
A Digital Library contains millions of ==books, stored according to their titles, authors, key words of their abstracts, publishers, and published years. Each book is assigned an unique 7-digit number as its ID. Given any query from a reader, you are supposed to output the resulting books, sorted in increasing order of their ID's.==

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer $N$ ($\le 10^4$) which is the total number of books. Then $N$ blocks follow, each contains the information of a book in 6 lines:

*   Line #1: the 7-digit ID number;
*   Line #2: the book title -- a string of no more than 80 characters;
*   Line #3: the author -- a string of no more than 80 characters;
*   Line #4: the key words -- each word is a string of no more than 10 characters without any white space, and the keywords are separated by exactly one space;
*   Line #5: the publisher -- a string of no more than 80 characters;
*   Line #6: the published year -- a 4-digit number which is in the range \[1000, 3000\].

It is assumed that each book belongs to one author only, and contains no more than 5 key words; there are no more than 1000 distinct key words in total; and there are no more than 1000 distinct publishers.

After the book information, there is a line containing ==a positive integer $M$ ($≤1000$) which is the number of user's search queries.== Then $M$ lines follow, each in one of the formats shown below:

*   1: a book title
*   2: name of an author
*   3: a key word
*   4: name of a publisher
*   5: a 4-digit number representing the year

### Output Specification:

For each query, first print the original query in a line, then output the resulting book ID's in increasing order, each occupying a line. If no book is found, print `Not Found` instead.

### Sample Input:

    3
    1111111
    The Testing Book
    Yue Chen
    test code debug sort keywords
    ZUCS Print
    2011
    3333333
    Another Testing Book
    Yue Chen
    test code sort keywords
    ZUCS Print2
    2012
    2222222
    The Testing Book
    CYLL
    keywords debug book
    ZUCS Print2
    2011
    6
    1: The Testing Book
    2: Yue Chen
    3: keywords
    4: ZUCS Print
    5: 2011
    3: blablabla


### Sample Output:

    1: The Testing Book
    1111111
    2222222
    2: Yue Chen
    1111111
    3333333
    3: keywords
    1111111
    2222222
    3333333
    4: ZUCS Print
    1111111
    5: 2011
    1111111
    2222222
    3: blablabla
    Not Found
# 题解

给出N本书的编号、书名、作者、关键词（可能有多个）、出版社及出版年份，并给出M个查询，每个查询给出书名、作者、关键词（单个）、出版社及出版年份中的一个，要求输出满足该给出信息的所有书的编号。



在输入时就把这些信息与id建立映射。注意在使用 `getline` 之前要处理掉多余的回车（ `cin.get()` ）。

```cpp
#include <algorithm>
#include <iostream>
#include <sstream>
#include <string>
#include <unordered_map>
#include <vector>

using namespace std;

int main() {

    int n;
    cin >> n;
    cin.get();

    unordered_map<int, unordered_map<string, vector<string>>> maps;

    while (n--) {
        string id, item;
        getline(cin, id);

        for (int i = 1; i <= 5; ++i) {
            getline(cin, item);

            if (i == 3) {  // 处理关键字
                stringstream ssm;
                ssm << item;
                while (ssm >> item) maps[i][item].push_back(id);
            }
            else {
                maps[i][item].push_back(id);
            }
        }
    }

    cin >> n;
    cin.get();

    while (n--) {
        string query;
        getline(cin, query);

        cout << query << '\n';
        if (maps[query[0] - '0'].count(query.substr(3)) == 0)
            cout << "Not Found\n";
        else {
            auto& ids = maps[query[0] - '0'].at(query.substr(3));
            sort(ids.begin(), ids.end());  // 在查询时再排序
            for (auto& e : ids) cout << e << '\n';
        }
    }
}
```

