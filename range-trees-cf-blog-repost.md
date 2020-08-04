# Range Trees \(CF Blog Repost\)

I reproduce my first CF blog here. It covers range trees as well as some of their overlooked properties.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

I decided to write my first blog post about a rather nebulous \(or at least overlooked\) data structure class known as range trees. I've seen that their intuition is implicitly used in many solutions but goes unnamed. For many, such intuition cannot even be easily derived until they encounter such editorials.

When I say the phrase "range tree", many people immediately think of segment trees or Fenwick trees \(binary indexed trees\). This is not a misconception -- range trees can be implemented from segment trees and Fenwick trees alike, and some in some countries, segment trees and range trees are frequently interchangeable terms in themselves -- but they are not strictly limited to these two categories.

So, why care about "range trees" as an abstraction if we can continue to ignore them and solve problems just by using segment trees or Fenwick trees? As I said before, the intuition is not necessarily straightforward. But by creating the abstraction of a "range tree", we can A\) better refer to the underlying intuition and B\) make it commonly known. We make these concepts more readily accessible to those just beginning their journey into algorithmic programming.

Since data structures and algorithms are best learned by way of approaching problems directly, I pose the following:

You are given an indexed array 𝐴A of elements. You must answer queries that are either point updates to an element or request the total number of elements whose values are in the range \[𝑙,𝑟\]\[l,r\].

This problem should not be too hard when even analyzed directly under the intentions of segment trees or Fenwick trees, and so if you are familiar with either of these structures, I encourage you to pause and think about this problem before reading any further.

I now assume you've paused and given some thought, so I will resume with the solution. Here, we can construct a segment tree indexed over the array _values_ themselves \(not indices!\). Every time we encounter a new value, we push a point update of +1+1 to that value. Our range queries themselves can be answered now as simple range sums from 𝑙l to 𝑟r, where these "indices" refer to array _values_ inherently. This works because our range sum effectively counts each occurrence with value in \[𝑙,𝑟\]\[l,r\] once and only once.

But what about point updates themselves then? If we want to update an element to a completely different value \(essentially an assignment\), we can think of logically pushing a point update of −1−1 to the segment tree at that value \(just subtracting 11 from that value, since we now want to get rid of it\), and pushing a point update of +1+1 to the new value. But wait! How do we know what the values at the indices are? We must remember that, along with consistently updating our segment tree, we have to update the array values themselves as they change! That way, to figure out what value we want to modify in our segment tree, we can just refer to the appropriate array index to find that value.

We can do all of the above equivalently with a Fenwick tree. In fact, if we crave optimization, we can go the route of a dynamic segment tree \(using pointers\) or a coordinate compression solution. Since this is simple enough given a standard segment tree, I'll leave this implementation up to you.

When we change things just a little, most people begin to invent something too complicated. We now delve into a slightly different type of problem:

You again have the array 𝐴A. Support queries to count the occurrences of elements with ONLY value in range \[𝑙,𝑟\]\[l,r\] in the subarray formed by index range \[𝑎,𝑏\]\[a,b\], along with point updates like before.

This is much stranger. Now, we must count elements not just within a given value range, but we must do this within subarrays! The problem with using the solution from before is that we must now retain exact information about the index each element comes from.

But there is a solution, which takes this idea of maintaining extra information on the values in exchange for an extra log factor.

Imagine a 2D rectangle of points in some coordinate plane. The horizontal axis may indeed be dedicated to the value of a point, while the vertical axis retains its index. This reduces our query from before as just counting the number of points within the bounding rectangle spanning \[𝑙,𝑟\]\[l,r\] horizontally and \[𝑎,𝑏\]\[a,b\] vertically. For this, we can use a 2D tree capable of range processing, such as a Fenwick tree on Fenwick trees or a segment tree on Fenwick trees. Here, our operation in itself can be viewed as a composite query on all Fenwick trees across a range.

