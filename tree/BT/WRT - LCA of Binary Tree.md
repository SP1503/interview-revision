## Problem link
- https://www.scaler.com/academy/mentee-dashboard/class/351254/assignment/problems/218/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- Root of a binary tree
	- Ancestor A
	- Ancestor B
- To return:
	- Find the LCA of the given ancestors in the binary tree
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- A LCA of two ancestors will be present only if two ancestors present in the given binary tree.
- Do first checking both ancestors availability needs to be done
- Now we know both ancestors are available.
	- While iterating if we get any one ancestor first that could be LCA for the given two ancestors.
	- Other wise if two ancestors return a new LCA then current grant parent is the ancestor.
### Reference:
### Code
```Java

// Time Complexity: O(N)
// Space Complexity: O(log N)
public int lca(TreeNode A, int B, int C) {
	boolean isAAvail = isNodeAvail(A, B);
	boolean isBAvail = isNodeAvail(A, C);
	return isAAvail && isBAvail ? findLCA(A, B, C) : -1;
}

private boolean isNodeAvail(TreeNode root, int tar){
	// Base Condition
	if(root == null) return false;
	// Recurrence Relation
	if(root.val == tar) return true;
	else return isNodeAvail(root.left, tar) || isNodeAvail(root.right, tar);
}

private int findLCA(TreeNode root, int B, int C){
	// Base Condition
	if(root == null) return -1;
	// Recurrence Relation
	if(root.val == B || root.val == C) return root.val;
	int leftLCA = findLCA(root.left, B, C);
	int rightLCA = findLCA(root.right, B, C);
	if(leftLCA != -1 && rightLCA != -1) return root.val;
	else if(leftLCA != -1) return leftLCA;
	else return rightLCA;
}

```
