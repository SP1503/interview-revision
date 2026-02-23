## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351248/assignment/problems/225?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a binary tree
	- If the BT is height balanced if the left height and right height of any node differs by at-most 1 .
- To return:
	- Check if the given binary tree is height balanced
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- To know whether the BT is balanced
	- At every node we need a left subtree height and right subtree height.
	- We need to check whether it is valid.
### Reference:
### Code
```Java

// Time Complexity: O(N * N)
// Space Complexity: O(log N)
// Brute Force:
private boolean isBalanced(TreeNode root){
	
	// Base Condition
	if(root == null) return true;
	
	// Recurrence Relation
	if(isBalanced(root.left) && isBalanced(root.right)){
		int leftHeight = findHeight(root.left);
		int rightHeight = findHeight(root.right);
		
		return Math.abs(leftHeight - rightHeight) <= 1;
	}
	else return false;
	
}

private int findHeight(TreeNode root){
	// Base Condition
	if(root == null) return 0;
	
	// Recurrence Relation
	return Math.max(findHeight(root.left), findHeight(root.right)) + 1;
}

// Optimised
// Time Complexity: O(N)
// Space Complexity: O(N) if tree is skewed
private boolean isValid = true;

private int isBalanced(TreeNode root){
	findHeight(root);
	return isValid;
}

private int findHeight(TreeNode root){
	// Base Condition
	if(root == null) return 0;
	
	// Recurrence Relation
	int leftHeight = findHeight(root.left);
	int rightHeight = findHeight(root.right);
	
	if(Math.abs(leftHeight - rightHeight) > 1) isValid = false;
	
	return Math.max(leftHeight, rightHeight) + 1;
}

```