Here, implementation is not trivial, so I will link you to \[d-dimensional Fenwick trees\]\(https://github.com/bqi343/USACO/blob/master/Implementations/content/data-structures/1D%20Range%20Queries%20\(9.2\)/BIT%20\(9.2\).h\), where we are exploring 2D in this particular problem. Of course, the query time is log𝑑logd, meaning that our 𝑑=2d=2 case invokes a square log factor.

But wait! If we conclude here, I won't be able to introduce anything new. Let's look into a third type of problem, which I like to call "existence" queries:

This time you are given an entire tree 𝑇T, where nodes are colored different colors. Process multiple queries efficiently, where a query is either of the form of updating the color of a certain node, or querying the existence of a color along some path of the tree.

This problem looks incredibly daunting the first time, but we make some simplifications. First, using one of HLD, centroid decomposition, or [the preorder traversal flattening trick](https://codeforces.com/blog/entry/78564), we reduce this to a problem on ranges. Ranges are much easier to handle. **But be mindful of the fact that this simplification adds an extra log factor.**

Now, imagine that we have a range query from left to right, asking about the existence of a particular color. Naively, this can be broken down into the problem of type 2 -- we can just query for a range like \[𝑙,𝑙\]\[l,l\] if we want to check for the existence of 𝑙l -- where we can easily answer this in square log time. But this is problematic! If 𝑛n is on the order of 105105 \(as it often is\), our overall problem will be a cubic log factor times a linear factor in time complexity, which is cutting it far too close. For context, consider the USACO problem [milkvisits](http://www.usaco.org/index.php?page=viewproblem2&cpid=970), in which we face exactly this.

Others might suggest solving this with a merge sort tree, where we construct a segment tree in which we hold vectors or ordered statistics as nodes and then merge upward. However, this too introduces a square log factor and thus falls to the same problem as before.

Then how exactly can we solve this? There are two very nice approaches.

Offline: If we are to do this offline, we can actually be very clever. Instead of looking for existence queries, we can consider range maximum queries. The trick here is that we can make an array of queries that we must process, queries of type 1 or type 2, and only update the tree up to the point that we have nodes ≤𝑣≤v when we query for range max. Then, if this range max is 𝑣v, it follows that 𝑣v does exist here! Range max is a simple operation, taking only a single log factor, so it is a decent solution.

Online: Now, if we must do this online, we have to be clever. We actually need to create some sort of range tree that allows us to query for existence. And the resolution here is that we can simply use a balanced binary search tree \(BBST\), such as a set in C++! In particular, maintain an array of sets, where an index in this array is a particular value. Then, the set under this particular value's index will just be every single location where this element occurs. We can simply initiate a binary search to answer this!

We can create a data structure such as something I'd call a "range set" for this. Below is my code for a range set that queries for boolean existence:

```text
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp> //hset, hmap
using namespace std; using namespace __gnu_pbds; 

//Range Set Data Structure: query for element existence (bool) in ranges
//Also useful for HLD (see my online milkvisits sol for an example)

template<class T, class U, class chash = hash<T>> using hmap = gp_hash_table<T, U, chash>; 

template<class T> struct rngset { //log n "exist?" queries
    #define DYNAMIC 

    #ifndef DYNAMIC
        vector<set<int>> colors; 
        rngset(T maxel) : colors(maxel + 1) { }
    #else 
        hmap<T, set<int>> colors; 
    #endif

    void upd(int i, T v, bool put = 1) { //put val v at idx i
        if(put) colors[v].insert(i); else colors[v].erase(i);
    }
    bool qry(int i, int j, T v) { //check if v in [i,j]
        bool res = 0; 
        auto it = colors[v].lower_bound(i);
        if(it != end(colors[v]) && *it <= j) res = 1; //we can modify this if we want to actually count occurrences 
        return res; 
    }
};

#ifndef DYNAMIC 
    rngset<int> s(100); //construct w/ max element
#else
    rngset<int> s; 
#endif

int main() {
    s.upd(0,10); 
    s.upd(1,20);
    assert(s.qry(0,1,10) && s.qry(0,1,20) && !s.qry(0,1,15));
    s.upd(0,10,0);
    assert(!s.qry(0,1,10));
}
```

Motivated by benq's note in the USACO editorial to solve milkvisits online, we can even follow up on our HLD approach, which works completely online.

I hope this was helpful as a first blog to help you better conceptualize range trees!

