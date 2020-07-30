# Beat USACO Bronze: A Guide

Unlike other divisions of USACO, bronze is nebulous in its claim of testing no algorithms at all but rather a general basic command of the contestant's programming ability. Since bronze is your first USACO division, you should go in confident. But that raises the essential question: what builds the bronze skillset. I will investigate just that in this guide. 

**Syntax:**

Before you can even begin any sort of programming, you have to know how to program. First, pick a programming language of interest. As I've explained countless times before, C++ is the best language for this, although Java is definitely a close second. Opinions vary, but C++ is most popular. 

Now that you have chosen a language, you need to learn how to program in the language. For C++, you will want one or more of the following three types of general resources.

1\) A textbook/online forum: typically very comprehensive and self-paced as an option, but requires self-reliance; two categories

* Effective Modern C++, Programming Principles, C++ Primer \(basic, more general -- better for basic bronze\)
* CSES, CP3, USACO Guide \(intermediate or hard, more contest-oriented -- better for intermediate bronze and beyond\)

2\) Video lectures: typically quick and self-paced, but not always most comprehensive; two categories

* YouTube series like [Bucky](https://www.youtube.com/watch?v=tvC1WCdV1XU&list=PLAE85DE8440AA6B83), covering fundamentals \(better for basic to intermediate bronze\)
* YouTube series like [Algorithms Live](https://www.youtube.com/channel/UCBLr7ISa_YDy5qeATupf26w) and [AlgorithmsThread](https://www.youtube.com/watch?v=cVBzMXYc4ss&list=PLZU0kmvryb_HZpDW2yfn-H-RxAu_ts6xq), covering algorithmic thinking \(better for advanced bronze and beyond\)

3\) A personal teacher or learning platforms: comprehensive, with the plus point of being able to ask questions, but sometimes expensive

Fortunately, 1\) and 2\) are very easy to acquire given the willpower. 

You will want to get familiar with the STL and all of the various ways to store and manipulate data. Take notes on how you can potentially use what you learn. It is helpful to write member function names and class names for types you think you will use most often. Get familiar for speed and comfort while coding. 

**Problem Solving:**

Now that you have syntax down, you need to solve problems -- as many as you can find at your level. The more the better. There are patterns you should look for in bronze problems to build intuition. Here are some.

Read/Write Files Effectively: Look at freopen \(very important\) and IO optimizations with cin/cout \(less important now, more later\)

Storing Data: You will be given data in bulk relevant to your problem. Instead of creating a separate variable each time, make an array \(a static, contiguous chunk of memory that holds lots of variables of the same type together, indexed\), or a vector \(a dynamic but contiguous and index chunk\). Store data globally to access it not only main but also helper functions without passing it in relentlessly. There are some exceptions, but bronze does not test them at all. 

Iterative Search: Most often, the solution to bronze is try all possible cases. But you need to be comfortable with _how_ you do this in C++. For loops and while loops are useful in that you can iteratively scan data. The most common way is by index \(or even pointer\) but you can do this as a scan by element in range-based fors. 

Recursive Search: Sometimes, an iterative scan is not enough. To do better, we can actually recurse \(essentially, continuously call subprocesses\). 



