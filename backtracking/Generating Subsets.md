## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351227/assignment/problems/89434/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- Array containing integer elements 
- To return:
	- The subsets of the given array with integer elements.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Here we need to Generate all the possible subsets.
- Hence there is no way we need to backtrack using recursion
- Subset is getting differed based on the presence of the element and its order.
- Hence on each element we can take two decision
	- Take 
	- No Take
- Complexity:
	- TC: O(2 ^ N)
	- SC: O(N)
### Reference:![[WhatsApp Image 2026-02-19 at 1.19.48 PM.jpeg]]
### Code
```Java

private ArrayList<ArrayList<Integer>> subsets = new ArrayList<>();

// Time Complexity: O(2 ^ N)
// Space Complexity: O(N)
private void findSubsets(
	ArrayList<Integer> ele, 
	int index, 
	ArrayList<Integer> subset){
	// Base Condition
	if(index == ele.size()){
		subsets.add(subset);
		return;
	}
	// Recurrence Relation
	// No Take
	findSubsets(ele, index + 1, new ArrayList<>(subset));
	
	// Take
	subset.add(ele.get(index));
	findSubsets(ele, index + 1, new ArrayList<>(subset));
	
	// Recursion is backtracking so undo the changes
	subset.remove(subset.size() - 1);
}

```

