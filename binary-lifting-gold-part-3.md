# Binary Lifting \(Gold\) Part 3

**What's Covered**

* **What is node to node distance and why is it important**
* **How to use LCA from binary lifting to calculate node to node distance**

**Prerequisites**

* **Required**
  * **All prerequisites carry over from "Binary Lifting \(Gold\) Part 1" and "Binary Lifting \(Gold\) Part 2"**
  * **A thorough understanding of binary lifting from "Binary Lifting \(Gold\) Part 1" and a thorough understanding of LCA computation from "Binary Lifting \(Gold\) Part 2"**

**Disclaimer**

* **This post, although considerably self-contained and full of information with great emphasis on obscure ideas, should be read and understood with care and ample time. Follow along with each piece of code as it is presented; I recommend you to have your own code file as well. This ensures that the code will be built up in digestible chunks instead of overwhelming and difficult ones.** 

Now that the first two parts have covered binary lifting and LCA computation, we have everything we need to efficiently query the distance between two nodes on a tree. This is simply defined as the length of the unique path from one node to the other \(the number of edges on that path\) in the tree. 

It is straightforward to note that the path from one node to another must include the LCA of the two nodes. In particular, the strategy we can use is first jumping to the LCA and then jumping down from the LCA. This means there are two parts to our calculation, `upd` and `downd`, and the answer is the sum of them. To actually calculate `upd` and `downd`, note that we are only traveling a distance up or down the tree based on the difference in depths between the LCA and a node. 

As such, we can write the following, where the whole array of depths has already been filled through the initial DFS: 

```cpp
int distance(int a, int b) { 
    int upd = dep[a] - dep[lca(a,b)]; 
    int downd = dep[b] - dep[lca(a,b)]; 
    return upd + downd; 
}
```

More frequently, you will see this condensed as follows, removing the intermediate variables:

```cpp
int distance(int a, int b) {
    return dep[a] + dep[b] - 2 * dep[lca(a,b)]; 
}
```

Then, to make a working program and answer `q` queries like we have done before, we can write the following code: 

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

void lift(int node, int dist) { // pass node and lift
    for(int l = 0; l < mxe; ++l) 
        if(node != -1) if(dist & (1 << l)) 
            node = up[node][l]; // jump up by the power of 2 at this point
    return node; 
}

int lca(int a, int b) {
    a = lift(a, dep[a] - min(dep[a], dep[b])); 
    b = lift(b, dep[b] - min(dep[a], dep[b])); 
    if(a == b) return a;
    for(int l = mxe - 1; l >= 0; --l)
        if(up[a][l] != up[b][l]) a = up[a][l], b = up[b][l]; 
    return up[a][0]; 
}

int distance(int a, int b) {
    return dep[a] + dep[b] - 2 * dep[lca(a,b)]; 
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
        cout << distance(a,b) << "\n"; 
    }
}
```

And, surprisingly, we have solved the problem. 

Part 4, if made, will address more advanced problems based on the concepts from the first three parts, but for now, the next series will be "Tree DFS Traversals."

