## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private int[] dRow = {-1, 1, 0 ,0};
private int[] dCol = {0, 0, -1, 1};
private int minSteps = Integer.MAX_VALUE;

// Time Complexity: At every point we have 4 options, Hence tc: O(4 ^ n * m)
// Space Complexity: O(N * M)
private void findShortestPath(
ArrayList<ArrayList<Integer>> grid,
int row,
int col,
int m, 
int n,
int destRow,
int destCol,
int stepsTak
){
	// Base Condition
	if(row >= m || col >= n) return;
	else if(grid.get(row).get(col) != 1) return;
	else if(row == destRow && col == destCol){
		minSteps = Integer.min(minSteps, stepsTak);
		return;
	}
	else{
		// Recurrence Relation
		// We can take 4 decisions go top, down, left, right
		
		// Marking current as visited
		int currVal = grid.get(row).get(col);
		grid.get(row).set(col, 2);
		
		for(int i = 0; i < 4; i++){
			int nextRow = row + dRow[i];
			int nextCol = col + dCol[i];
			
			if(nextRow >= 0 
			&& nextRow < m 
			&& nextCol >= 0 
			&& nextCol < n
			&& grid.get(nextRow).get(nextCol) == 1){
				findShortestPath(grid, nextRow, nextCol, m, n, stepsTak + 1);
			}
		}
		
		// Backtracking undo changes
		grid.get(row).set(col, currVal);
	}	
}

```
