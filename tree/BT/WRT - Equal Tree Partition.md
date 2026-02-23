## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351248/assignment/problems/4859/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- Root of a binary tree
- To return 
	- Check whether it is possible to partition the tree into two equal half so that both of its sum is equal.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- fact 1: The total sum of the tree should be even so that it can be divided into 2.
- fact 2: We need to delete exactly one edge between two nodes to partition the given tree into two. making partition so that the Tree t1 having x sum that is equal to x sum of tree t2.
### Reference:
### Code
```Java
private boolean isPartitionPoss(TreeNode root){
	long totalSum = findSum(root);
	
	if(totalSum % 2 == 1) return false;
	
	return isTreeWithSumPoss(root, totalSum/2);
}

private boolean isTreeWithSumPoss(TreeNode root, long target){
	
	// Base Condition
	if(root == null) return false;
	
	// Recurrence Relation
	if(findSum(root) == target) return true;
	else return isTreeWithSumPoss(root.left, target) 
		|| isTreeWithSumPoss(root.right, target);
	
}

private long findSum(TreeNode root){
	
	// Base Condition
	if(root == null) return 0;
	
	// Recurrence Relation
	long leftSum = findSum(root.left);
	long rightSum = findSum(root.right);
	return leftSum + rightSum + root;
}

```



