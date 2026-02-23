#revision-1 #CanBeImplementedWithoutRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351265/assignment/problems/376?navref=cl_tt_lst_nm
## Understanding:
- Given A Islands
- M bridges connecting the islands.
- Find the bridges with minimum cost that connects the given islands.
## Input and Output:
![[Screenshot 2026-01-20 at 9.32.04 PM.png]]
![[Screenshot 2026-01-20 at 9.32.24 PM.png]]
 ![[Screenshot 2026-01-20 at 9.32.39 PM.png]]
 
## Problem Constraints:
![[Screenshot 2026-01-20 at 9.33.10 PM.png]]
## Approach:
### Brute Force:
- Prim's Algorithm = Find minimum weight to connect all the nodes using the given edges.
- We need to connect the given nodes, It should contain all the nodes but only connected with minimum possible weight edges.
- This is nothing but creating a spanning tree (nodes connected with less number of edges) with minimum weight.
- Create Adjacency List with connecting nodes and its weight with current node. Here the way is bidirectional.
- Create a visited array to check if the given nodes are connected.
- Create a min Heap to give always the minimum weight edge possible.
- Choose any node at first as all the node needs to be part of the result graph.
- Add the adjacency list of the selected node to the min Heap.
- Iterate the min Heap and start finding the unvisited nodes with min weight.
- Once the min Heap becomes empty return the total weight consumed.
- Complexity:
	- Time Complexity: O(E log E) as we are inserting all the edges of all the nodes.
	- Space Complexity: O(V) + O(E), O(V) for visited array, O(E) for min heap.
### Reference:
![[WhatsApp Image 2026-01-20 at 9.40.52 PM.jpeg]]
### Code
```Java
class Bridge{
	public int weight;
	public int toNode;
	public Bridge(int weight, int toNode){
		this.weight = weight;
		this.toNode = toNode;
	}
}

private int findMinCost(int nodes, int[][] bridges){
	// Create Adjacency List
	ArrayList<ArrayList<Bridge>> adjList = new ArrayList<>();
	for(int i = 0; i <= nodes; i++) adjList.add(new ArrayList<>());
	for(int[] bridge : bridges){
		int start = bridge[0];
		int end = bridge[1];
		int weight = bridge[2];
		adjList.get(start).add(new Bridge(weight, end));
		adjList.get(end).add(new Bridge(weight, start));
	}
	
	boolean[] visited = new boolean[nodes + 1];
	PriorityQueue<Bridge> minHeap = 
		new PriorityQueue<>(Comparator.comparing(bridge -> bridge.weight));
	
	visited[1] = true;
	for(Bridge bridge : adjList.get(1)) minHeap.add(bridge);
	
	int minCost = 0;
	while(!minHeap.isEmpty()){
		Bridge minBridge = minHeap.poll();
		if(visited[minBridge.toNode]) continue;
		else{
			visited[minBridge.toNode] = true;
			minCost += minBridge.weight;
			minCost %= 1000000007;
			for(Bridge bridge : adjList.get(minBridge.toNode)){
				if(!visited[bridge.toNode]) minHeap.add(bridge);
			}
		}
	}
	return minCost;
}

```


