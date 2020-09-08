# Binary Lifting \(Gold\) Part 2

**What's Covered**

* **LCA definition, properties, and importance**
* **Calculation using binary lifting based on understanding developed from the first part**

**Prerequisites**

* **Required**
  * **All prerequisites carry over from "Binary Lifting \(Gold\) Part 1"**
  * **A thorough understanding of binary lifting from "Binary Lifting \(Gold\) Part 1"**
  * **An idea of binary search \(silver\)**

In the first part, we discovered a way to lift up any distance from a node in a tree in merely logarithmic time. This enabled us to efficiently answer problems demanding multiple queries. Now, we are ready to take on a more challenging but related problem. 

We must efficiently compute the lowest common ancestor, or **LCA**, of two nodes \(and support `q` queries like before\). Based on just the words "lowest common ancestor," you can infer what the problem looks like, but formally, we must find the node `x` in the tree such that `x` is an ancestor of both nodes `a` and `b` but has maximum depth \(is "lowest" or as far from the root as possible\). 

Why is this problem important? LCA pops up not only in numerous competitive programming problems regarding tree traversals or reaching one node from another, but it also has real world significance when studying how inheritance classes can work in object-oriented programming. 

To address the problem, we first pose it in a different way. Imagine looking at nodes `a` and `b` on the tree. We have two cases:

* Both nodes are at the same depth. This is very interesting because then both nodes are equidistant \(the same distance away\) from their LCA as well. In other words, we achieve a great simplification: the problem reduces to finding the minimum jump distance `k` for which both nodes end up at the same final node \(an ancestor to both nodes, and in the minimum case for `k`, actually the LCA\). 
* Both nodes are at different depths. This is problematic because we do not reach any simplification immediately. 

Fortunately, the second case can be transformed into the first! How? Since we already know the depths of all nodes in the tree \(based on a simple DFS from the root\), we can just jump one of the nodes up to the same depth as the other in logarithmic time based on binary lifting!

Rawly, we can do something of the following nature, where `lift()` is just a binary lifting function:

```cpp
lift(a, dep[a] - min(dep[a], dep[b])); 
lift(b, dep[b] - min(dep[a], dep[b])); 
```

For completeness, we also separate `lift()` out as a function from the code from the first part, adding an `lca()` function and fixing the nature of our queries:

```cpp
#include <bits/stdc++.h>
using namespace std; 

int n,q; 
const int mxn = 1e5 + 5, mxe = log2(mxn) + 5; 
vector<vector<int>> adj; 
int up[mxn][mxe], dep[mxn]; 
bool vis[mxn];

void dfs(int v, int p, int d) { // keep track of current, parent, and depth
    up[v][0] = p; // mark the parent
    dep[v] = d; // mark the depth
    vis[v] = 1; // mark the node as visited
    for(int u: adj[v]) if(!vis[u]) dfs(u,v,d+1); // visit all unvisited children
}

void lift(int &node, int dist) { // pass node by reference and lift
    for(int l = 0; l < mxe; ++l) 
        if(node != -1) if(dist & (1 << l)) 
            node = up[node][l]; // jump up by the power of 2 at this point
}

int lca(int a, int b) {
    lift(a, dep[a] - min(dep[a], dep[b])); 
    lift(b, dep[b] - min(dep[a], dep[b])); 
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
    dfs(0,-1,0); // fill out up base cases and depths
    
    for(int l = 1; l < mxe; ++l) 
        for(int i = 0; i < n; ++i) 
            if(up[i][l-1] != -1) up[i][l] = up[up[i][l-1]][l-1];
    
    for(int i = 0; i < q; ++i) {
        int a,b; cin >> a >> b, --a, --b; 
        cout << lca(a,b) + 1 << "\n"; 
    }
}
```

Going back to our problem, the nodes are now at a common depth, and we must find the value `k` that works to binary jump by and reach the LCA. One natural approach now is binary searching on `k`. Why? 

This is because we know that our "LCA categorization" monotonically increases. In other words, we know that for small `k`, we will jump to different nodes from `a` and `b`, but for large enough `k`, we will jump to an ancestor of both nodes anyway. Thus, there is a "sweet spot" that comes _right before_ the LCA. If we binary search for this sweet spot, we can just take its parent to get our LCA. 

The question is, how do we binary search? We can think of what we did with binary lifting, when we already knew what distance we wanted to jump and took its binary representation. So what if we take the binary representation of `k` and binary search on that? We can actually look at the individual exponents in the powers of two that fit into `k` and always include those!

We can structure this as follows. First, we start with the maximum possible exponent, `mxe`. Then, if this overshoots our sweet spot and takes us to the same node from both `a` and `b`, we strategically choose to ignore it. Then, we repeat with `mxe-1`. Of course, at some point, there will be an exponent `e` such that jumping by `exp(2, e)` no longer overshoots but is just right. In that case, we can actually jump both nodes up by `exp(2, e)` and continue to binary search on the remaining distance. 

Then, once we locate the sweet spot, we take its parent. 

The one case to be careful of is if `a` and `b` are already equal, in which case we return `a`. 

In all, this looks like the following: 

```cpp
int lca(int a, int b) {
    lift(a, dep[a] - min(dep[a], dep[b])); 
    lift(b, dep[b] - min(dep[a], dep[b])); 
    if(a == b) return a;
    for(int l = mxe - 1; l >= 0; --l)
        if(up[a][l] != up[b][l]) a = up[a][l], b = up[b][l]; 
    return up[a][0]; 
}
```

And, surprisingly, the process is as simple as this! We are now done and can integrate the function back into our overall code:

```cpp
#include <bits/stdc++.h>
using namespace std; 

int n,q; 
const int mxn = 1e5 + 5, mxe = log2(mxn) + 5; 
vector<vector<int>> adj; 
int up[mxn][mxe], dep[mxn]; 
bool vis[mxn];

void dfs(int v, int p, int d) { // keep track of current, parent, and depth
    up[v][0] = p; // mark the parent
    dep[v] = d; // mark the depth
    vis[v] = 1; // mark the node as visited
    for(int u: adj[v]) if(!vis[u]) dfs(u,v,d+1); // visit all unvisited children
}

void lift(int &node, int dist) { // pass node by reference and lift
    for(int l = 0; l < mxe; ++l) 
        if(node != -1) if(dist & (1 << l)) 
            node = up[node][l]; // jump up by the power of 2 at this point
}

int lca(int a, int b) {
    lift(a, dep[a] - min(dep[a], dep[b])); 
    lift(b, dep[b] - min(dep[a], dep[b])); 
    if(a == b) return a;
    for(int l = mxe - 1; l >= 0; --l)
        if(up[a][l] != up[b][l]) a = up[a][l], b = up[b][l]; 
    return up[a][0]; 
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
    dfs(0,-1,0); // fill out up base cases and depths
    
    for(int l = 1; l < mxe; ++l) 
        for(int i = 0; i < n; ++i) 
            if(up[i][l-1] != -1) up[i][l] = up[up[i][l-1]][l-1];
    
    for(int i = 0; i < q; ++i) {
        int a,b; cin >> a >> b, --a, --b; 
        cout << lca(a,b) + 1 << "\n"; 
    }
}
```

The relevant section in CPH also explains the algorithm fairly well graphically but is not self-contained and lacks code and implementation details:

![Introducing the Problem](.gitbook/assets/image%20%2810%29.png)

![Introducing the Method](.gitbook/assets/image%20%288%29.png)

![Time Complexity Analysis](.gitbook/assets/image%20%289%29.png)

