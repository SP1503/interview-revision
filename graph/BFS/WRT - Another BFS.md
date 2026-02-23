#revision-1 #revision-2 #NeedRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351262/assignment/problems/4707?navref=cl_tt_lst_nm
## Understanding:
- Given weighted, undirected graph with V nodes and E edges.
- The weight of the edge can be between 1 <= weight <= 2
- Find the shortest distance from A to B.
## Input and Output:
![[Screenshot 2026-01-21 at 6.58.13 PM.png]]
![[Screenshot 2026-01-21 at 6.58.26 PM.png]]
![[Screenshot 2026-01-21 at 6.58.41 PM.png]]
## Problem Constraints:
## Approach:
### Brute Force:
- We can use BFS to find the shortest distance between two nodes.
- But here the graph is weighted. One easy point is weight can be between 1 and 2.
- Hence for edge with weight 1 we can consider as it is.
- But if the weight of the edge is 2, then we can split that edge into two by having a dummy node in between.
- This results in a graph with edges whose weight is always 1.
- So with this simplified graph by using BFS we can find the min distance we have between A and B.
- **Complexity:**
	- **Time Complexity:** 
		- We are using visited array.
		- Hence the unique nodes are visited only once. 
		- Hence the time complexity will be O(V + E), Here we can divide every E into 2 E based on the given weight. Hence the time complexity can be O(V + 2 E) that can be O(V + E).
	- **Space Complexity:**
		- O(V), we will insert every vertices at-least once in the queue.
### Reference:
### Code
```Java

class Pair{
	int node;
	int distance;
	public Pair(int node, int distance){
		this.node = node;
		this.distance = distance;
	}
}

private int findMinDist(int vertices, int[][] edges, int from, int to){
	// Edge cases
	// If source and destination same then return 0
	if(from == to) return 0;
	
	// Create Adjacency list
	List<List<Integer>> adjList = new ArrayList<>();
	int dummyNode = vertices;
	
	// Populate index with list
	for(int i = 0; i < vertices; i++) adjList.add(new ArrayList<>());
	
	for(int[] edge : edges){
		int start = edge[0];
		int end = edge[1];
		int weight = edge[2];
		if(weight == 1){
			adjList.get(start).add(end);
			adjList.get(end).add(start);
		}
		else{
			// Here the weight is 2,
			// Hence dividing the edge into two with dummy node in middle
			adjList.add(new ArrayList<>());
			adjList.get(start).add(dummyNode);
			adjList.get(dummyNode).add(start);
			adjList.get(end).add(dummyNode);
			adjList.get(dummyNode).add(end);
			dummyNode++;
		}
	}
	
	// Do BFS
	boolean[] visited = new boolean[dummyNode];
	
	Queue<Pair> queue = new LinkedList<>();
	
	// Marking the source as visited
	visited[from] = true;
	// Adding the source adjacency into the queue
	for(Integer node : adjList.get(from)) queue.add(new Pair(node, 1));
	
	while(!queue.isEmpty()){
		Pair currNode = queue.poll();
		visited[currNode.node] = true;
		// If current node is the destination return the current distance
		if(currNode.node == to) return currNode.distance;
		
		// Adding all the unvisited adjacency nodes into the queue to check 
		for(Integer node : adjList.get(currNode.node)){
			if(!visited[node]) queue.add(new Pair(node, currNode.distance + 1));
		}
	}
	
	// If no path found return -1
	return -1;
}

```


