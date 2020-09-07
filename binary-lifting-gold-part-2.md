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

We must efficiently compute the lowest common ancestor, or **LCA**, of two nodes. Based on just the words "lowest common ancestor," you can infer what the problem looks like, but formally, we must find the node `x` in the tree such that `x` is an ancestor of both nodes `a` and `b` but has maximum depth \(is "lowest" or as far from the root as possible\). 

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

For completeness, we also separate `lift()` out as a function from the code from the first part:

```cpp
#include <bits/stdc++.h>
using namespace std; 

int n,q; 
const int mxn = 1e5 + 5, mxe = log2(mxn) + 5; 
vector<vector<int>> adj; 
int up[mxn][mxe], dep[mxn]; 

void dfs(int v, int p, int d) { // keep track of current, parent, and depth
    up[v][0] = p; // mark the parent
    dep[v] = d; // mark the depth
    vis[v] = 1; // mark the node as visited
    for(int u: adj[v]) if(!vis[u]) dfs(u,v,++d); // visit all unvisited children
}

void lift(int &node, int dist) { // pass node by reference and lift
    for(int l = 0; l < mxe; ++l) 
        if(node != -1) if(dist & (1 << l)) 
            node = up[node][l]; // jump up by the power of 2 at this point
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
}
```

