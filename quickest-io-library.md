# Quickest IO Library

For USACO, all code needs to be produced from scratch in contest. This means that if you are going to have a template, it needs to be planned out aptly to minimize the time it takes to write. 

One of the best things about templates is the ability to have an IO library. This means writing `re(a,b,c,d)` instead of `cin >> a >> b >> c >> d` and similar. The problem is, creating such a library from scratch can take a very long time. 

But I recently came upon a novel method that can streamline this very process, called **operator forwarding**. 

The process is extremely simple: you can write the entire library in only four lines of code. 

The first thing you need to know is that functions like `re` and `pr` need to be based on variadic template arguments. We can make a reusable macro like the below:

```cpp
#define m1(x) template<class T, class... U> void x(T&& a, U&&... b) 
```

But how do we write the function itself? This is where we notice that the arguments are in the form of a parameter pack, meaning that we can just forward. In particular, we can make a second macro that hosts our forwarding pack:

```cpp
#define m2(x) (int[]){(x forward<U>(b),0)...}
```

Now, based on just these two macros, we can write an entire IO library with surprising ease:

```cpp
m1(pr) { cout << forward<T>(a);  m2(cout << " " <<); cout << "\n"; } 
m1(re) { cin >> forward<T>(a); m2(cin >>); }
```

Just like that, we have the capacity to write an IO library extremely quickly, in only four lines total!

For more, you might consider writing a debug function by using `cerr` and `endl`. I leave this open to you as a simple exercise. 

