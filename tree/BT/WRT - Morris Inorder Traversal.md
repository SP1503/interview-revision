## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351254/assignment/problems/35988?navref=cl_tt_lst_nm
## Understanding:
- Given 
	- Root of Binary Tree
- To return
	- In-order traversal of binary tree without using extra space or even stack space.
## Input and Output:
## Problem Constraints:
## Approach:
###  Algorithm Implementation:
- Why we need a stack to process in-order traversal
	- Because, we need to store the reference of the root after processing its left node to process the right node of it.
	- Hence we need a stack that store the current root and creating new stack level to process it left when it is done we are coming back to root to process the right.
- Hence we need a space to store.
	- let say, in the given tree can we store these root reference
	- Where we can store,
		- When we will process the root, After processing the inorder predecessor element in the left subtree we will process the root.
		- in order predecessor node of root always have its right as null.
		- Can we use this space ?
- When visiting a node,
	- Check if it have a inorder predecessor
	- If yes, 
		- get it
		- store the current node reference in right pointer
		- Now process the left subtree.
	- If the inorder predecessor already having its right node points to current 
		- we can remove that link
		- process the current element
		- proceed to right subtree.
	- If No left subtree for current node
		- Process current node
		- proceed to its right
	- It is guaranteed that all the right pointers is populated either to its right subtree or the root. 
	- Hence if we found a node that have curr.right == null 
		- then this is the last node that have no other right node to point
- Complexity:
	- Time Complexity:
		- Every node processed thrice,
			- Once to create a Node
			- Once to process the node
			- Once to remove the node.
		- Hence O(3N) = O(N)
	- Space Complexity: O(1)

### Reference:![[WhatsApp Image 2026-02-19 at 8.19.08 AM.jpeg]]
### Code
```Java
// Resuce the rcursive space using Morris InOrder Traversal

private int count = 0;
private int ans = Integer.MIN_VALUE;


// Time Compelxity: O(3N) 
// as we need to iterate the node twice once for creating link  and one for deleteing link

// Space Complexity: O(1)

private int inorder(TreeNode root, int k){
	TreeNode curr = root;
	
	while(curr != null){
		
		// Having left create or remove link to current
		if(curr.left != null){
			TreeNode predecssor = curr.left;
			while(predecssor.right != null && predecssor.right != curr)
				predecssor = predecssor.right;
			
			if(predecssor.right == null){
				// Creating new link to the current node
				predecssor.right = curr;
				curr = curr.left;
			}
			else{
				// Removing the link to the current node
				predecssor.right = null;
				System.out.println(curr.val);
				curr = curr.right;
			}
		}
		else{
			System.out.println(curr.val);
			curr = curr.right;
		}
	}
	
	return ans;
}

```

