# "Unusing" Identifiers

A lot of people complain and agonize about small things without realizing that the fix is right in front of them. As a chief example, a couple of weeks ago, I was bent on finding a way to unuse identifiers. 

For background, consider the standard namespace. In competitive programming, frequent practice is 

```cpp
using namespace std; 
```

And this is beautiful! No more need to write std::. Terrible for production of course, but amazing for coding platforms where time is of essence. 

Of course, the reason this is considered bad practice is if you want to use something the std already predefines \(especially an issue if you use bits\). For instance

```cpp
#include <bits/stdc++.h>
using namespace std; 
```

This code makes y1 unusable because it is already a symbol for a mathematical distribution in cmath. Naturally, one would want something like

```cpp
#include <bits/stdc++.h>
using namespace std; 
not using std::y1; 
```

But the scary reality is that this doesn't exist. One might now rewrite usings manually out of sheer frustration, as I was about to a few weeks ago. 

But there is a very clever solution. Whenever we need to go against something in the standard, we can use its most vulgar tools. In this case, we can literally use a find-and-replace solution to escape the y1 problem:

```cpp
#include <bits/stdc++.h>
using namespace std; 
#define y1 _y1
```

Now, every y1 is replaced with \_y1, a symbol that means floof to your compiler! This means that you can safely use y1 \(while preprocessor will use the other version, a semantically safe alternative\). 

Alas, this extends and can be applied to anything one wants to "unuse" from the standard. Some people wish they had the opportunity to use "rank" instead of "rnk" in their functions. Well, this is a tried and tested solve!

