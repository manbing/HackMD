# Data Structures and Algorithms
###### tags: `Google`

[![hackmd-github-sync-badge](https://hackmd.io/Q_2yAYg7SDOIBdxc7hFbDQ/badge)](https://hackmd.io/Q_2yAYg7SDOIBdxc7hFbDQ)

[visualising alorighm](https://visualgo.net/en/sorting)


## Data structure
### Array
### Linking list
### Stak
### Heap
Max heap
Min heap
### Queue
### [Priority Queue](https://ithelp.ithome.com.tw/articles/10269601)
### Hash table
hash set
>stores unique elements without any associated values

hash map
>stores key-value pairs where the keys are unique identifiers and the values are associated data
### Tree
Tree traversal: DFS, BFS

#### Binary tree
Depth First Traversals:
1. Inorder (Left, Root, Right)
2. Preorder (Root, Left, Right)
3. Postorder (Left, Right, Root)

#### Binary search tree
Left < Root < Right


#### Tree time analysis

* Worst case

| Algorithms | Search | Insert | Delete |
| -------- | -------- | -------- | -------- |
| [Red-black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree) |  O(log$n$)  | O(log$n$) | O(log$n$)|
| [AVL tree](https://en.wikipedia.org/wiki/AVL_tree) |  O(log$n$) |  O(log$n$) | O(log$n$) |
| [Binary serach tree](https://en.wikipedia.org/wiki/Binary_search_tree) |  O($n$) | O($n$)  | O($n$) |
| Binary Heap tree |  1 (*max/min*) | O(log$n$)  | 1 (*max/min*) |
| [Binary tree](https://en.wikipedia.org/wiki/Binary_tree) | O($n$)  | O($n$)  | O($n$) |


* Average case

| Algorithms | Search | Insert | Delete |
| -------- | -------- | -------- | -------- |
| Red-black tree |  O(log$n$) |  O(log$n$) | O(log$n$)|
| AVL tree | O(log$n$)  |  O(log$n$)  | O(log$n$) |
| Binary serach tree |  O(log$n$) | O(log$n$)  |O(log$n$) |
| Binary Heap tree |   1 (*max/min*) |  O(log$n$) |  1 (*max/min*) |
| Binary tree |  O($n$) | O($n$)  | O($n$) |





## Algorithms
### Sort

| Algorithms | Worst case | Best case | Average case|
| -------- | -------- | -------- | -------- |
| [Heap sort](https://en.wikipedia.org/wiki/Heapsort) | O($n$ log$n$)  |  O($n$ log$n$)  | O($n$ log$n$)|
| [Quick sort](https://en.wikipedia.org/wiki/Quicksort) | O($n^2$) |  O($n$ log$n$)  | O($n$ log$n$)|
| [Merge sort](https://en.wikipedia.org/wiki/Merge_sort) | O($n$ log$n$) |  O($n$ log$n$)     | O($n$ log$n$)|
| [Bubble sort](https://en.wikipedia.org/wiki/Bubble_sort) | O($n^2$) |  O($n$) |O($n^2$)|
| [Insertion sort](https://en.wikipedia.org/wiki/Insertion_sort)   |   O($n^2$)   |   O($n$)   |O($n^2$)|
| [Selection sort](https://en.wikipedia.org/wiki/Selection_sort)   |   O($n^2$)   | O($n^2$)  |O($n^2$)|


### Search

| Algorithms | Worst case | Best case | Average case |
| -------- | -------- | -------- | -------- |
| [Binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm)    | O(log$n$)     | 1   | O(log$n$)|


### Graph Theory
#### DFS/BFS
[Depth First & Breadth First Graph Search](https://www.youtube.com/watch?v=TIbUeeksXcI&t=567s)

#### Shortest paths
[Dijkstra's algorithm](https://ithelp.ithome.com.tw/articles/10209593)
A\* algorithm

#### [Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting)
a directed graph

Kahn's algorithm
| Step | Action |
| -------- | -------- |
| 0     | Find indegree for all nodes.     |
| 1     | Identify a node with no incoming edges.     |
| 2     | Remove the node from the graph and add it to the ordering.     |
| 3     | Remove the node’s out-going edges from the graph.     |
| 4     | Decrement the indegree where connected edges were deleted.     |
| 5     | Repeat Steps 1 to 4 till there are no nodes left with zero indegree.     |
| 6     | Check if all elements are present in the sorted order.     |
| 7     | If the result of Step 6 is true, we have the sorted order. Else, no topological ordering exists.     |
| 8     | Exit.     |

### Union-find Algorithm

## Runtime analysis

X is NP-complete if only if X $\in$ NP $\&$ X is NP-hard.
X is NP-hard if only if every problem y $\in$ NP can be reduces to X.

Reduction from problem A to problem B is polynomial time algorithm converting A inputs to equivalent B inputs.

if B $\in$ P, then A $\in$ P
if B $\in$ NP, then A $\in$ NP

How to prove X is NP-complete?
1. X $\in$ NP
*     nondeterministic algorithm
*     certificate and verifier

2. reduces from some know NP-complete problem Y to X


[asymptotic-notation](https://www.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/asymptotic-notation)
[Big-O cheat sheet](https://www.bigocheatsheet.com/)
[16. Complexity: P, NP, NP-completeness, Reductions](https://www.youtube.com/watch?v=eHZifpgyH_4)


