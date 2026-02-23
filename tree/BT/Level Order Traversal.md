## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351241/assignment/problems/206?navref=cl_tt_lst_nm
## Understanding:
- Given 
	- Root of a Binary Tree
- To Return
	- Level order traversal of the binary tree
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- In level order traversal, Each level is getting into result based on FIFO. Hence we can use a queue to achieve the same.
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(N) Auxiliary space
### Reference:
### Code
```Java

// Time Complexity: O(N) iterating through each element in the given tree
// Space Complexity: O(N) Auxillary space
private ArrayList<ArrayList<Integer>> findLevelOrder(TreeNode root){

	Deque<TreeNode> queue = new ArrayDeque<>();
	ArrayList<ArrayList<Integer>> levels = new ArrayList<>();
	ArrayList<Integer> level = new ArrayList<>();
	
	TreeNode last = root;
	
	queue.addLast(root);
	
	while(!queue.isEmpty()){
		TreeNode curr = queue.removeFirst();
		if(curr.left != null) queue.addLast(curr.left);
		if(curr.right != null) queue.addLast(curr.right);
		level.add(curr.val);
		if(curr == last){
			levels.add(level);
			level = new ArrayList<>();
			last = queue.isEmpty() ? null : queue.getLast();
		}
	}

	return levels;
}
```



