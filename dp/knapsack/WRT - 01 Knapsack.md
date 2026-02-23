#revision-1 #revision-2  #CanBeImplementedWithoutRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351256/assignment/problems/9292?navref=cl_tt_lst_nm
## Understanding:
- Given N products info in two integers weight, values.
- Given an integer current capacity.
- Find the maximum happiness the person can get with capacity given
## Input and Output:
![[Screenshot 2026-01-10 at 1.28.00 PM.png]]
![[Screenshot 2026-01-10 at 1.28.16 PM.png]]
![[Screenshot 2026-01-10 at 1.28.32 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-10 at 1.28.50 PM.png]]
## Approach:
### Brute Force:
- The locally optimal decision that we are taking will not lead to global optimal.
- Taking the product with max happiness first will not give the max happiness that we can get from the current capacity we have.
- Trying out all the possible combinations.
- States we need:
	- Current capacity we have at this current step.
	- At which index we are at.
- At each step we have two options:
	- Take the current product
	- Skip the current product
- Out of these options find the maximum.
- **Recurrence relation:** 
	- F(index, current Capacity) = Integer.max(values[index] + F(index + 1, current capacity - weights[index]), F(index + 1, current Capacity))
- **Base Condition:** 
	- If capacity == 0 no more taking return 0
	- If all the index are considered then return 0
- **Time Complexity:** O(2 ^ N) as we are taking 2 decisions at every step.
- S**pace Complexity:** O(N) depth of recursion is N.
### Optimised Approach:
- This problem have optimal substructure and overlapping sub problems.
- Hence this can be optimised using Top down and Bottom up approach of DP.
- Forced Order: 
	- index depends on index + 1. Hence reverse order
	- capacity depends on capacity - x, Hence forward order.
- Cache shape: edge cases depends on index + 1 and capacity - x.
- **Time Complexity:** O(N * capacity)
- **Space Complexity:** O(N * capacity)
### Reference:![[WhatsApp Image 2026-01-10 at 1.39.42 PM.jpeg]]
### Code
```Java

Recursive:
private int findMaxPossHapp(
	int[] values, 
	int[] weights, 
	int capacity, 
	int currIndex){
	
	// Base Condition
	if(capacity == 0) return 0;
	else if(currIndex == values.length) return 0;
	else{
		// Recurrence Relation
		// Take
		int take = 0;
		if(weights[currIndex] <= capacity) 
			take = values[currIndex] + 
				findMaxPossHapp(values, weights,
					capacity - weights[currIndex], currIndex + 1);
		// No Take
		int noTake = findMaxPossHapp(values, weights, capacity, currIndex + 1);
		return Math.max(take, noTake);
	}
}

Bottom Up:
private int findMaxPossHappIterative(int[] values, int[] weights, int totalCap){
	int n = values.length;
	int[][] maxPoss = new int[n + 1][totalCap + 1];
	for(int currIndex = n; currIndex >= 0; currIndex--){
		for(int capacity = 0; capacity <= totalCap; capacity++){
			if(capacity == 0) maxPoss[currIndex][capacity] = 0;
			else if(currIndex == n) maxPoss[currIndex][capacity] = 0;
			else{
				// Recurrence Relation
				// Take
				int take = 0;
				if(weights[currIndex] <= capacity) 
				take = values[currIndex] 
					+ maxPoss[currIndex + 1][capacity - weights[currIndex]];
				
				// No Take
				int noTake = maxPoss[currIndex + 1][capacity];
				maxPoss[currIndex][capacity] = Math.max(take, noTake);
			}
		}
	}
	return maxPoss[0][totalCap];
}

```

