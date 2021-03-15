# ÌâÄ¿ÏêÇé
![wbfg.JPG](https://images.ptausercontent.com/e8219d7d-c640-4b82-a98d-4553a1817364.JPG)

The above picture is from Sina Weibo, showing May 23rd, 2019 as a very cool "Prime Day". That is, not only that the corresponding number of the date `20190523` is a prime, but all its sub-strings ended at the last digit `3` are prime numbers.

Now your job is to tell if a given date is a Prime Day.

### Input Specification:

Each input file contains one test case. For each case, a date between January 1st, 0001 and December 31st, 9999 is given, in the format `yyyymmdd`.

### Output Specification:

For each given date, output in the decreasing order of the length of the substrings, each occupies a line. In each line, print the string first, followed by a space, then `Yes` if it is a prime number, or `No` if not. If this date is a Prime Day, print in the last line `All Prime!`.

### Sample Input 1:

```
20190523
```

### Sample Output 1:

```
20190523 Yes
0190523 Yes
190523 Yes
90523 Yes
0523 Yes
523 Yes
23 Yes
3 Yes
All Prime!
```

### Sample Input 2:

```
20191231
```

### Sample Output 2:

```
20191231 Yes
0191231 Yes
191231 Yes
91231 No
1231 Yes
231 No
31 Yes
1 No
```