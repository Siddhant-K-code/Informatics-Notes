# Making a Contest Template

Some contests allow you to use prewritten code, but most other contests -- such as USACO -- require you to write out all your code during the contest. **This either means no templating or templating very effectively so that you produce something you can easily write from scratch in contest.** 

First, we have the bare minimum design below:

```cpp
#include <bits/stdc++.h>
using namespace std; 

#define IO(NAME) \
    cin.tie(0)->sync_with_stdio(0); \
    if(fopen(NAME ".in","r")) freopen(NAME ".in","r",stdin), \
    freopen(NAME ".out","w",stdout); 

int main() {
    IO(""); // put the name here
    // put code here
}
```

Just this much allows you to use fast IO, read and write from a file if possible, and use anything from the standard library, meaning that it should be considered a bare minimum for your code. 

One thing we can immediately add is the GNU policy data structures for hashing and ordered sets, since this allows us to use absolutely all of the resources provided to us under the g++ compiler and these DS are frequently helpful gold and above \(there is also `ext/rope` with the namespace `__gnu_cxx`, but it is almost never useful and thus not included\):

```cpp
#include <bits/stdc++.h>
using namespace std; 

#define IO(NAME) \
    cin.tie(0)->sync_with_stdio(0); \
    if(fopen(NAME ".in","r")) freopen(NAME ".in","r",stdin), \
    freopen(NAME ".out","w",stdout); 
    
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;
template<class T, class U = null_type, class chash = hash<T>> using hset = 
gp_hash_table<T,U,chash>;
template<class T, class U = null_type, class cmp = less<T>> using oset = 
tree<T,U,cmp,rb_tree_tag,tree_order_statistics_node_update>;

int main() {
    IO(""); // put the name here
    // put code here
}
```

At this point, we have constructed a good basic template to use. If you want to add more utilities such as macros and so on, make sure you produce a template you can memorize to provide stressing near contest day. Find a balance between what you can memorize and include and what you should simply leave out. Here are some examples:

[Ben's Original Template](https://github.com/bqi343/USACO/blob/master/Implementations/content/contest/templateLong.cpp) \(too big for USACO but has helpful features you might consider including\)

[Ben's Modified Template](https://github.com/bqi343/USACO/blob/master/Implementations/content/contest/template.cpp)

[Mikey's Abridged Version](https://github.com/caoash/competitive-programming/blob/master/template.cpp)

[My Big Template](https://github.com/Aryansh-S/USACO/blob/master/BigTemplate.cpp) \(too big for USACO but has helpful features you might consider including\)

[My Short CF Template](https://github.com/Aryansh-S/USACO/blob/master/CFformat.cpp)

[My Very Short Template](https://github.com/Aryansh-S/USACO/blob/master/SmallestTemplate.cpp)

