#revision-1 #revision-2 #revision-3 #NeedRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351258/assignment/problems/9327/?navref=cl_pb_nv_tb
## Understanding:
- Given Directed graph
- Having A nodes and M edges
- Whether there is a Cycle in the given graph.
- Is yes return 1 or else return 0. 
## Input and Output:
![[Screenshot 2026-01-17 at 7.08.52 PM.png]]
![[Screenshot 2026-01-17 at 7.09.05 PM.png]]
![[Screenshot 2026-01-17 at 7.09.22 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-17 at 7.09.41 PM.png]]
## Approach:
### Strategy: 
- DFS - Moving from current neighbour in depth wise. Helps in finding cycle detection.
- Move from current to neighbour, then neighbour's neighbour.
### Brute Force:
- Here we need to find whether there is a cycle exists in the given graph.
- What is a cycle? If a node is visited again in the same path then it forms a cycle.
- Can we use this visited array ? No, why because the visited array element can become true if the node is visited by different component of same graph. Hence we need to keep track of the path that we are proceeding with.
- Complexity:
	- Time Complexity: O(V + E)
	- Space Complexity: O(V + E) for adjacency list, O(N) for visited array, O(N) for path
### Optimised Approach:
### Reference:![[WhatsApp Image 2026-01-17 at 7.21.54 PM.jpeg]]
### Code
```Java
private boolean isCyclePresentImpl(
int vCount, 
ArrayList<ArrayList<Integer>> edges){
	
	boolean[] visited = new boolean[vCount + 1];
	
	boolean[] path = new boolean[vCount + 1];
	
	ArrayList<ArrayList<Integer>> adjList = findAdjList(vCount, edges);
	
	for(int vertex = 1; vertex <= vCount; vertex++){
		if(!visited[vertex]){
			if(isCyclePresent(vertex, vCount, adjList, visited, path)) 
				return true;
		}
	}
	
	return false;
}

private boolean isCyclePresent(
int vertex, 
int vCount, 
ArrayList<ArrayList<Integer>> adjList, 
boolean[] visited, 
boolean[] path){
	
	// Base Condition
	if(visited[vertex]) return false;
	
	visited[vertex] = true;
	
	path[vertex] = true;
	
	// Recurrence Relation
	for(Integer neighbour : adjList.get(vertex)){
		if(path[neighbour]) return true;
		
		if(!visited[neighbour]){
			if(isCyclePresent(neighbour, vCount, adjList, visited, path)) 
				return true;
		}
	}
	
	path[vertex] = false;
	
	return false;
}

private ArrayList<ArrayList<Integer>> findAdjList(
int vCount, 
ArrayList<ArrayList<Integer>> edges){
	ArrayList<ArrayList<Integer>> adjList = new ArrayList<>();
	
	for(int i = 0; i <= vCount; i++) adjList.add(new ArrayList<>());
	
	for(ArrayList<Integer> edge : edges){
		int src = edge.get(0);
		int dest = edge.get(1);
		adjList.get(src).add(dest);
	}
	
	return adjList;
}
```

