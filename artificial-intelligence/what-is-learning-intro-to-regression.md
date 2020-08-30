# What is Learning? Intro to Regression

![](../.gitbook/assets/image%20%282%29.png)

If we want a program to learn to do something on its own, we need to treat it like a baby. We need to show it an example. This is a very general idea of what we strive to do with AI. 

Up until now, unless you have experience in artificial intelligence, you have always just told a computer what to do. For instance, maybe you wanted it to double a number and add 7. For this, you just gave it the general logic:

```cpp
int someFunction(int x) { 
    return 2 * x + 7;  
}
```

And the computer understood this! The computer just had to use the function you already gave it and feed that function various inputs! 

But what if we wanted the computer to _find the function_ based on examples we gave it?

![What is AI? Think of it as finding a function to map from X \(input\) to Y \(output\).](../.gitbook/assets/image%20%285%29.png)

Maybe this time we feed the program examples of what we want it to do:

| Input x | Output f\(x\) |
| :--- | :--- |
| 0 | 7 |
| 1 | 9 |
| 2 | 11 |
| 3 | 13 |
| 4 | 15 |
| 5 | 17 |

This seems promising, right? We have a ton of data!

But _how_ do we make the computer understand the function from the data so that it can use the function it derives to then interpret new data? That is the pressing question, and leads us into the idea of **regression**, finding a functional correlation \(a perfect one-to-one mapping\) between an input space and an output space. 

The example of regression here is very simple and can be done with a bit of basic algebra, suitable for beginners. It is the first AI model we cover because it takes only algebra to understand how it works. 

If you have mathematical experience, you know a few things about functions. First, we narrow the broad category of "functions" into just polynomial functions for now, since those are the easiest to grasp. Now, the following fact is true:

A polynomial function of degree n is perfectly defined by n + 1 points. 

You might wonder why this is the case. To prove it, consider a general polynomial function P\(x\) of degree n. Now, P\(x\) can be written in some factored form as a\(x - r\_1\) \(x - r\_2\)\(x - r\_3\)...\(x - r\_n\), where r\_1 through r\_n are all roots of P\(x\). As such, we know P\(x\) is perfectly defined when we have enough information to determine a as well as all the roots r\_1 through r\_n

Now, suppose we are given a single point of data, one \(x, y\) pair, that lies on the target function P\(x\). This gives us exactly one unique equation of the form y = a\(x - r\_1\) \(x - r\_2\)\(x - r\_3\)...\(x - r\_n\). Then, if we get another \(x, y\) pair data point, we get another unique equation in the same form. In general, if we have K points, we get a _system_ of K unique equations. An important idea we can apply is that a system of equations with N independent variables is perfectly determined once there are N unique equations, so for the K points to perfectly determine all variables, a and r\_1 through r\_n, there must be K variables. In other words, K = n + 1, since there are n + 1 independent variables involved in our P\(x\) equation. Hence, it has been proven that a function of degree n is perfectly defined by n + 1 points.  

We can now apply this fact to our original problem. We know we want the computer to understand a linear function from the data, meaning that we need only 1 \(the degree of a linear function\) + 1 = 2 data points. From these two data points, we can perfectly derive the equation of the line in the program. 

This means that instead of including the ton of data provided before, we can simply use two data points. Suppose we use the ones below:

| Input x | Output f\(x\) |
| :--- | :--- |
| 0 | 7 |
| 1 | 9 |

How exactly do we derive the equation of the line itself? For this, the program just needs to solve the system of equations corresponding to the general linear function y = mx + b to get the values of m and b corresponding to the example. Then, it knows the function perfectly. 

Instead of coding this \(it isn't too nice to code a system of equations solver, and indeed, for polynomial functions with high degree, we get a very large number of systems of equations and resort to **Cramer's rule** to efficiently solve the system\) for now, I will show you "AI by hand" and solve the regression myself. 

![Using the given training data to &quot;fit&quot; a linear regression model \(a linear function\)](../.gitbook/assets/image%20%283%29.png)

This linear function is now ready to predict outputs for inputs it has never seen! In other words, the computer has successfully _learned_ from the data via a simple linear regression model. 

This, as a first post, outlines the general idea of AI and what it means for a computer program to "learn" from given data and actually find the underlying functional relation. 

