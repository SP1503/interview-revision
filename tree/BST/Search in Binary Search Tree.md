## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351251/assignment/problems/35476?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a Binary Search Tree
- To return:
	- Return true or false based on the presence of element in the given BST
## Input and Output:
## Problem Constraints:
## Approach:
### Optimised Approach:
- 
### Reference:
### Code
```Java

// Time Complexity: O(log N)
// Space Complexity: O(log N)
private boolean isValPresent(TreeNode root, int target){
	
	// Base Condition
	if(root == null) return false;
	
	// Recurrence Relation
	if(root.val == target) return true;
	
	if(target <= root.val) return isValPresent(root.left, target);
	else if(target >= root.val) return isValPresent(root.right, target);
	else return false;
}

```

