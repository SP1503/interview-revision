## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351222/assignment/problems/235?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a Binary Tree
- To return
	- same Tree with its next node points to its right nodes
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Every node should be pointing to its right node.
- So at the given node I need to access the right node of it.
- This can be done by Level order traversal
### Optimised Approach:
### Reference:![[WhatsApp Image 2026-02-18 at 9.17.46 AM.jpeg]]

### Code
```Java

// Time Complexity: O(N)
// Space Compelxity: O(N/2)
private TreeNode populateNext(TreeNode root){
	
	TreeNode last = root;
	
	Deque<TreeNode> queue = new ArrayDeque<>();
	
	queue.add(root);
	
	while(!queue.isEmpty()){
		
		TreeNode curr = queue.removeFirst();
		
		if(curr.left != null) queue.addLast(curr.left);
		
		if(curr.right != null) queue.addlast(curr.right);
		
		if(curr == last){
			curr.next = null;
			last = queue.getLast();
		}
		else{
			curr.next = queue.getFirst();
		}
	}
	
	return root;
}

```

