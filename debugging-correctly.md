# Debugging Correctly

Debugging is one of the most important skills in programming \(not limited to the informatics scene\)! A good programmer can debug efficiently and produce efficient, bug-free code. Since debugging is often overlooked, this is your guide for how to debug _correctly_ \(yes, there are frequent pitfalls\). 

## Basic Print Statements

The most basic way that you might debug is adding a print statement. This is great and serves the purpose for the most part. For instance, we can write the below to check the value of `x` at a point in our code. 

```cpp
#include <iostream>
using namespace std; 

int x = 10; //some important variable

inline void dbg() { cout << "x is " << x << "\n"; }

int main() {
    dbg(); 
    x = 5000; 
    dbg();  
}
```

Such print statements are great on a basic level, and we can comment or define them out of our main code when we need to compile and execute a more final version of our code. 

However, as great as print statements are, they are annoying to work with and efficiently separate from the actual parts of our code. This is important for example when we want an online judge to read our output. 

## A Step Further: Standard Error Stream

The standard error stream \(`cerr` in C++\) is a quick fix to this. Instead of

