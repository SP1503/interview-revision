## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351251/assignment/problems/226?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Sorted Array
- To return:
	- Construct Binary Search Tree using the sorted Array
## Input and Output:
## Problem Constraints:
## Approach:

### Optimised Approach:
- Here BST means all the left node will be <= root.val 
- All the right nodes will be >= root.val
- Hence we can say the mid element of the sorted array is a root of the binary tree
- Construct left subtree using 0 to mid - 1 elements 
- Construct right subtree using mid + 1 to n elements.
### Reference:
### Code
```Java

// Time Complexity: O(N)
// Space Compelxity: O(N/2);
private TreeNode constructBST(int[] ele, int start, int end){

	// Find root element
	int mid = start + (end - start) / 2;
	int midEle = ele[mid];
	
	TreeNode root = new TreeNode(midEle);
	
	root.left = constructBST(ele, start, mid - 1);
	root.right = constructBST(ele, mid + 1, end);
	
	return root;
}

```


