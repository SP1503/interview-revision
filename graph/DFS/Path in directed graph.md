#revision-1 #revision-2 #revision-3 #CanBeImplementedWithoutRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351258/assignment/problems/9359?navref=cl_tt_lst_nm
## Understanding:
- Given a Directed Graph
- A nodes
- M edges
- Find whether the path exists between 1 and A.
- If yes return 1 or else return 0
## Input and Output:
![[Screenshot 2026-01-17 at 6.50.44 PM.png]]
![[Screenshot 2026-01-17 at 6.51.22 PM.png]]
![[Screenshot 2026-01-17 at 6.51.35 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-17 at 6.51.49 PM.png]]
## Approach:
### Strategy: 
- DFS - Moving from current neighbour in depth wise. Helps in finding cycle detection.
### Brute Force:
- We need find whether there is any path exists between 1 and A.
- Hence need to check what are all the nodes that are connected with 1 and proceed.
- Here we need **What are all the nodes that are connected with 1 ?** Hence Adjacency List.
- Graph is **Directed**
- Doing DFS will get whether the path exists
- **Complexity:**
	- **Time Complexity**: O(V + E), At worst case we will iterate all the space that we allocated in the adjacency list. 
	- **Space Complexity** : O(V + E) for adjacency list + O(V) for visited array.
### Optimised Approach:
### Reference:
![[WhatsApp Image 2026-01-17 at 7.06.13 PM.jpeg]]
### Code
```Java
public int searchPaths(int vertexCount, ArrayList<ArrayList<Integer>> edges) {
	boolean[] visited = new boolean[vertexCount + 1];
	List<List<Integer>> adjList = getAdjList(vertexCount, edges);
	return isPathExists(1, visited, adjList) ? 1 : 0;
}

private List<List<Integer>> getAdjList(
	int vertexCount, 
	ArrayList<ArrayList<Integer>> B){
	List<List<Integer>> adjList = new ArrayList<>();
	
	for(int i = 0; i <= vertexCount; i++) adjList.add(new ArrayList<>());
	
	for(ArrayList<Integer> edge : B){
		adjList.get(edge.get(0)).add(edge.get(1));
	}
	
	return adjList;
}

  

private boolean isPathExists(
	int vertex, 
	boolean[] visited, 
	List<List<Integer>>adjList){
	
	if(visited[vertex]) return false;
	
	if(vertex == visited.length - 1) return true;
	
	visited[vertex] = true;
	
	for(Integer neighbour: adjList.get(vertex)){
		if(!visited[neighbour]){
			boolean isPathAvail = isPathExists(neighbour, visited, adjList);
			if(isPathAvail) return true;
		}
	}
	
	return false;
}
```

