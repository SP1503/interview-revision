## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351241/assignment/problems/5714?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a binary tree
- To return:
	- Find the elements that are visible from right of the binary tree
## Input and Output:
## Problem Constraints:
## Approach:
### Optimised Approach:
- Right view is nothing but last element in every level of the binary tree
- Hence we can make a level order traversal.
- In every level we can store only the last element of the level.
### Reference:
### Code:
```Java

// Time Complexity: O(N) 
// Space Complexity: O(N)
private ArrayList<Integer> findLevelOrder(TreeNode root){

	Deque<TreeNode> queue = new ArrayDeque<>();
	ArrayList<Integer> rightView = new ArrayList<>();
	
	TreeNode last = root;
	
	queue.addLast(root);
	
	while(!queue.isEmpty()){
		TreeNode curr = queue.removeFirst();
		if(curr.left != null) queue.addLast(curr.left);
		if(curr.right != null) queue.addLast(curr.right);
		if(curr == last){
			rightView.add(curr.val);
			last = queue.isEmpty() ? null : queue.getLast();
		}
	}

	return rightView;
}
```


