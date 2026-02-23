## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351261/assignment/problems/125076?navref=cl_tt_lst_nm
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- The constraints are like 10, 15, 20, So exponential code is accepted.
- At every moment we have two decisions
	- Taking 1 step
	- Taking 2 step.
- Base Condition
	- If steps == 0 then we reached the top
- Complexity:
	- Time Complexity: O(2 ^ N)
	- Space Complexity: O(N)
### Reference:![[WhatsApp Image 2026-02-19 at 7.24.42 PM.jpeg]]
### Code
```Java

private List<List<Integer>> possSteps = new ArrayList<>();


// Time Complexity: O(2 ^ N)
// Space Complexity: O(N)
private void findPossWays(int steps, List<Integer> stepsTaken){
	
	// Base Condition
	if(steps <= 0){
		if(steps == 0) possSteps.add(stepsTaken);
		return;
	}
	
	// Recurrence Relation
	// At this moment I hae two decisions to take
	
	// Taking one step
	stepsTaken.add(1);
	findPossWays(steps -1, new ArrayList<>(stepsTaken));
	
	// Backtracking changes undo
	stepsTaken.remove(stepsTaken.size() - 1); 
	
	// Taking two step
	stepsTaken.add(2);
	findPossWays(steps - 2, new ArrayList<>(stepsTaken));
	
	// Backtracking changes undo
	stepsTaken.remove(stepsTaken.size() - 1);
}

```
