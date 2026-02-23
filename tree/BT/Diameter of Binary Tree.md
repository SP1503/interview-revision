## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351222/assignment/problems/9256/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- root of a Binary tree
- To return:
	- Find the depth of the binary tree
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Depth of a tree = Number of edges in (max height of left subtree +max height of right subtree + 1)
- We know in N nodes we have N - 1 edges.
- So depth of a tree = Number of edges in (max height of left subtree + max height of right subtree)
### Reference:![[WhatsApp Image 2026-02-18 at 11.08.08 AM.jpeg]]
### Code
```Java

private int maxDepth = 0;

// Time Complexity: O(N)
// Space Complexity: O(N) if tree is skewed.
private int findDepth(TreeNode root){
	findheight(root);
	return maxDepth;
}

private int findHeight(TreeNode root){
	// Base Condition
	if(root == null) return 0;
	
	// Recurrence Relation
	int leftHeight = findHeight(root.left);
	int rightHeight = findHeight(root.right);
	
	maxDepth = Math.max(maxDepth, leftHeight + rightHeight);
	
	return Math.max(leftHeight, rightHeight) + 1;
}
```