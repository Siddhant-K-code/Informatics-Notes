# USACO Division Ladders



These will extend from bronze to gold, since platinum problems are especially difficult to come up with. These will not necessarily be high quality or original, but they will be worth your time and great for intuition building. 

BRONZE: 

The spirit of bronze is to essentially conduct an exhaustive search of all possible solutions. In some cases, this search will be rather direct, but in other cases, some ingenuity may be required. 

1. You are given up to 15 positive integers in sorted order. Between any two adjacent integers in the ordering is a gap \(for a total of 14 gaps\). In each gap you may place a + sign or - sign, thereby producing an arithmetic expression that will be evaluated. Now, you are given another positive integer X. How many positive integers not exceeding X is it impossible for this arithmetic expression to ever yield? \(EASY\)
2. You are given a string S, not exceeding 8 in length. The _index_ of S is its position among the set of all its permutations written in lexicographically ascending order, where the lexicographically smallest permutation of S has index 1. Find the index of S. \(MEDIUM\) 
3. You are given an array of size 1e3. At any step, you can choose two numbers in this array and completely reverse the contiguous subarray of all elements between and including these chosen numbers. Determine the minimum number of such steps to perfectly sort the original array in lexicographically ascending order. \(HARD\)

SILVER:

The spirit of silver is to move toward more advanced searching techniques and delve into introductory graph theory, further emphasizing the ad-hoc skills developed in bronze. 

1. You are painting a one-dimensional number line by covering, at each step, a certain interval in a coat of paint. You will process up to 5e6 of these steps. Next, you will be given 5e6 queries asking about the number of coats of paint covering a specific point. Answer these queries efficiently. \(MEDIUM\)
2. Now, you are painting two dimensional rectangles of tiles on a roof. Again, you must process 5e6 queries of painting a certain rectangle on the roof, given its top right and bottom left points. Next, you will be given 5e6 queries asking about the number of coats of paint covering a specific tile. Answer these queries efficiently. \(HARD\)

GOLD:

The spirit of gold is to reach a level of algorithmic maturity, retaining not only the already covered advanced search techniques and basic graph theory but also extending into the domain of more advanced graph theory, querying data structures, and dynamic programming. 

1. You are given integers X and Y. At each step, you can half X and round down, double X, or increase X by 2. Find the minimum number of steps required to transform X into Y. \[As an extension, how would your procedure change if these steps were penalized differently and you wanted to reach Y with least penalty? For instance, the first step might incur penalty 1, the second 2, and the third 3.\] \(EASY\)
2. You are given an array A. Find the length of the longest contiguous subarray of A that will also be a subarray of its ascending sort \(in that order\). \(MEDIUM\) 
3. You are given strings S and T. At each step, you can either add, delete, or edit a character in S. Find the minimum number of such steps required to completely transform S into T. \(MEDIUM\) 
4. You are given an adjacency list description of a completely connected undirected weighted graph of nodes. At each step, you must either delete all edges with weight not exceeding some given threshold X and report the number of remaining connected components that comprise the graph in this situation. Answer 1e5 such queries efficiently. \(MEDIUM\) 

