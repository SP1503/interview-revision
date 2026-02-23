#revision-1 #CanBeImplementedWithoutRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351256/assignment/problems/9340/submissions
## Understanding:
- Given integer array values[] of size N.
- Given integer array weights[] of size N.
- Given integer capacity.
- Find the max values we can have with the given capacity.
- Taking the same product again is okay.
## Input and Output:
![[Screenshot 2026-01-10 at 2.19.41 PM.png]]
![[Screenshot 2026-01-10 at 2.19.55 PM.png]]
![[Screenshot 2026-01-10 at 2.20.06 PM.png]]
## Problem Constraints:
 ![[Screenshot 2026-01-10 at 2.20.22 PM.png]] 
## Approach:
### Brute Force:
- Thinking greedily wont help as the local optimum decision that we are taking not leads to global optimum.
- Finding all the possibilities including the duplicates only will help.
- Recursion can be used to find the possibilities.
- **Recurrence Relation:**
	- F(index, capacity) = Math.max(values[index] + F(index, capacity - weights[index]), F(index + 1, capacity))
	- Decisions that we can take at one step:
		- **Taking the current product** even though it is already taken.
		- **Not taking current product** again and moving forward.
- **Base Condition:**
	- If capacity == 0 nothing we can take return 0
	- If index == weights.length then nothing left to take return 0
- **Time Complexity:** O(2 ^ N) we are taking at max two decisions art every step.
- **Space Complexity:** O(N) the recursion stack can go until n depth.
### Optimised Approach:
- In the recursion tree we can see the optimal substructure and overlapping subproblems.
- We can solve that using the Top Down and Bottom Up Approach.
- Forced Order:
	- index i depends on i + 1. Hence reverse order required
	- Capacity depends on capacity - weights[index]. Hence forward order required.
- Cache Shape: 2D cache required.
### Reference:
![[WhatsApp Image 2026-01-10 at 2.33.02 PM.jpeg]]
### Code
```Java

Recursion:
private int findMaxHapp(int[] weights, int[] values, int capacity, int index){
	// Base Condition
	if(index == weights.length) return 0;
	else if(capacity == 0) return 0;
	else {
		// Recurrence Relation
		// Take current
		int takingCurr = 0;
		if(capacity >= weights[index]){
			takingCurr = values[index] 
				+ findMaxHapp(weights, values, capacity - weights[index], index);
		}
		// Not Taking
		int notTaking = findMaxHapp(weights, values, capacity, index + 1);
		return Math.max(takingCurr, notTaking);
	}
}

Bottom Up:
private int findMaxHappIterative(int[] weights, int[] values, int totCapacity){
	int n = weights.length;
	int[][] maxHapp = new int[n + 1][totCapacity + 1];
	for(int index = n; index >= 0; index--){
		for(int capacity = 0; capacity <= totCapacity; capacity++){
			if(index == weights.length) maxHapp[index][capacity] = 0;
			else if(capacity == 0) maxHapp[index][capacity] = 0;
			else {
				// Recurrence Relation
				// Take current
				int takingCurr = 0;
				if(capacity >= weights[index]){
				takingCurr = values[index] 
					+ maxHapp[index][capacity - weights[index]];
				}
				// Not Taking
				int notTaking = maxHapp[index + 1][capacity];
				maxHapp[index][capacity] = Math.max(takingCurr, notTaking);
			}
		}
	}
	return maxHapp[0][totCapacity];
}

```


