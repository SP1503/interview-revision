## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351248/assignment/problems/224/submissions
## Understanding:
- Given:
	- Inorder traversal of Binary Tree
	- Post order traversal of Binary Tree
- To return:
	- Return the constructed Tree with the traversals present
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- In Order = left root right
- Post Order = left right root
- Always the post order end is the root node 
- Find the index of the root element in inorder
- find the left tree element count
- call recursively to find the left subtree
- call recursively to find the right subtree
### Reference:![[WhatsApp Image 2026-02-18 at 9.17.47 AM.jpeg]]

### Code
```Java

// Time Complexity: O(N)
// Space Complexity: O(1) 
// If we use hash map to get the index of the element in O(1) then space becomes O(N)
private TreeNode constructTree(
	int[] inOrder, 
	int[] postOrder,
	int inStart,
	int inEnd,
	int postStart,
	int postEnd){
	
	//Base Condition
	if(inStart > inEnd) return null;
	
	// Recurrence Relation
	int rootEle = postOrder[postEnd];
	TreeNode root = new TreeNode(rootEle);
	
	int rootIndex = findIndex(inorder, inStart, inEnd, rootEle);
	int leftEleCnt = rootIndex - inStart;
	
	root.left = constructTree(inOrder, postOrder, 
		inStart, 
		rootIndex - 1, 
		postStart, postStart + leftEleCnt - 1);
	
	root.right = constructTree(inOrder, postOrder, 
		rootIndex + 1, 
		inEnd, 
		postStart + leftEleCnt, postEnd - 1);
		
	return root;	
}

private int findIndex(int[] arr, int start, int end, int target){
	for(int i = start; i <= end; i++){
		if(arr[i] == target) return i;
	}
	return -1;
}

```

