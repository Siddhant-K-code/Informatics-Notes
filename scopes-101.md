# Scopes 101

Scopes are one of the biggest factors in programming under the hood and can make or break a program. In fact, bad scope control means producing a program that can take hours to debug. For these reasons, they are important to cover.

First, what is a scope? Formally, a **scope** within a program demarcates the actionable context of an identifier, or in simpler terms, keeps track of where and how long something can be manipulated and referenced in a program. 

For a simple example, consider the below:

```cpp
#include <iostream>

int x; // define identifier "x" in the global scope

int main() { 

}
```

Here, `x` is now an identifier of type `int` \(simply, a variable\) in the program. But in order to see where and when exactly it makes sense to use `x` to refer to this variable, we must consider scope. In this example, `x` has been declared in the **global scope** \(and is **global**\), the place in the program before the beginning of any functions. 

When a variable is declared globally as such, it is **implicitly initialized**, meaning that it can be safely evaluated to whatever the corresponding default initialization of the type is. This is a _very important_ property to remember!

To better see this, we can try printing `x`:

```cpp
#include <iostream>

int x; 

int main() {
    std::cout << x << "\n"; 
    // this gives 0, the default initialization of integer types!
}
```

The wonderful thing about global variables is that they can be used anywhere, including functions other than `main` and still make perfect sense in the context! 

```cpp
#include <iostream>

int x; 

void plus_n(int n) { 
    return x + n; 
    // notice we used n instead of x; avoid variable masking!
}

int main() {
   std::cout << plus_n(10) << "\n"; 
   // this gives 0 + 10 = 10
}
```

All this demonstrates that the _global scope is the safest scope_. You can expect to get predictable results and program seamlessly. Therefore, you should always prefer declaring variables in the global scope!

\(In fact, the global scope also allows for declaration of identifiers with much more lenient memory limits and far more efficient performance. This along with default initialization begs you to declare arrays and other containers globally when possible.\)

But enough about the global scope. When using **local scopes**, very strict local demarcations separated clearly by pairs of curly braces, scopes get quite a lot more messy \(in addition to a lot more strict on memory, meaning more chance of segmentation faults\). 

```cpp
#include <iostream>

int main() {
    int x; 
    std::cout << x << "\n"; 
    // this gives a random value, but why?
}
```

As above, variables declared locally are **default uninitialized** and must be **explicitly initialized**, lest one attain "random" values \(technically whatever is in the memory at that location\). _Variables left uninitialized in local scopes catalyze undefined behavior according to the standard and are best avoided._ 

To avoid this, we could use either the global scope like before or explicitly initialize `x`:

```cpp
#include <iostream>

int main() {
    int x = 0;
    std::cout << x << "\n"; 
    // this gives 0 upon explicit initialization
}
```

Of course, functions are not the only examples of global scopes; we can also create our own scopes within curly braces as long as these are within functions:

```cpp
#include <iostream>

int main() {
    { // you can make your own local scope within a function
        int x = 10, y = 20; 
    }
    // int x = 0; 
    std::cout << x << "\n";
    // this gives an error because x does not exist in this scope's context
}

```

```cpp
#include <iostream>

int main() {
    { // you can make your own local scope within a function
        int x = 10, y = 20; 
    }
    int x = 0; 
    std::cout << x << "\n";
    // this works now that x is defined in the context
}

```

