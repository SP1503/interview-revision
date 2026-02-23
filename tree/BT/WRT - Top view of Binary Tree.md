## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351222/assignment/problems/5715?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Root of a Binary Tree
- To return:
	- Top view of the binary tree, Return a list that contains all the elements that can be viewed from the top of the binary tree.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Observation:
	- ~~The top viewed elements are nothing but the first and last elements of each level of the binary tree.~~
	- ~~So adding the first and last element of every level is the top view elements result.~~
	- Top view elements are nothing but the first element of every vertical order list.
### Reference:![[WhatsApp Image 2026-02-18 at 10.55.55 AM.jpeg]]
### Code
```Java

private ArrayList<Integer> topView = new ArrayList<>();

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
private ArrayList<Integer> findTopViewEle(TreeNode root){
	return findTopView(root);
}

public ArrayList<Integer> findTopView(TreeNode A) {
	return findVerticalOrderFirst(A);
}

private ArrayList<Integer> findVerticalOrderFirst(TreeNode root){
	
	populateVerticalOrder(root);
	
	ArrayList<Integer> vOrder = new ArrayList<>();
	
	for(int i = min; i <= max; i++){
		vOrder.add(this.order.get(i).get(0));
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

