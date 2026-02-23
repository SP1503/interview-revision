## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351261/assignment/problems/125661?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- A grid of size M and N
- To return:
	- Find the poss paths available in the given grid to reach from top to bottom
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Here we need to generate all the paths.
- Hence we need to d Backtracking
- At every moment we have two decisions
	- Go Down
	- GO right
- Base case
	- If reached the bottom then save the path
	- If the path is invalid discard the path
### Reference:![[WhatsApp Image 2026-02-20 at 7.34.01 AM.jpeg]]
### Code
```Java

private ArrayList<ArrayList<String>> possPaths = new ArrayList<>();

// Time Complexity: No of function calls * TC of each call
// No of function calls= at every point I am taking 2 decisions, Down or right
// so the Gp grows like 1 + 2 + 4 + 8 + .. 2^N = 2^N
// Total TC: O(2^(M + N)) * O(1) = O(2^(M + N))
// Space Compelxity: O(M + N)
private void findPossPaths(
	ArrayList<ArrayList<Integer>> grid, 
	int row, 
	int col,
	StringBuilder path){
		
		// Base Condition
		if(row >= grid.size() || col >= grid.get(0).size()) return;
		else if(row == grid.size() - 1 && col == grid.get(0).size() - 1){
			possPaths.add(path.toString());
			return;
		}
		else{
			// Recurrence Relation
			// At any moment I have two decisions go down or go right
			
			// Go Down
			path.append('D');
			findPosspaths(grid, row + 1, col, new StringBuilder(path));
			
			// Backtracking undoing the changes
			path.deleteCharAt(path.length() - 1);
			
			// Go Right
			path.append('R');
			findPosspaths(grid, row, col + 1, new StringBuilder(path));
			
			// Backtracking undoing the changes
			path.deleteCharAt(path.length() - 1);
		}
	}

```
