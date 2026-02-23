## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351248/assignment/problems/234?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a binary tree
	- Sum a integer value
- To return:
	- Check if there exists a root to leaf path whose sum == sum
## Input and Output:
## Problem Constraints:
## Approach:
### Optimised Approach:
- We need to check if the leaf node value is equal to sum
### Reference:
### Code
```Java

// Complecity:
// Time Complexity: O(N)
// Space Complexity: O(log N)
private boolean isPathSumPoss(TreeNode root, int sum){
	
	// Base Condition
	if(root == null) return false;
	// Recurrence relation
	else if(root.left == null && root.right == null) return sum == root.val;
	else{
		return isPathSumPoss(root.left, sum - root.val) 
			|| isPathSumPoss(root.right, sum - root.val);
	}
}

```

