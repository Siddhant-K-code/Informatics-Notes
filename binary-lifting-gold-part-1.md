# Binary Lifting \(Gold\) Part 1

If you haven't read the intro yet, now would be a good time to do so. While you technically don't _need_ binary lifting for gold, it is a situation similar to how you also don't need segment tree for gold: it can make your life easier if you use it, but you don't have to. 

This post, although essentially self-contained and well-developed, is extremely dense. To maximize understanding, read each chunk carefully. 

Suppose we start simple and review what a tree is first. 

A **tree** is any graph with `n` nodes and `n-1` edges \(where each node is connected to at least one other node via an edge\). For now, we will assume the edges of the tree are all undirected \(meaning that they can be traversed in both ways\). It is also straightforward to inductively show the following properties:

* A tree is acyclic \(does not contain cycles\)
* There is exactly one unique way to get from any node of a tree to any other node of a tree via some sequence of edges with no repeats
  * Logically, based on this, a tree only has one connected component \(itself, where all nodes of the tree are interconnected in some way\)

So how do we traverse a tree? As you know by now, we can always initiate either depth-first searches \(DFS\) or breadth-first searches \(BFS\) from a node to then visit all unvisited nodes. 

In a normal graph with no restrictions, of course, we would actually need to "floodfill" in order to guarantee we visit every node, since DFS or BFS from an arbitrary node would never reach unconnected nodes \(the case where there are multiple connected components\). But the wonderful thing about a tree is that a _single_ DFS or BFS from any node in the tree is guaranteed to reach all nodes in the tree \(since no nodes are unconnected\)!

We use the word **root** to describe the node of a tree where we start our traversals \(we can think of this like an "origin" point, a node with respect to which we can describe any other node\). You will sometimes hear the phrase "root the tree at" some node, and this just means that we effectively designate a single node of a tree as a root node, since this makes it much easier to visualize the underlying structure of the tree. 

Once we root the tree, we can make even more observations about the tree structure. We can note that any node in the tree now has a **depth**, or distance for the root of the tree \(where the root has depth 0\). Relatedly, the **height** of a tree is the lowest depth in the overall tree. 

Furthermore, we can then say that for any node `u` at depth `d`, if there is a node `v` at depth `d-1`, and `v` is adjacent to `u`, then `v` is the **parent** node of `u`, while `u` is a **child** node of `v`. It is straightforward to also show inductively that every node has a unique parent node but can have multiple child nodes. A node with no children is called a **leaf** node \(the nodes at the very bottom of a tree\), and the root node is uniquely the only node with no parent \(although in some cases it is convenient to designate the root node as its own parent and a leaf node as its own child\). 

Based on these nomenclatures, we also have a concept of going up a tree and going down a tree. In particular, we can start at a node and move up a distance `1`. This takes us to its \(unique\) parent. If we moved up distance `2` however, we would end up going to the parent of its parent \(or its second ancestor, also unique\). Similarly, going down a distance of `1`, we would reach some \(not necessarily unique\) child node, and going down a distance of `2` would take us to a child node of a child node \(also not necessarily unique\). 

In general, the \(unique\) `k`th **ancestor** of a node is the node obtained when we travel up from that node a distance of `k` edges \(in a way that is guaranteed to be unique\), and similarly a \(not "the," since this is not unique\) `k`th **descendant** of a node is a node obtained when we travel down from that node a distance of `k` edges \(in some not necessarily unique way\).

Finally, you might imagine a "worst case" tree as a linked list beginning at the root node. This would give a height of `n-1`, where `n` is the number of nodes, and it is easy to see that this would literally be `O(n)` if we moved height-wise. 

============================================================================

Now, we have understood what a tree is. 

To introduce the necessity of binary lifting, begin with a simple question: how do we find the `k`th ancestor of a node? Assume that nonexistent ancestors are node value `-1` by default.

There is a very simple `O(k)` solution: move up `k` times, taking parents of parents iteratively \(similar to how we described ancestors before\). 

