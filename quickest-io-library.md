# Quickest IO Library

For USACO, all code needs to be produced from scratch in contest. This means that if you are going to have a template, it needs to be planned out aptly to minimize the time it takes to write. 

One of the best things about templates is the ability to have an IO library. This means writing `re(a,b,c,d)` instead of `cin >> a >> b >> c >> d` and similar. The problem is, creating such a library from scratch can take a very long time if done traditionally \(blatant recursive calls\), and this also loses some efficiency in runtime. 

But I recently came upon a novel method that can streamline this very process called **operator forwarding**. 

The process is extremely simple: you can write the entire library in only four lines of code and no extra hassle. 

The first thing you need to know is that functions like `re` and `pr` need to be based on variadic template arguments \(i.e., a comma separated chain\). We can make a reusable macro like the below to handle any desired function `x`. The semantics here are that the arguments are given as universal rvalue references for flexibility. 

```cpp
#define m1(x) template<class T, class... U> void x(T&& a, U&&... b) 
```

But how do we write the function bodies? This is where we notice that the arguments are in the form of a parameter pack, meaning that we can use perfect forwarding. This avoids recursion, not only speeding up runtime by avoiding recursive call overhead but even speeding up coding time \(one line instead of many\).  In particular, we can make a second macro that hosts our forwarding pack:

```cpp
#define m2(x) (int[]){(x forward<U>(b),0)...}
```

Now, based on just these two macros, we can write an entire IO library with surprising ease:

```cpp
m1(pr) { cout << forward<T>(a);  m2(cout << " " <<); cout << "\n"; } 
m1(re) { cin >> forward<T>(a); m2(cin >>); }
```

Just like that, we have the capacity to write an IO library extremely quickly, in only four lines total! These lines are not only easy to memorize but also easy to understand. 

For more, you might consider writing a debug function by using `cerr` and `endl`. I leave this open to you as a simple exercise. Once this is done, you can get even more out of it by writing another macro that uses the variadic debugging function and also provides line numbers using `__LINE__` and variable names using `#__VA_ARGS__`. Although this does make two extra lines of code, it is a worthwhile investment.

If this wasn't enough, note that the generic nature of the macros `m1` and `m2` enables us to write simple functions that take comma-separated parameters for anything in C++ that is inconvenient when time is of essence \(overloaded operators\). 

