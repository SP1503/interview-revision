## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351251/assignment/problems/221?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a Binary Search Tree
- To return:
	- Check if the given binary search tree is BST
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Create array by doing in-order traversal of BST.
- The array created needs to be sorted format as the in-order traversal of BST is sorted array.
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(N)
### Optimised Approach:
- Why we need to create a array and check if that ordered in sorted instead we can check using recursion.
### Reference:
### Code
```Java

private static int prev = -1;

private boolean checkBST(TreeNode root){
	// Base condition
	if(root == null) return false;
	
	// Recurrence Relation
	if(!checkBST(root.left, prev)) return false;
	
	if(prev >= root.val) return false;
	
	prev = root.val;
	
	return checkBST(root.right); 
}
```



