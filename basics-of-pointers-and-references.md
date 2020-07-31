# Basics of Pointers and References \(Advanced Bronze\)

When people hear about pointers, they frequently dismiss them as a part of the language that is unnecessary to know. This is an alternative fact, and I will prove this to you. 

Imagine wanting to sort an array, a task not uncommon for informatics. We could write our own merge sort algorithm, but why not use what the STL has to offer?

If we want to now sort a `vector<int> v`, we can write `sort(begin(v), end(v))`, but unless you are aware of pointers, you will not be able to understand what is going on here. Here, I provide very limited exposition on pointers, all that is relevant to informatics. 

You store variables in program memory, which you can imagine as an arbitrary grid of cells \(it is really a lot more complex, with bits and other things, but take for now this simplified interpretation\). Each cell stores a variable or a "value" that is a fundamental data type. This is obviously oversimplified, but it is enough. 

Now, arrays, vectors, and strings can all be contiguous memory slots in this memory grid. But how exactly can this work? We can place "pointers" on the beginning and ending positions of the structures on the memory grid. As such, **pointers** are merely addresses in memory. 

When we declare a data type `d`, it implicitly gets its own address `&d` in the memory, which is literally a pointer. If we want to now create a reference `p` to this pointer, we can do this as

```cpp
auto p = &d; 
```

Or, if `d` is really an integer and we wish to be more specific, it would look like:

```cpp
int * p = &d 
```

Where the \* means pointer, and the data type before it tells us what it is a pointer of. Now, we can dereference a pointer, or find what it is pointing to, via

```cpp
int val = *p; 
```

In other words, it is true that `d` is simply `*(&d)`, or what is pointed to by its own pointer.

