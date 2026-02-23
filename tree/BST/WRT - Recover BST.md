## Problem link:
- https://leetcode.com/problems/recover-binary-search-tree/
## Understanding:
- Given 
	- root of BST with exactly two nodes swapped
- To return:
	- Fix the swap and return proper BST
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Here we can do the in order traversal of the given tree
- This should  results in the sorted array with exactly two nodes not present in its respective position.
- Sort the given array
- Replace the nodes  with sorted array values in inorder traversal
- Complexity:
	- Time Complexity: O(N) + O(N log N) + O(N)
	- Space Complexity: O(N)
### Optimised Approach:
- In the sorted array we can see two violations
	- largest element comes before smallest
	- Smaller element comes after largest
- But In sorted array
	- larger element comes after smaller
	- Smaller element comes before largest
- With this we can find the nodes that needs to be swapped.
- largest element comes before smallest -> prev.val > current.val
- Smaller element comes after largest -> prevVal. > current.val
- Iterate and find the nodes that violates these condition
- Swap two nodes at the last
- Complexity: 
	- Time Complexity: O(N)
	- Space Complexity: O(1)
### Reference:![[WhatsApp Image 2026-02-19 at 9.27.32 AM.jpeg]]
![[WhatsApp Image 2026-02-19 at 9.27.34 AM.jpeg]]
### Code
```Java

private TreeNode prev = null;
private TreeNode first = null;
private TreeNode second = null;

// Time Complexity: O(N) iterating through every node
// Space Complexity: O(H) Stack space
public void recoverTree(TreeNode root) {
	inorder(root);
	swapValue(first, second);
}

private void swapValue(TreeNode a, TreeNode b){
	int temp = a.val;
	a.val = b.val;
	b.val = temp;
}

private void inorder(TreeNode root){
	// Base Condition
	if(root == null) return;
	// Recurrence Relation
	inorder(root.left);
	if(prev != null && prev.val > root.val){
		if(first == null){
			first = prev;
			second = root;
		}
		else second = root;
	}
	prev = root;
	inorder(root.right);
}
```
