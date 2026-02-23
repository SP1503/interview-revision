## Problem link:
- https://leetcode.com/problems/minimum-number-of-days-to-disconnect-island/description/
## Understanding:
- Given 
	- grid of size M * N represents a land and water.
		- 0 represents water
		- 1 represents land
- To return:
	- The grid should not contain only 1 island in it, We need to split it 
	- Minimum number of days required to split the 1 island into two
	- Splitting means converting 1 land into water in 1 day
### Optimised Approach:
- The max values at any grid is two because, Here we focus only on dividing a single island into 2.
- Here why the max day is 2 because if a island is connected it should be in form of a square or rectangle.
- Every square or rectangle will have a diagonal of length 2 which can be used to cut the square or rectangle to divide into two.
- Hence check if the island is already separated if yes, return 0
- Convert every 1 into 0 only by one and check number of islands. If n == 0 or 2 return 1
- Else return 2.
### Reference:![[WhatsApp Image 2026-02-23 at 10.45.53 AM.jpeg]]
![[WhatsApp Image 2026-02-23 at 10.45.35 AM.jpeg]]
![[WhatsApp Image 2026-02-23 at 10.45.52 AM.jpeg]]
### Code
```Java
class Solution {
	private int[] row = {-1, 1, 0, 0};
	private int[] col = {0, 0, -1, 1};
	
	public int minDays(int[][] grid) {
		return findMinNoOfDays(grid);
	}
	  
	
	// Time Complexity: O(N * M) + O(N * M * N * M)
	// Space Complexity: O(N * M)
	private int findMinNoOfDays(int[][] grid){
		// The max values at any gris two because, 
		//Here we focus only on dividing a single island into 2,
		// Here why the max dayis 2 because if a island is connected 
		// it should be in form of a square or rectangle.
		// Every square or rectangle will have a diagonal of length 2 
		// which can be used to cut the square or rectangle to divide into two.
		int noOfIslands = findNoOfIslands(grid);
		if(noOfIslands != 1) return 0;
		else{
			int m = grid.length;
			int n = grid[0].length;
			for(int r = 0; r < m; r++){
				for(int c = 0; c < n; c++){
					if(grid[r][c] == 1){
						grid[r][c] = 0;
						noOfIslands = findNoOfIslands(grid);
						if(noOfIslands != 1) return 1;
						grid[r][c] = 1;
					}
				}
			}
			return 2;
		}
	}
	  
	
	private int findNoOfIslands(int[][] grid){
		int count = 0;
		int m = grid.length;
		int n = grid[0].length;
		boolean[][] visited = new boolean[m][n];
		for(int r = 0; r < m; r++){
			for(int c = 0; c < n; c++){
				if(!visited[r][c] && grid[r][c] == 1){
					dfs(grid, r, c, visited);
					count++;
				}
			}
		}
		return count;
	}
	
	private void dfs(int[][] grid, int r, int c, boolean[][] visited){
		int m = grid.length;
		int n = grid[0].length;
		// Base Condition
		if(r < 0
		|| r >= m
		|| c < 0
		|| c >= n
		|| visited[r][c]) return;
		
		// Recurrence relation
		visited[r][c] = true;
		for(int dir = 0; dir < 4; dir++){
			int nextR = r + row[dir];
			int nextC = c + col[dir];
			if(nextR >= 0
			&& nextR < m
			&& nextC >= 0
			&& nextC < n
			&& !visited[nextR][nextC]
			&& grid[r][c] == 1){
				dfs(grid, nextR, nextC, visited);
			}
		}
	}
}
```

