People often have a preference among synonyms of the same word. For example, some may prefer "the police", while others may prefer "the cops". Analyzing such patterns can help to narrow down a speaker's identity, which is useful when validating, for example, whether it's still the same person behind an online avatar.

Now given a paragraph of text sampled from someone's speech, can you find the person's most commonly used word?
### Input Specification:
Each input file contains one test case. For each case, there is one line of text no more than 1048576 characters in length, terminated by a carriage return `\n`. The input contains at least one alphanumerical character, i.e., one character from the set [`0-9 A-Z a-z`].
### Output Specification:
For each test case, print in one line the most commonly occurring word in the input text, followed by a space and the number of times it has occurred in the input. If there are more than one such words, print the lexicographically smallest one. The word should be printed in all lower case. Here a "word" is defined as a continuous sequence of alphanumerical characters separated by non-alphanumerical characters or the line beginning/end.

Note that words are case **insensitive**.
### Sample Input:
```
Can1: "Can a can can a can?  It can!"
```
### Sample Output:
```
can 5
```
# 题解

给出一行字符。输出出现字数最多的单词和次数，全部以小写字母输出。如果有多个答案，输出字典序最小的。这里单词的定义是全部以数字或字母组成的字符串。



使用map统计单词的次数，而且map能保证字典序小的先出现。
```cpp
#include <cctype>
#include <iostream>
#include <map>
#include <string>

using namespace std;

int main() {
    string s, t;
    map<string, int> mp;

    getline(cin, s);

    for (int i = 0; i < s.length(); ++i) {
        if (isalnum(s[i])) {
            s[i] = tolower(s[i]);
            t += s[i];
        }
        if (!isalnum(s[i]) or i == s.length() - 1) {
            if (t != "")
                ++mp[t];
            t = "";
        }
    }

    int max_cnt = 0;
    for (auto &e : mp) {
        if (e.second > max_cnt) {
            s = e.first;
            max_cnt = e.second;
        }
    }

    cout << s << " " << max_cnt;
}
```