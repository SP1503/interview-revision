## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/313322/assignment/problems/4092?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Row wise and column wise sorted matrix
- To return:
	- Find whether the given element is present in the given matrix
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Do linear search in the given array
- Complexity:
	- Time Complexity: O(N * M)
	- Space Complexity: O(1)
### Optimised Approach:
- Based on the increasing and decreasing order format we can reduce the search space.
- How
	- At top left all the elements from left to right is increasing
	- At top right all the elements from right to left is decreasing
	- At bottom left all the elements from left to right is increasing
	- At bottom right all the elements from right to left is decreasing.
- We we choose to search from top right
	- Then, top to bottom in increasing
	- Right to left is decreasing
- This condition helps us to reduce the space
### Reference:![[WhatsApp Image 2026-02-21 at 12.38.42 PM.jpeg]]
![[WhatsApp Image 2026-02-21 at 12.38.42 PM (1).jpeg]]
### Code
```Java

// Time Complexity: O(m * n log m * n)
// Space Compelxity: O(1)
private boolean isPresent(int[][] mat, int target){
	
	int m = mat.length;
	int n - mat[0].length;
	int i = 0;
	int j = n - 1;
	
	while(i < m && j >= 0){
		if(mat[i][j] == target) return true;
		else if(mat[i][j] < target) i++;
		else j--;
	}
	
	return false;
}

```

