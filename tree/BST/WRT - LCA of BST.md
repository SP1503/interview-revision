## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

// Time Complexity: O(log N) either going left side or rightside not going in both sides
// Space Compelxity: O(H)
private TreeNode findLCA(TreeNode root, int A1, int A2){
	
	TreeNode curr = root;
	
	while(curr != null){
		if(curr.data == A1 || curr.data == A2) return curr;
		else if(A1 < curr.data && A2 < curr.data) curr = curr.left;
		else if(A1 > curr.date && A2 > curr.data) curr = curr.right;
		else return curr;
	}
}

```
