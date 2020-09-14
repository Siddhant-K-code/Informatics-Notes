# Tree DFS Traversals \(Gold\) Part 1

**What's Covered**

* **Specializations of general DFS for trees \(preorder and postorder, along with some discussion of in-order and reverse in-order\)** 

**Prerequisites**

* **Required**
  * **Basic understanding of what a general graph is in computer science, such as DFS and adjacency lists \(silver\)**
  * **Reading of "Tree DFS Traversals \(Gold\) Intro" for what to expect**
  * **Understanding of tree definition \(given in "Binary Lifting \(Gold\) Part 1"\)**

As you presumably have a silver level understanding of graphs, you know that to tour a general graph starting from a node, we can run either a DFS \(depth first search\) or even BFS \(breadth first search\). It is not guaranteed that the general graph will have only one connected component, so we must "floodfill" \(run the methods iteratively from every node but avoid revisits, ensuring amortization\). 

Trees are a lot more interesting because of the various properties they present. For instance, we can run a single basic DFS and capture every node. The code behind this is silver level and will not be explained to avoid redundancy, but I will include it below for the sake of comparison to what I am about to explain:

```cpp
// a snippet of standard DFS code
// for a tree, we can run this just once from any node (usually the root)

vector<int> adj[mxn]; // adj list
bool vis[mxn]; // keep track of visits

void dfs(int v) { 
    if(vis[v]) return; // do not revisit
    vis[v] = 1; // mark visited
    // process
    for(int u: adj[v]) dfs(u); // visit adj
}
```

But the key realization here is that there are many ways to capture these nodes via DFS:

Specifically, we can identify the below steps for any single general DFS call:

1. Process the current node `v` 
2. Travel to adjacent node `adj[v][0]` and start a DFS call stack to cover its subtree
3. Travel to adjacent node `adj[v][1]` and start a DFS call stack to cover its subtree
4. Travel to adjacent node `adj[v][2]` and start a DFS call stack to cover its subtree
5. ... \(and so on\)

In a general tree, there is no distinction between steps 2 and beyond because there is no way to differentiate those nodes from one another \(all of them are just adjacent nodes we could potentially reach\). 

But an interesting distinction we can make is when we actually choose to process the current node \(specifically step 1\)! This gives rise to two variations: preorder and postorder. 

The **preorder traversal** of a tree does a DFS where, at each call, the current node is processed _before_ traveling to adjacent nodes. 

```cpp
// a snippet of preorder traversal

vector<int> adj[mxn]; // adj list
bool vis[mxn]; // keep track of visits

void dfs(int v) { 
    if(vis[v]) return; // do not revisit
    vis[v] = 1; // mark visited
    // FIRST process
    for(int u: adj[v]) dfs(u); // THEN visit adj
}
```

The **postorder traversal** of a tree does a DFS where, at each call, the current node is processed _after_ traveling to adjacent nodes. 

```cpp
// a snippet of postorder traversal

vector<int> adj[mxn]; // adj list
bool vis[mxn]; // keep track of visits

void dfs(int v) { 
    if(vis[v]) return; // do not revisit
    vis[v] = 1; // mark visited
    for(int u: adj[v]) dfs(u); // FIRST visit adj
    // THEN process
}
```

Note that a third type of DFS, in-order, also exists, but only for binary trees. This is because in a binary tree, we can make a distinction between the left and right children -- and thus the left and right subtrees -- of a node. Then, an in-order DFS would first process the left subtree, then the current node, and finally the right subtree. We could also reverse this process to create a fourth type of DFS, reverse in-order. However, the third and fourth types of DFS are not essential.  

Finally, here are some useful resources I found in posts at [https://stackoverflow.com/questions/9456937/when-to-use-preorder-postorder-and-inorder-binary-search-tree-traversal-strate](https://stackoverflow.com/questions/9456937/when-to-use-preorder-postorder-and-inorder-binary-search-tree-traversal-strate). This also includes an in-order traversal, which is perfectly fine because the original question asked refers to a strictly binary tree. 

![How each traversal works in practice](.gitbook/assets/image%20%2812%29.png)

![When to use each traversal in, say, an algorithmic problem](.gitbook/assets/image%20%2811%29.png)

![Practical applications of the traversals to actually manipulate the structure of a binary tree](.gitbook/assets/image%20%2813%29.png)



