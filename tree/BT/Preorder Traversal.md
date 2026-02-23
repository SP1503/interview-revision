## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351241/assignment/problems/222?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a binary tree
- To return:
	- return list contains the preorder traversal of the binary tree
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Since the tree looks likes recursive backtracking diagram, We can use recursion to make this happen.
- Base Condition:
	- if root == null return null
- Recurrence Relation:
	- process root
	- process left node
	- process right node
### Optimised Approach:
### Reference:
### Code
```Java

private ArrayList<Integer> preorderTraversal(TreeNode root){
	ArrayList<Integer> preorder = new ArrayList<>();
	findPreorder(root, preorder);
}

// Brute Force
// Time Complexity: O(N)
// Space Complexity: O(1)
private void findPreorder(TreeNode root, ArrayList<Integer> preorder){
	// Base Condition
	if(root == null) return;
	
	// Recurrence Relation
	preorder.add(root.val);
	findPreorder(root.left, preorder);
	findPreorder(root.right, preorder);
}

private ArrayList<Integer> findPreOrder(TreeNode root){
	
	Stack<TreeNode> stack = new Stack<>();
	
	ArrayList<Integer> preorder = new ArrayList<>();
	
	TreeNode curr = root;
	
	while(curr != null || !stack.isEmpty()){
		if(curr != null){
			preorder.add(curr.val);
			stack.push(curr);
			curr = curr.left;
		}
		else{
			curr = stack.pop();
			curr = curr.right;
		}
	}
	
	return preorder;
}

