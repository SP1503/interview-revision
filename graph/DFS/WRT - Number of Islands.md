#revision-1 #revision-2 #CanBeImplementedWithoutRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351264/assignment/problems/4702/submissions
## Understanding:
- Given a 2D matrix of size N * M.
- The elements of matrix is either 0 or 1.
- 1 represents land
- 0 represents water.
- The Island is considered if 2 lands are connected in Up, Down, left, Right, UpLeftDiagonal, UpRightDiagonal, DownLeftDiagonal, DownRightDiagonal.
- Find the total number of islands present in the given grid.
## Input and Output:
![[Screenshot 2026-01-25 at 12.16.13 PM.png]]
![[Screenshot 2026-01-25 at 12.16.25 PM.png]]
![[Screenshot 2026-01-25 at 12.16.40 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-25 at 12.16.51 PM.png]]
## Approach:
### Brute Force:
- We can connect the lands are form a single Island Using DFS (Connect the neighbours).
- Iterate the given grid to find what can be a potential starting point of a Island.
- If yes start from that land and connect the lands adjacent to it to form the island.
- Mark the connected lands as visited.
- Then continue the iteration to go through the non visited nodes like how we will go through the nodes that are not connected in a given graph.
- **Complexity**:
	- **Time Complexity:** O(V + E) for DFS. V = N * M , E = 8 * N * M for every node there is 8 edges. So the time complexity is O(N * M + 8 * N * M)
	- **Space Complexity**: O(V) = O(N * M)
### Reference:![[WhatsApp Image 2026-01-25 at 12.29.41 PM.jpeg]]
### Code
```Java
private int[] row = {-1, 1, 0, 0, -1, -1, 1, 1};
private int[] col = {0, 0, -1, 1, -1, 1, -1, 1};
  

private int findTotalIslandCount(ArrayList<ArrayList<Integer>> grid){
	int count = 0;
	for(int i = 0; i < grid.size(); i++){
		for(int j = 0; j < grid.get(i).size(); j++){
			if(grid.get(i).get(j) == 1){
				connectLands(grid, i, j);
				count++;
			}
		}
	}
	return count;
}

  

private void connectLands(ArrayList<ArrayList<Integer>> grid, int r, int c){
	// Base Condition
	if(grid.get(r).get(c) != 1) return;
	
	// Recurrence Relation
	grid.get(r).set(c, 2);
	for(int dir = 0; dir < 8; dir++){
		int nextR = r + row[dir];
		int nextC = c + col[dir];
		if(nextR >= 0
		&& nextR < grid.size()
		&& nextC >= 0
		&& nextC < grid.get(0).size()
		&& grid.get(nextR).get(nextC) == 1){
			connectLands(grid, nextR, nextC);
		}
	}
}
```
