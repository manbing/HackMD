---
title: Data Structures and Algorithms
tags: [Google]

---

# Data
Data type -> Data structure -> Abstract data type

## [Abstract data type](https://en.wikipedia.org/wiki/Abstract_data_type)
In computer science, an abstract data type (ADT) is a mathematical model for data types, defined by its behavior (semantics) from the point of view of a user of the data, specifically in terms of possible values, possible operations on data of this type, and the behavior of these operations.

### [Priority Queue](https://ithelp.ithome.com.tw/articles/10269601)
In a priority queue, each element has an associated priority, which determines its order of service. Priority queue serves highest priority items first.

### Stacks
last in, first out
### Queues
first in, first out

### Trees
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

#### [Trie](https://en.wikipedia.org/wiki/Trie)
* Dictionary tree
In computer science, a trie, also known as a digital tree or prefix tree, is a specialized search tree data structure used to store and retrieve strings from a dictionary or set. 

## [Data structure](https://en.wikipedia.org/wiki/Data_structure)
### Array
### Linked list

### Heap
Max heap
Min heap

### [Hash tables](https://en.wikipedia.org/wiki/Hash_table)
hash set
>stores unique elements without any associated values

hash map
>stores key-value pairs where the keys are unique identifiers and the values are associated data

## [Data type](https://en.wikipedia.org/wiki/Data_type)
basic data types of integer numbers, floating-point numbers, characters and Booleans.

# Algorithms
## Sort

| Algorithms | Worst case | Best case | Average case|
| -------- | -------- | -------- | -------- |
| [Heap sort](https://en.wikipedia.org/wiki/Heapsort) | O($n$ log$n$)  |  O($n$ log$n$)  | O($n$ log$n$)|
| [Quick sort](https://en.wikipedia.org/wiki/Quicksort) | O($n^2$) |  O($n$ log$n$)  | O($n$ log$n$)|
| [Merge sort](https://en.wikipedia.org/wiki/Merge_sort) | O($n$ log$n$) |  O($n$ log$n$)     | O($n$ log$n$)|
| [Bubble sort](https://en.wikipedia.org/wiki/Bubble_sort) | O($n^2$) |  O($n$) |O($n^2$)|
| [Insertion sort](https://en.wikipedia.org/wiki/Insertion_sort)   |   O($n^2$)   |   O($n$)   |O($n^2$)|
| [Selection sort](https://en.wikipedia.org/wiki/Selection_sort)   |   O($n^2$)   | O($n^2$)  |O($n^2$)|


## Search

| Algorithms | Worst case | Best case | Average case |
| -------- | -------- | -------- | -------- |
| [Binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm)    | O(log$n$)     | 1   | O(log$n$)|
| [Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)    | O(log$n$)     | 1   | O(log$n$)|

## Graph Theory
### DFS/BFS
[Depth First & Breadth First Graph Search](https://www.youtube.com/watch?v=TIbUeeksXcI&t=567s)

### Shortest paths
[Dijkstra's algorithm](https://ithelp.ithome.com.tw/articles/10209593)

A\* algorithm

[Prim's algorithm](https://en.wikipedia.org/wiki/Prim%27s_algorithm)

### [Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting)
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

## Union-find Algorithm


[visualising alorighm](https://visualgo.net/en/sorting)

## Greedy algorithm V.S. Dynamic programming
Greedy
> Make the choice **before** solving the subproblem, Top-Down


Dynamic programming
> Make the choice **after** solving the subproblem, Down-Top



# Runtime analysis

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

# Leetcode
* Hashtable
380. Insert Delete GetRandom O(1)
811. Subdomain Visit Count

* Heap Tree
692. Top K Frequent Words
253. Meeting Rooms II

* Recursive
437. Path Sum III
T: O(n)
S: O(n)

# Reference


# Data
Data type -> Data structure -> Abstract data type

## [Abstract data type](https://en.wikipedia.org/wiki/Abstract_data_type)
In computer science, an abstract data type (ADT) is a mathematical model for data types, defined by its behavior (semantics) from the point of view of a user of the data, specifically in terms of possible values, possible operations on data of this type, and the behavior of these operations.

### [Priority Queue](https://ithelp.ithome.com.tw/articles/10269601)
In a priority queue, each element has an associated priority, which determines its order of service. Priority queue serves highest priority items first.

### Stacks
last in, first out
### Queues
first in, first out

### Trees
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

#### [Trie](https://en.wikipedia.org/wiki/Trie)
* Dictionary tree
In computer science, a trie, also known as a digital tree or prefix tree, is a specialized search tree data structure used to store and retrieve strings from a dictionary or set. 

## [Data structure](https://en.wikipedia.org/wiki/Data_structure)
### Array
### Linked list

### Heap
Max heap
Min heap

### [Hash tables](https://en.wikipedia.org/wiki/Hash_table)
hash set
>stores unique elements without any associated values

hash map
>stores key-value pairs where the keys are unique identifiers and the values are associated data

## [Data type](https://en.wikipedia.org/wiki/Data_type)
basic data types of integer numbers, floating-point numbers, characters and Booleans.

# Algorithms
## Sort

| Algorithms | Worst case | Best case | Average case|
| -------- | -------- | -------- | -------- |
| [Heap sort](https://en.wikipedia.org/wiki/Heapsort) | O($n$ log$n$)  |  O($n$ log$n$)  | O($n$ log$n$)|
| [Quick sort](https://en.wikipedia.org/wiki/Quicksort) | O($n^2$) |  O($n$ log$n$)  | O($n$ log$n$)|
| [Merge sort](https://en.wikipedia.org/wiki/Merge_sort) | O($n$ log$n$) |  O($n$ log$n$)     | O($n$ log$n$)|
| [Bubble sort](https://en.wikipedia.org/wiki/Bubble_sort) | O($n^2$) |  O($n$) |O($n^2$)|
| [Insertion sort](https://en.wikipedia.org/wiki/Insertion_sort)   |   O($n^2$)   |   O($n$)   |O($n^2$)|
| [Selection sort](https://en.wikipedia.org/wiki/Selection_sort)   |   O($n^2$)   | O($n^2$)  |O($n^2$)|


## Search

| Algorithms | Worst case | Best case | Average case |
| -------- | -------- | -------- | -------- |
| [Binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm)    | O(log$n$)     | 1   | O(log$n$)|
| [Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)    | O(log$n$)     | 1   | O(log$n$)|

## Graph Theory
### DFS/BFS
[Depth First & Breadth First Graph Search](https://www.youtube.com/watch?v=TIbUeeksXcI&t=567s)

### Shortest paths
[Dijkstra's algorithm](https://ithelp.ithome.com.tw/articles/10209593)

A\* algorithm

[Prim's algorithm](https://en.wikipedia.org/wiki/Prim%27s_algorithm)

### [Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting)
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

## Union-find Algorithm


[visualising alorighm](https://visualgo.net/en/sorting)

## Greedy algorithm V.S. Dynamic programming
Greedy
> Make the choice **before** solving the subproblem, Top-Down


Dynamic programming
> Make the choice **after** solving the subproblem, Down-Top



# Runtime analysis

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

# Leetcode
* Hashtable
380. Insert Delete GetRandom O(1)
811. Subdomain Visit Count

* Heap Tree
692. Top K Frequent Words
253. Meeting Rooms II

* Recursive
437. Path Sum III
T: O(n)
S: O(n)

# Reference


