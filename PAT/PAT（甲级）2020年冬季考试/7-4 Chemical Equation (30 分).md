# 题目详情

A **chemical equation** is the symbolic representation of a chemical reaction in the form of symbols and formulae, wherein the reactant entities are given on the left-hand side and the product entities on the right-hand side. For example, $CH_4 + 2 O_2 = CO_2 + 2 H_2 O$ means that the reactants in this chemical reaction are methane and oxygen: $CH_4$ and $O_2$, and the products of this reaction are carbon dioxide and water: $CO_2$ and $H_2 O$.

Given a set of reactants and products, you are supposed to tell that in which way we can obtain these products, provided that each reactant can be used only **once**. For the sake of simplicity, we will consider all the entities on the right-hand side of the equation as one single product.

### Input Specification:

Each input file contains one test case. For each case, the first line gives an integer $N (2 \le N \le 20)$, followed by $N$ distinct indices of reactants. The second line gives an integer $M (1 \le M \le 10)$, followed by $M$ distinct indices of products. ==The index of an entity is a 2-digit number==.

Then a positive integer $K (\le 50)$ is given, followed by $K$ lines of equations, in the format:

```
reactant_1 + reactant_2 + ... + reactant_n -> product
```

where all the reactants are distinct and are in increasing order of their indices.

Note: It is guaranteed that

-   one set of reactants will not produce two or more different products, i.e. situation like `01 + 02 -> 03` and `01 + 02 -> 04` is impossible;
-   a reactant cannot be its product unless it is the only one on the left-hand side, i.e. `01 -> 01` is always true (no matter the equation is given or not), but `01 + 02 -> 01` is impossible; and
-   there are never more than 5 different ways of obtaining a product given in the equations list.

### Output Specification:

For each case, print the equations that use the given reactants to obtain all the given products. Note that each reactant can be used only **once**.

Each equation occupies a line, in the same format as we see in the inputs. The equations must be print in the same order as the products given in the input. For each product in order, if the solution is not unique, always print the one with the smallest sequence of reactants -- A sequence ${ a_1, \cdots , a_m }$ is said to be smaller than another sequence ${ b_1, \cdots , b_n }$ if there exists $1\le i\le min(m,n)$ so that $a_j=b_j$ for all $j<i$, and $a_i<b_i$.

It is guaranteed that at least one solution exists.

### Sample Input:

```
8 09 05 03 04 02 01 16 10
3 08 03 04
6
03 + 09 -> 08
02 + 08 -> 04
02 + 04 -> 03
01 + 05 -> 03
01 + 09 + 16 -> 03
02 + 03 + 05 -> 08
```

### Sample Output:

```
02 + 03 + 05 -> 08
01 + 09 + 16 -> 03
04 -> 04
```