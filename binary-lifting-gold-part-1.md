# Binary Lifting \(Gold\) Part 1

If you haven't read the intro yet, now would be a good time to do so. 

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

To introduce the necessity of binary lifting, begin with a simple question: how do we find the `k`th ancestor of a node? 

There is a very simple `O(k)` solution: move up `k` times, taking parents of parents iteratively \(similar to how we described ancestors before\). 

But recall the "worst case" tree is a linked list: this means that moving up `k` times could be as terrible as literally starting at the leaf node and moving to the root in a linked list, which is an `O(n)` procedure. If we wanted to answer `q` such queries for instance, we would literally spend `O(nq)` time, which is really bad in cases where `n,q` are on the order of `1e5` \(as is the case in the majority of problems\). 

How can we optimize then? If you are familiar with the concept that underlies Fenwick trees, we can use a similar clever trick here, and I encourage you to stop reading here and try deriving what I am about to explain. Otherwise, read on. 

Note that any number has a unique representation as a sum of powers of two. This is known as the **binary** representation of the number. For instance, `7` in binary is `111`, so it can be represented as `4+2+1`. 

Why is this useful? Well, instead of jumping up a height of `7` edges from a node, if we knew how we could jump `1`, `2`, and `4` edges, we could simply jump up `1`, `2`, and `4`. This gives us a key insight: for any node, what if we precomputed what nodes we would reach if we jumped up from that node by powers of `2`? This would mean merely `log(n)` different computations for each of `n` nodes, or `n log(n)` preprocessing. Then, we could simply jump any distance `d` by breaking it into powers of two based on its binary representation and using a precomputed table to answer queries in `log(d)` time.  If `d` is even on the order of `n` as in the "worst case" tree \(linked list\), we can still answer `q` queries in this way in `O(n log(n))` preprocessing and then `O(log(n))` per query for `q` queries, for a total of `O(n log(n) + q log(n))`, which passes in time. 

This is a great idea, but the next step is to see it through in code. 

