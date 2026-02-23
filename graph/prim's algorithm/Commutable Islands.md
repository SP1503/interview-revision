#revision-1 #NeedRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351265/assignment/problems/376?navref=cl_tt_lst_nm
## Understanding:
- Given A Islands
- M bridges connecting the islands.
- Find the bridges with minimum cost that connects the given islands.
## Input and Output:
![[Screenshot 2026-01-20 at 8.34.56 PM.png]]
![[Screenshot 2026-01-20 at 8.35.10 PM.png]]
![[Screenshot 2026-01-20 at 8.35.25 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-20 at 8.35.41 PM.png]]
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
- **Complexity:**
	- **Time Complexity**: O(E log E) as we are inserting all the edges of all the nodes.
	- **Space Complexity:** O(V) + O(E), O(V) for visited array, O(E) for min heap.
### Reference:![[WhatsApp Image 2026-01-20 at 9.14.34 PM.jpeg]]
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

  

private int findMinCost(int islands, int[][] bridges){
	
	// Create adjacency listwith biderectional edges with weight
	ArrayList<ArrayList<Bridge>> adjList = new ArrayList<>();
	for(int i = 0; i <= islands; i++) adjList.add(new ArrayList<>());
	for(int[] bridge : bridges){
		int start = bridge[0];
		int end = bridge[1];
		int weight = bridge[2];
		adjList.get(start).add(new Bridge(weight, end));
		adjList.get(end).add(new Bridge(weight, start));
	}
	
	// Creating visited array to check if all the nodes are connected
	boolean[] visited = new boolean[islands + 1];
	
	// To fecth the edge with minimum weight always
	PriorityQueue<Bridge> minHeap = 
			new PriorityQueue<>(Comparator.comparing(bridge -> bridge.weight));
	
	// By default choosing the node 1 
	// and inserting all the adjacency nodes into heap
	visited[1] = true;
	for(Bridge bridge: adjList.get(1)){
		minHeap.add(bridge);
	}
	
	// Finding the min cost to connect all the nodes with minimum edges
	int minCost = 0;
	while(!minHeap.isEmpty()){
		// Fetch the minimum edge
		Bridge minBridge = minHeap.poll();
		
		// If the node is already visited continue
		if(visited[minBridge.toNode]) continue;
		else{
			// mark the current node as visited and add the cost
			visited[minBridge.toNode] = true;
			minCost += minBridge.weight;
			
			// Add the adjacency non visited nodes into the heap
			for(Bridge bridge: adjList.get(minBridge.toNode)){
				if(!visited[bridge.toNode]) minHeap.add(bridge);
			}
		}
	}
	
	// return the min cost
	return minCost;
}

```

