+++
date = '2025-08-09T13:19:20+01:00'
draft = true
title = 'Graph Traversal'
summary = 'Everything you wanted to know about going down and along trees'
toc = true
math = true
+++
### Graph Traversal

#### Representing Graphs
We can represent graphs in multiple ways.
- Adjacency List. Each node has a list of nodes to which there is an edge from
  that node, stored as an array of vectors (in Cpp), or just a normal list of
  lists in python. Each element is a list representing the
  edges attached to that node.
  
  e.g. `[[2], [3,4], [4], [1]]` !! Assuming we index from 1 and not 0
  
  or `vector<int> adj[n]` where n is the number of nodes.
  
  Then `adj[1].push_back(2)` adds edge 2 to node 1, etc.
  In python we would do `adj[1].append(2)`
  
  Weighted graphs can be represented with `vector<pair<int, int>> adj[N]` or maybe
  by using a list of dicts in python?
  
- Adjacency matrix. Using matrices and graph theory here. We can represent the
  graph as a matrix by adding a 1 to an n\*n matrix, whre n is the number of
  nodes, at each point where two nodes share an edge. For example, if nodes 1
  and 2 share an edge, we add a 1 at 1,2. Representing weights is easier due to
  the fact that we can just add the weight at the relevant location, e.g. 5
  instead of 1.
  
  However, there are a lot of zeroes, especially for larger graphs. We don't
  generally like sparse matrices.
  
- Edge list. In a vector of pairs or another python list of lists, we simply
  represent the edge relationships as pairs of nodes that share an edge. e.g. if
  1, 2 share an edge, we have `edges.push_back({1,2})` in Cpp, or
  `edges.append([1,2])` in python.
  
  We can also add a value for the weight of the edge.

  Time complexity is O(m+n) because it processes n nodes and m edges once.

#### Depth-First Search.

DFS begins at a given node, then proceeds to nodes reachable from there using
the edges of the graph. Once it visits all neighbour nodes, it returns to
previous nodes, checking if the neighbours have been seen, and so on until it
finds an unvisited, until all nodes are visited.

Implementation:
```cpp
//Assuming adjacency list 
vector<int> adj[N];

//Array to keep track of visited nodes
bool visited[N];

//Check if node is visited, if so return, else set visited[s] to true
void dfs(int s) {
  if(visited[s]) = true;
  visited[s] = true;
  // process node s
  // iterates over the neighbors of node s
  for (auto u: adj[s]) {
    dfs(u);
  }
}

```

#### Breadth-First Search

This begins at a given node, then visits all the other nodes at an increasing
distance from the starting node. Thus, we can calculate the distance from the
starting node to other nodes using  breadth first search. This is harder to
implement, though.

We start from our first node, then add the neighbours to the queue. As opposed
to the stack in DFS, which is FIFO, the queue in BFS is LIFO. We popleft() to
take the next element in a queue, whereas we pop() to take off the rearmost
element in a stack, if we are adding elements by append().

An example implementation in cpp.
The queue q contains all the nodes to be visited in order of their distance.
Visited contains then already seen nodes, and the array ddistance contains teh
distances from the starting node to all nodes of the graph.

```cpp
queue<int> q;
bool visited[N];
int distance[N];

//starting at node x

visited[x] = true;
distance[x] = 0;
q.push(x);

while(!q.empty()) {
  int s = q.front(); q.pop();
  
  for (auto u : adj[s]) {
    if (visited[u]) continue;
    visited[u] = true;
    distance[u] = distance[s]+1
    q.push(u);
  }
}
```

Time complexity is O(m+n) where we have n nodes and m edges.

### Shortest paths
#### Bellman-Ford

This algorithm finds the shortest path from a starting node to all nodes of the
graph. It can cope with any kind of graph except those containing a cycle of
negative length.

On each round of the algorithm it goes through all the edges of the graph to try
to find the smallest distances.

```cpp
for (int i; i <= n; i++) distance[i] = INF;
distance[x] = 0;
for (int i = 1; i <= n-1; i++) {
  for (auto e : edges) {
    int a, b, w;
    tie(a, b, w) = e;
    distance[b] = min(distance[b], distance[a]+w);
  }
}
```

Time complexity is O(nm) because it consists of n-1 rounds and iterates through
all m edges.

#### Dijkstra's Algorithm

Like with BF, we initially set the distance to the origin node to 0 and the
distances to every other node to infinity. At each step the algorithm checks the
closes possible node that has not yet been seen. (Algorithms that do this "local
optimisation" are called a "greedy").

Once a node is picked, the algorithm then does the same, picking the closes
unvisited node. As with BF, this algorithm doesn't work with graphs with
negative edges.

The most efficient way to implement this is to use a priority queue - the
neighbour nodes are ordered by distance, meaning it will be able to retrieve the
next node in logarithmic time.

In the below implementation, the priority queue q contains pairs of the form
(-d, x), meaning that the distance to x is d. We also have an array distance to
keep track of distances to each node, and processed to keep track of whether
we've seen that node yet, like in DFS and BFS.

The distances are negative because the Cpp priority queue DS finds maximum
elements, whereas we want to find minimum elements.

Implementation below:
```cpp
for(int i = 1l i <= n; i++) distance[i] = INF;
distance[x] = 0;
q.push({0, x});
while(!q.empty()) {
  int a = q.top().second; q.pop();
  if (procesed[a]) continue;
  processed[a] = true;
  for (auto u : adj[a]) {
    
  }
}
```

Time complexity is O(n + mlogm)
