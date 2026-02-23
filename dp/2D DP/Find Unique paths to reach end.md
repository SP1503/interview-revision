#revision-1 #revision-2  #revision-3 #revision-4 #CanBeImplementedWithoutRevisit  
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/351257/assignment/problems/8?navref=cl_tt_lst_nm
## Understanding:
- Given a grid of size n * m
- Find total number of ways to reach from start(0, 0) to end(n, m) in the given grid.
## Input and Output:
![[Screenshot 2026-01-07 at 12.12.38 PM.png]]
![[Screenshot 2026-01-07 at 12.13.05 PM.png]]
![[Screenshot 2026-01-07 at 12.13.21 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-07 at 12.13.33 PM.png]]
## Approach:
### Brute Force:
- **Idea:** Need to find all the ways. Hence recursion can be used.
- **Recurrence relation:** F(i, j) = at every step we can go down or we can go right. So F(i, j) = F(i + 1, j) + F(i , j + 1)
- **Base Condition:** if i == n - 1 AND j == m -1, then we reached end. return 0
- If i == n - 1 then we can go only right
- If j == m - 1 then we can go only down.
- **Time Complexity:** O(2 ^ n * m)
- **Space Complexity:** O(n * m)
### Optimised Approach:
- The recursion tree having overlapping sub problems and optimal substructure.
- Applying top down approach. Caching the values
- **Time Complexity:** O(n * m)
- **Space Complexity:** O(n * m)

### Optimised Approach:
- Applying Bottom Up:
- Forced Order: F(i, j) = F(i + 1, j) + F(i , j + 1)
- So i depends on i + 1, j depends on j + 1
- So the iteration should be in reverse order.
- **Time Complexity:** O(n * m)
- **Space Complexity:** O(n * m)
### Reference:
### Code

```Java

Recursion:

private int findPathCount(ArrayList<ArrayList<Integer>> grid, int r, int c){
	if(r == grid.size() - 1 && c == grid.get(0).size() - 1) 
		return grid.get(r).get(c) == 1 ? 0 : 1;
	else if(r >= grid.size() || c >= grid.get(0).size()) return 0;
	else if(grid.get(r).get(c) == 1) return 0;
	else{
		int decisionDown = findPathCount(grid, r + 1, c);
		int decisionRight = findPathCount(grid, r, c + 1);
		return decisionDown + decisionRight;
	}
}


Bottom Up:  

private int findPathCountIterative(ArrayList<ArrayList<Integer>> grid){
	int n = grid.size();
	int m = grid.get(0).size();
	int[][] pathCount = new int[n][m];
	for(int r = n - 1; r >= 0; r--){
		for(int c = m - 1; c >= 0; c--){
			if(r == n - 1 && c == m - 1) 
				pathCount[r][c] = grid.get(r).get(c) == 1 ? 0 : 1;
			else if(grid.get(r).get(c) == 1) pathCount[r][c] = 0;
			else{
				int decisionDown = 0;
				if(r + 1 < n){
					decisionDown = pathCount[r + 1][c];
				}
				
				int decisionRight = 0;
				if(c + 1 < m){
					decisionRight= pathCount[r][c + 1];
				}
				pathCount[r][c] = decisionDown + decisionRight;
			}
		}
	}
	return pathCount[0][0];
}
```


