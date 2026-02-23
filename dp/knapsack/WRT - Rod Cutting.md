#revision-1 #revision-2 #NeedRevisit #written-code-on-18-jan 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351260/assignment/problems/9318/submissions
## Understanding:
- Given a integer array A represents the cost of stick if stick size = index i  + 1
- Find the maximum price we can get by cutting the rod into different sizes.
## Input and Output:
![[Screenshot 2026-01-10 at 5.29.56 PM.png]]
![[Screenshot 2026-01-10 at 5.30.16 PM.png]]
![[Screenshot 2026-01-10 at 5.31.20 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-10 at 5.30.47 PM.png]]
## Approach:
### Brute Force:
- We need to find different combinations of cutting the rod and find the maximum value that we can get out of it.
- We need to find all the possibilities, Hence proceeding with recursion.
- **Recurrence relation:** F(i, rodLength) = Math.max(F(i, rodLength - (i + 1), F(i + 1, rodLength)))
	- We have two options at every state.
		- Either cutting with current size
		- Not cutting and proceeding with next size.
- **Base Condition:**
	- If rodLength == 0 , no sizes to cut return 0
	- If index == n we cannot make at-least one cut as rodLength == index return 0
- **Complexity:**
	- **Time Complexity:** At every state we are taking 2 decisions and the depth can go until N. hence the time complexity : O(2 ^ N)
	- **Space Complexity:** O(N)
### Optimised Approach:
- We can optimise this using Bottom Up approach
- Forced order:
	- index depends on index + 1-> reverse order index = n to 0
	- rodLength depends on rodLength - (index + 1) -> forward order. rodLength = 0 to n
- Cache shape: O(2D cache)
### Reference:
![[WhatsApp Image 2026-01-18 at 1.57.34 PM.jpeg]]
### Code
```Java


Recursive:
private int findMaxCost(ArrayList<Integer> cost, int index, int rodLength){
	// Base Condition
	if(index >= cost.size()) return 0;
	else if(rodLength == 0) return 0;
	else{
		// Recurrence Relation
		int cuttingWithCurr = 0;
		if(rodLength >= (index + 1))
			cuttingWithCurr = cost.get(index) 
				+ findMaxCost(cost, index, rodLength - (index + 1));
		int noCutting = findMaxCost(cost, index + 1, rodLength);
		return Math.max(cuttingWithCurr, noCutting);
	}
}


Bottom Up:
private int findMaxCostIterative(ArrayList<Integer> cost){
	int[][] maxPoss = new int[cost.size() + 1][cost.size() + 1];
	for(int index = cost.size(); index >= 0; index--){
		for(int rodLength = 0; rodLength <= cost.size(); rodLength++){
			// Base Condition
			if(index >= cost.size()) maxPoss[index][rodLength] = 0;
			else if(rodLength == 0) maxPoss[index][rodLength] = 0;
			else{
				// Recurrence Relation
				int cuttingWithCurr = 0;
				if(rodLength >= (index + 1)){
					cuttingWithCurr = cost.get(index) 
						+ maxPoss[index][rodLength - (index + 1)];
				}
				int noCutting = maxPoss[index + 1][rodLength];
				maxPoss[index][rodLength] = Math.max(cuttingWithCurr, noCutting);
			}
		}
	}
	return maxPoss[0][cost.size()];
}

```

