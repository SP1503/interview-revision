## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351241/assignment/problems/214/?navref=cl_pb_nv_tb
## Understanding:
- Given
	- Root of a Binary Tree
- To return:
	- List contains the in order traversal of the binary tree given
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Since Tree is of recursive tree structure, we can use recursion to iterate over the given tree.
- Base Condition:
	- If current == null, No nodes to process return
- Recurrence Relation:
	- For every element
		- Process its left element first 
		- Process current
		- Process right
### Reference:
### Code
```Java

// Brute Force:

// Time Complexity: O(N) iterating every element atleast once
// Space Complexity: O(N) auxillary space

// This will not work for larger inputs as the stack overflow error can occur.
private ArrayList<Integer> findElements(TreeNode root){
	ArrayList<Integer> elements = new ArrayList<>();
	findInorder(root, elements);
}

private void findInorderTraversal(
	TreeNode root, 
	List<Integer> traversal){
	
	//Base Condition
	if(root == null) return;
	
	// Recurrence Relation
	findInorderTraversal(root.left);
	traversal.add(root.val);
	findInorderTraversal(root.right);
}

// Iterative Approach:

// Time Complexity; O(N)
// Space Complexity: O(N) Auxillary space
private ArrayList<Integer> findInorder(TreeNode root){
	
	Stack<TreeNode> stack = new Stack<>();
	TreeNode curr = root;
	
	ArrayList<Integer> inorder = new ArrayList<>();
	
	while(curr != null || !stack.isEmpty()){
		if(curr != null){
			stack.push(curr);
			curr = curr.left;
		}
		else{
			curr = stack.pop();
			inorder.add(curr.val);
			curr = curr.right;
		}
	} 
	
	return inorder;
}

```
