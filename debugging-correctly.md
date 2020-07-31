# Debugging Correctly \(Bronze\)

Debugging is one of the most important skills in programming \(not limited to the informatics scene\)! A good programmer can debug efficiently and produce efficient, bug-free code. Since debugging is often overlooked, this is your guide for how to debug _correctly_ \(yes, there are frequent pitfalls\). 

## Basic Print Statements

The most basic way that you might debug is adding a print statement. This is great and serves the purpose for the most part. For instance, we can write the below to check the value of `x` at a point in our code. 

```cpp
#include <iostream>
using namespace std; 

int x = 10; // some important variable

inline void dbg() { cout << "x is " << x << "\n"; }

int main() {
    dbg(); 
    x = 5000; 
    dbg();  
}
```

Such print statements are great on a basic level, and we can comment or define them out of our main code when we need to compile and execute a more final version of our code. 

However, as great as print statements are, they are annoying to work with and efficiently separate from the actual parts of our code. This is important for example when we want an online judge \(OJ\) to read our output. 

## A Step Further: Standard Error Stream

The standard error stream \(`cerr` in C++\) is a quick fix to this. Instead of printing in standard iostream, we actually generate a whole new stream of data called the error stream. Simply replace all instances of `cout` with `cerr`. For example:

```cpp
inline void dbg() { cerr << "x is " << x << "\n"; }

int main() {
    dbg(); 
    x = 5000; 
    dbg(); 
}
```

Try running this program and you might be confused about the difference. You will be able to see the output of cerr right next to regular cout outputs. But this is the beauty of it! And if we use freopen to open up file pipes or submit this to an OJ, the program will not include the error stream. 

## Address Sanitizer

Of course, debugging by print statements is great. But what about runtime errors that mess with the flow of the program itself? For this, we have a clever tool known as the address sanitizer. It can be invoked as through the below flags.  

```text
-ggdb -fsanitize=address,undefined 
```

The first flag generates a debug report \(in dSYM file format\) based on the line numbering of the program, while the second flag can then access the dSYM file at runtime and give meaningful errors. This is great because it helps diagnose \(or "sanitize" if you will\) errors that prevent the run flow of the program, such as out of bounds, exceptions, and segmentation faults, even indicating precise line numbers. This is definitely a tool you should add to your arsenal. 

Feel free to delete dSYM files after the run of course. 

## Possible Pitfall: Simply Compiling Code Mentally

Many beginners are convinced that they can compile the code line by line in their head to notice the error. This is actually a very advanced technique of debugging in that it takes great skill to master! Sure, a beginner may catch a missing semicolon, but most beginners will have their judgment clouded by their perceptions in the language, which may be behind the very problem. 

Even those who are experienced may waste time trying to do this, even if they have higher chances of locating the problem as such. 

Concisely, resort to mental compilation when you are more experienced and are not left with other options. Otherwise, print statements are enough to get around wrong answers \(incorrect results\), with address sanitizers around faulty results!

## Possible Pitfall: Rewriting the Program

Rewriting the program is sometimes a good idea, but remember to keep track of time! It is very easy to get carried away rewriting even parts of a program that are bug-free in obsessive hope that something might come through. 

Do NOT delete your previous program. Make a new file! 

If the problem is with the program's implementation of your logic and not your logic itself, it is usually better to fix the program directly. 

Again, **always make extra copies!!**

## Conclusion

You are now well-equipped with the most efficient methods of debugging around. Use them well!