But recall the "worst case" tree is a linked list: this means that moving up `k` times could be as terrible as literally starting at the leaf node and moving to the root in a linked list, which is an `O(n)` procedure. If we wanted to answer `q` such queries for instance, we would literally spend `O(nq)` time, which is really bad in cases where `n,q` are on the order of `1e5` \(as is the case in the majority of problems\). 

How can we optimize then? If you are familiar with the concept that underlies Fenwick trees, we can use a similar clever trick here, and I encourage you to stop reading here and try deriving what I am about to explain. Otherwise, read on. 

Note that any number has a unique representation as a sum of powers of two. This is known as the **binary** representation of the number. For instance, `7` in binary is `111`, so it can be represented as `4+2+1`. 

Why is this useful? Well, instead of jumping up a height of `7` edges from a node, if we knew how we could jump `1`, `2`, and `4` edges, we could simply jump up `1`, `2`, and `4`. This gives us a key insight: for any node, what if we precomputed what nodes we would reach if we jumped up from that node by powers of `2`? This would mean merely `log(n)` different computations for each of `n` nodes, or `n log(n)` preprocessing. Then, we could simply jump any distance `d` by breaking it into powers of two based on its binary representation and using a precomputed table to answer queries in `log(d)` time.  If `d` is even on the order of `n` as in the "worst case" tree \(linked list\), we can still answer `q` queries in this way in `O(n log(n))` preprocessing and then `O(log(n))` per query for `q` queries, for a total of `O(n log(n) + q log(n))`, which passes in time. 

This is a great idea, but the next step is to see it through in code. 

* For a general picture of what we are about to do, for each node, we want to be able to have a precomputed lookup table for jumping up powers of two. In other words, we want a 2D array `up[node][exponent]` such that we jump from `node` by a distance of `exp(2, exponent)`. We can do this by using the principle of dynamic programming, calculating powers of two iteratively based on results we have already stored. 
  * In particular, we can imagine knowing the value of `up[i][l-1]`, or the node we jump to when we move a distance of `exp(2, l-1)` up from `i`. If we want to find `up[i][l]`, then this is just the same as jumping up `l-1` levels from `i` to some node `m` and then another `l-1` levels \(we know that `exp(2, l) = exp(2, l-1) + exp(2, l-1)`, so it is logical to make this manipulation\). Since `m` would just be `up[i][l-1]`, our target node `up[i][l]` would just be `up[m][l-1] = up[up[i][l-1]][l-1]`. 
  * This gives us an idea of what we want our DP to do. Since we can write `up[i][l] = up[up[i][l-1]][l-1]`, it follows that it makes sense to have the outer for loop to iterate by levels \(since we only want to start computing things at level `l` once we have exhausted level `l-1`, as this will allow us to use our developed recurrence\). 

There are only two caveats remaining that we can iron out for the DP. 

* The first is to find our base cases in this DP. Fortunately, this is simple. Our base cases look like `up[i][0]` for any node `i`, or verbally, we are asking what the `exp(2,0) = 1`st ancestor of node `i` is. This is the same as the parent of node `i`. Sometimes problems are nice and just give us the parents right off the bat, in which case we can just fill in all `up[i][0]` directly. Otherwise, if we just have the raw adjacency list, we need to be a bit more careful. Since we want the parent node of each node, we can start a downward DFS from the root node and fill out parents accordingly. This is what such a DFS would look like: 

```cpp
void dfs(int v, int p) { // keep track of current node and its parent node
    up[v][0] = p; // mark the parent in the array
    vis[v] = 1; // mark the node as visited
    for(int u: adj[v]) if(!vis[u]) dfs(u,v); // visit all unvisited children
}
```

And we can just start this up as `dfs(0,-1)` in the `main` function, recalling that nonexistent ancestors must be node value `-1` \(since the parent of a root does not exist, as noted before\). 

* Now, we have filled out the base cases. All that is left is paying close attention to node values that do not exist. This means that we can `memset` our whole `up` array at `-1` to the beginning, so as we are filling it out, the values that are not updated with existing nodes are kept track of as nonexistent. This allows us to respect that nonexistent ancestors must be node value `-1`. 

