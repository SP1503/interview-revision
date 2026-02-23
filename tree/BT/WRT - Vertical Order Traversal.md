## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351222/assignment/problems/378?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a binary tree
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
![[WhatsApp Image 2026-02-18 at 9.17.46 AM (1).jpeg]]
### Code
```Java

private Map<Integer, ArrayList<Integer>> order = new HashMap<>();
private int min = 0;
private int max = 0;

class NodeInfo{

	TreeNode node;
	int rank;
	
	NodeInfo(TreeNode node, int rank){
		this.node = node;
		this.rank = rank;
	}

}

// Time Complexity: O(N + Order count)
// Space Compelxity : O(N)

public ArrayList<ArrayList<Integer>> verticalOrderTraversal(TreeNode A) {
	return findVerticalOrder(A);
}

private ArrayList<ArrayList<Integer>> findVerticalOrder(TreeNode root){
	
	populateVerticalOrder(root);
	
	ArrayList<ArrayList<Integer>> vOrder = new ArrayList<>();
	for(int i = min; i <= max; i++){
		vOrder.add(this.order.get(i));
	}
	
	return vOrder;
}

  

private void populateVerticalOrder(TreeNode root){
	Deque<NodeInfo> queue = new ArrayDeque<>();

	queue.add(new NodeInfo(root, 0));

	while(!queue.isEmpty()){
		NodeInfo currNodeInfo = queue.removeFirst();
		
		TreeNode currNode = currNodeInfo.node;
		int currRank = currNodeInfo.rank;
		
		ArrayList<Integer> ele = this.order.containsKey(currRank)
								? this.order.get(currRank)
								: new ArrayList<>();
	
		ele.add(currNode.val);
		this.order.put(currRank, ele);
		
		if(currNode.left != null){
			queue.add(new NodeInfo(currNode.left, currRank - 1));
			min = Math.min(min, currRank - 1);
		}
		
		if(currNode.right != null){
			queue.add(new NodeInfo(currNode.right, currRank + 1));
			max = Math.max(max, currRank + 1);
		}
	}
}
```




