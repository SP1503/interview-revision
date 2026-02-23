#revision-1 #revision-2 #NeedRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351262/assignment/problems/4706?navref=cl_tt_lst_nm
## Understanding:
- Given undirected, weighted graph.
- Given a source node, vertices count A and M edges.
- Find the minimum distance between source and other nodes possible.
## Input and Output:
![[Screenshot 2026-01-23 at 6.44.18 AM.png]]
![[Screenshot 2026-01-23 at 6.44.47 AM.png]]
![[Screenshot 2026-01-23 at 6.45.13 AM.png]]
## Problem Constraints
![[Screenshot 2026-01-23 at 6.45.27 AM.png]]

## Approach:
### Brute Force:
- **Idea:** The minimum distance between source and other nodes can be found using **Dijkstra Algorithm**.
- **What Dijkstra Algorithm says ?**
	- Same as prim's algorithm always go choose the edge with minimum weight.
	- Add the weight to the **distance[]** of index reached node. 
	- From the currently reached node what are all the nodes can be reached add that nodes information in head with weight = distance[currentNode] + edge weight, node = node.
	- This will continuously give the node that is reachable with smallest weight.
	- Comparing the distance[], check whether this can be minimum or not.
	- If minimum update the distance[], minHeap.
	- If not proceed with next minimum.
- **Complexity:**
	- **Time Complexity:** O(E) for adjacency list calculation + O(E logE) for adding edges in min heap
	- **Space Complexity:** O(V + V) for adjacency list, O(E) for min heap
### Reference:
![[WhatsApp Image 2026-01-23 at 7.09.38 AM.jpeg]]
![[WhatsApp Image 2026-01-23 at 7.09.38 AM (1).jpeg]]
### Code:
```Java

private int[] findMinDistanceBetweenNodes(
int vertexCount, 
int[][] edges, 
int source){
	
	// Find Adjacency List for undirected and weighted graph.
	List<List<NodeInfo>> adjList = findAdjList(vertexCount, edges);
	
	// Creating min distance array to calculate the min distance 
	// bwt source and nodes
	int[] minDistance = new int[vertexCount];
	for(int i = 0; i < vertexCount; i++) minDistance[i] = Integer.MAX_VALUE;
	
	// Min Heap to calculate the min distance edge from current node
	PriorityQueue<NodeInfo> minHeap = 
		new PriorityQueue<>(Comparator.comparing(nodeInfo -> nodeInfo.weight));
	
	// Populating for source
	minDistance[source] = 0;
	// Adding the reachable nodes from source into heap
	for(NodeInfo adjecentEdge : adjList.get(source)) minHeap.add(adjecentEdge);
	
	while(!minHeap.isEmpty()){
		// Get reachable node with minimum edge
		NodeInfo minEdge = minHeap.poll();
		int weight = minEdge.weight;
		int node = minEdge.node;
		
		// If current weight is greater than already available min weight 
		// then skip
		if(weight > minDistance[node]) continue;
		else{
			// If less weight, then save it and add reachable nodes 
			// from current node into the heap
			minDistance[node] = weight;
			for(NodeInfo adjecentEdge : adjList.get(node)) 
				if((minDistance[node] + adjecentEdge.weight) 
							< minDistance[adjecentEdge.node])
					minHeap.add(adjecentEdge);
		}
	}
	
	return minDistance;
}
```


