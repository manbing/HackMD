# Data Structures and Algorithms
[visualising alorighm](https://visualgo.net/en/sorting)

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

#### Topological sorting
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


## Data structure
### Array
### Linking list
### Stak
### Queue
### Hsh table
### Tree
**Binary tree**
**Binary serach tree**
**Red black tree**
**AVL tree**
**Heap tree**





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

