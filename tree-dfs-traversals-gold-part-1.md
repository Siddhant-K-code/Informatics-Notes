# Tree DFS Traversals \(Gold\) Part 1

**What's Covered**

* **Specializations of general DFS for trees \(preorder, in-order, postorder\)** 

**Prerequisites**

* **Required**
  * **Basic understanding of what a general graph is in computer science, such as DFS and adjacency lists \(silver\)**
  * **Reading of "Tree DFS Traversals \(Gold\) Intro" for what to expect**
  * **Understanding of tree definition \(given in "Binary Lifting \(Gold\) Part 1"\)**

To tour a general graph starting from a node, we can run either a DFS \(depth first search\) or BFS \(breadth first search\). It is not guaranteed that the general graph will have only one connected component, so we must "floodfill" \(run the methods iteratively from every node but avoid revisits, ensuring amortization\). 

Trees are a lot more interesting because of the various properties they present. If we root a tree arbitrarily, we can run a single basic DFS and capture every node. The code behind this is silver level and not presented here. 

But the key realization here is that there are many ways to capture these nodes via DFS. Think of these all as different



