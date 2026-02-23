## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351254/assignment/problems/335?navref=cl_tt_lst_nm
## Understanding:
- Given 
	- Root of a BST
- Return:
	- Kth smallest element in the BST
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- We know the in-order traversal of a BST is always a sorted array.
- Iterate the BST in in-order fashion to get the sorted array.
- Now return the k - 1 th element from the sorted array.
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(1)
### Optimised Approach:
- We can optimise the space by using the count variable in the inorder traversal.
- For every node that we are visiting increment the count
	- If count == k then this is the node that we are searching for.
- Complexity: 
	- Time Complexity: O(N)
	- Space Complexity: O(H) height of the tree for traversal
### Optimised Space:
- Here further more we can optimise the space to O(1) using Morris in-order traversal.
- We need O(H) space if we are using recursion instead can we go for any iterative solution.
### Reference:![[WhatsApp Image 2026-02-19 at 8.19.08 AM (1).jpeg]]
![[WhatsApp Image 2026-02-19 at 8.19.10 AM.jpeg]]
![[WhatsApp Image 2026-02-19 at 8.19.10 AM (1).jpeg]]

### Code
```Java


// Time Complexity: O(N)
// Space COmplexity: O(N) list space
private int findKthSmallest(TreeNode root, int k){
	List<Integer> inorder = new ArrayList<>();
	inorder(root, inorder);
	return inorder.get(k - 1);
}

private void inorder(TreeNode root, List<Integer> inorder){
	// Base Condition
	if(root == null) return;
	
	// Recurrence Relation
	inorder(root.left, inorder);
	inorder.add(root.val);
	inorder(root.right, inorder);
}

// Optimised Space approach
private int count = 0;
private int ans = Integer.MIN_VALUE;

// Time Compelxity: O(N)
// Space Compelxity: O(H) recursive stack space
private void inorder(TreeNode root, int k){
	// Base Condition
	if(root == null || ans != Integer.MIN_VALUE) return;
	
	// Recurrence Relation
	inorder(root.left, k);
	this.count++;
	if(count == k) ans = root.val;
	inorder(root.right, k);
} 


// Resuce the rcursive space using Morris InOrder Traversal

private int count = 0;
private int ans = Integer.MIN_VALUE;


// Time Compelxity: O(2N) 
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
				count++;
				if(count == k) ans = curr.val;
				curr = curr.right;
			}
		}
		else{
			count++;
			if(count == k) ans = curr.val;
			curr = curr.right;
		}
	}
	
	return ans;
}

```
