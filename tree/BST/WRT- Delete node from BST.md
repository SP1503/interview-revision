## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351251/assignment/problems/18384?navref=cl_tt_lst_nm
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- If we found our target we need to delete that particular node but that tree should be maintaining the same BST property.
- To achieve this
	- Case 1: if deleting root is a leaf node
		- Since it is a lead node deleting this particular node is not a problem.
	- Case 2: if deleting root having only one child with it
		- Since it have either any one of the children updating that children instead of current deleted node will not cause the BST property issue.
	- Case 3: if deleting root having both child with it.
		- Here we have both children
		- What node we can be replaced with so that the BST property is valid
		- Replacing with preorder successor node will always help as that is the largest element in the left subtree.
		- Once swapped delete the preorder successor from the left subtree
- Complexity:
	- Time Complexity: O(log N)
	- Space Complexity: O(log N)
### Reference:
### Code
```Java

// Complexity:
// Time Complexity: O(log N)
// Space Complexity: O(log N) stack depth space
private TreeNode deleteTarget(TreeNode root, int target){
	
	// Base Condition
	if(root == null) return root;
	else if(root.val == target){
		// Recurrence Relation
		if(root.left == null && root.right == null){
			// Case 1: is Leaf node
			return null;
		}
		else if(root.left == null || root.right == null){
			// Case 2: root having one child that cna be left or right
			if(root.left == null){
				return root.right;
			}
			else return root.left;
		}
		else{
			// Case 3: root having both left and right
			TreeNode preOrderSucc = root.left;
			
			while(preOrderSucc.right != null) preOrderSucc = preOrderSucc.right;
			
			root.val = preOrderSucc.val;
			
			root.left = deleteTarget(root.left, root.val);
			
			return root;
		}
	}
	else if(root.val > target){
		root.left = deleteTarget(root.left, target);
		return root;
	}
	else{
		root.right = deleteTarget(root.right, target);
		return root;
	} 
}
```