In summary, this is our entire preprocessing code, assuming that nodes are given to use in a one-indexed fashion \(meaning that we shift them all down by `1` to normalize for zero-indexing, which is preferred\):

```cpp
#include <bits/stdc++.h>
using namespace std; 

int n,q; 
const int mxn = 1e5 + 5, mxe = log2(mxn) + 5; 
vector<vector<int>> adj; 
int up[mxn][mxe]; 

void dfs(int v, int p) { // keep track of current node and its parent node
    up[v][0] = p; // mark the parent in the array
    vis[v] = 1; // mark the node as visited
    for(int u: adj[v]) if(!vis[u]) dfs(u,v); // visit all unvisited children
}

int main() {
    cin.tie(0)->sync_with_stdio(0);
     
    cin >> n >> q; 
    adj.resize(n); // start the adjacency list with space for n nodes
    
    for(int i = 0; i < n - 1; ++i) { // get in n - 1 edges
        int a,b; cin >> a >> b, --a, --b; 
        adj[a].emplace_back(b), adj[b].emplace_back(a); 
    } 
    
    memset(up, -1, sizeof(up)); // memset up to -1 to begin
    dfs(0,-1); // fill out up base cases
    
    for(int l = 1; l < mxe; ++l) 
        for(int i = 0; i < n; ++i) 
            if(up[i][l-1] != -1) up[i][l] = up[up[i][l-1]][l-1]; 
}
```

Now that we have preprocessed everything and filled out our `up` array, we must answer queries. 

In particular, for each of `q` queries, we are given `node` and `k`, some value to jump up by. We want to jump up by powers of two based on our lookup table `up`, but in order to do so, we must again be careful. One idea is to use a `bitset`, since those give us binary representations for free. But we can do better. 

Recall the bitwise `&` operator. It gives us a comparison between each bit of the first operand to the second. However, since we plan to iterate for some level exponent `l` over only powers of two \(specifically given by `exp(2, l) = 1 << l`\), we know that a bit in common means that this power of two does exist in the binary representation of the desired `k`. If we iterate then over all powers of two and check this continuously, we will have addressed effectively every bit in the binary representation of `k`, which is exactly what we need to do. Once again, as we do this, we make sure that if the node value is `-1` at some point in the process, it remains `-1` throughout and basically skips the rest of the process:

```cpp
#include <bits/stdc++.h>
using namespace std; 

int n,q; 
const int mxn = 1e5 + 5, mxe = log2(mxn) + 5; 
vector<vector<int>> adj; 
int up[mxn][mxe]; 

void dfs(int v, int p) { // keep track of current node and its parent node
    up[v][0] = p; // mark the parent in the array
    vis[v] = 1; // mark the node as visited
    for(int u: adj[v]) if(!vis[u]) dfs(u,v); // visit all unvisited children
}

int main() {
    cin.tie(0)->sync_with_stdio(0);
     
    cin >> n >> q; 
    adj.resize(n); // start the adjacency list with space for n nodes
    
    for(int i = 0; i < n - 1; ++i) { // get in n - 1 edges
        int a,b; cin >> a >> b, --a, --b; 
        adj[a].emplace_back(b), adj[b].emplace_back(a); 
    } 
    
    memset(up, -1, sizeof(up)); // memset up to -1 to begin
    dfs(0,-1); // fill out up base cases
    
    for(int l = 1; l < mxe; ++l) 
        for(int i = 0; i < n; ++i) 
            if(up[i][l-1] != -1) up[i][l] = up[up[i][l-1]][l-1];
    
    for(int i = 0; i < q; ++i) { 
        int node, k; cin >> node >> k, --node; 
        for(int l = 0; l < mxe; ++l) 
            if(x != -1) if(k & (1 << l)) 
                node = up[node][l]; // jump up by the power of 2 at this point
        cout << node << "\n"; 
    } 
}
```

And we're done!

The relevant section in CPH also explains the algorithm fairly well graphically but is not self-contained and lacks code and implementation details:

![Introducing the Problem](.gitbook/assets/image%20%286%29.png)

![Explaining the 2D Preprocessing Table](.gitbook/assets/image%20%287%29.png)

