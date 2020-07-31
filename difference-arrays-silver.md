# Difference Arrays \(Silver\)

You have probably heard of a prefix sum, and it is indeed a very useful technique to find range sums as elements remain static. But difference arrays are less common and just as useful, making them great candidates for USACO problems and thus essential to master for the silver division. 

Formally, for an array `A`, take `P(A)` to be its prefix sum array. This is simple code to write \(note the splendid use of templates\):

```cpp
template<class T> T P(T A) { 
    T psum = A; 
    for(int i = 1; i < A.size(); ++i) psum[i] += psum[i - 1]; 
    return psum;  
} 
```

Now, a difference array is simply defined as the functional _inverse_ of a prefix sum array. Concretely, `A` is the difference array of `P(A)`, or if `D(A)` is the difference array of `A`, then `D(P(A)) = P(D(A)) = A`.  Difference arrays are also fairly simple to write:

```cpp
template<class T> T D(T A) {
    T diff = A; 
    for(int i = A.size() - 1; i > 0; --i) diff[i] -= diff[i - 1]; 
    diff[0] = 0; 
    return diff;
}
```

 But what how do these apply to problems? There is a key observation, often referred to as _the difference array trick._

Adding a quantity `d` to some contiguous subarray of an array `A` whose indices are expressed as the interval `[l, r]` is just the same as adding `d` to `D(A)[l]`, subtracting `d` from `D(A)[r + 1]`, and finally taking `P(D(A))` to get the desired array `A`. In other words, we first construct a difference array and then prefix sum it to get the intended array. This allows us to do operations that would be `O(l - r + 1)` in the intended array world as `O(1)` operations in the difference array world. 

