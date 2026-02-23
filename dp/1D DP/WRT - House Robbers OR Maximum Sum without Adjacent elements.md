#revision-1  #revision-2  #NeedRevisit #written-code-on-18-jan 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/351257/assignment/problems/278?navref=cl_tt_lst_nm
## Understanding:
- Given an integer array having n elements
- If we pick ith element for sum we are not allowed to pick i + 1 the element.
- Find the maximum sum that can be formed without touching the adjacent elements.
## Input and Output:
![[Screenshot 2026-01-07 at 5.56.42 AM.png]]
![[Screenshot 2026-01-07 at 5.57.01 AM.png]]![[Screenshot 2026-01-07 at 5.57.35 AM.png]]
## Problem Constraints:
![[Screenshot 2026-01-07 at 5.58.16 AM.png]]
## Approach:
### Brute Force:
- **Idea:** To find all the possible sum and then find the max of it.
- Recursion is the way to find all the possibilities.
- **Recurrence relation:** F(i) = Integer.max(nums[i] + F(i + 2), F(i + 1))
- **Base Condition:** if i >= nums.length return 0
- **Time Complexity:** Taking 2 decisions at every stage and Number of levels are N. Hence the time complexity is O(2 ^ N)
- **Space complexity:** O(N) going N steps deeper.
### Optimised Approach I:
- We can see many overlapping sub problems that arises in the recursion tree.
- We can avoid computing the same solution for same index by using caching.
- Following the top down approach
- **Time Complexity:** Taking 2 decisions at every stage, But distinct calls are only made already found answers are reused and Number of levels are N. Hence the time complexity is O(N).
- **Space Complexity:** O(N + N) for the recursion depth and the caching values.
### Optimised Approach II:
- Converting this into Bottom up using forced order.
- Forced order: F(i) = Integer.max(nums[i] + F(i + 2), F(i + 1)), i depends on i + 1 or i + 2.
- So to find i = 0 we need 1 and 2 values. Hence iteration is in reverse order i = N to i = 0
- **Time Complexity:** O(N) iterating from i = N to i = 0
- **Space Complexity:** O(N)
### Reference:
### Code:
```Java
Recursion:

private int findMaxSumWithoutAdjacent(int[] nums, int i){
	
	// Base Condition
	if(i >= nums.length) return 0;
	
	// Recurrence relation
	int taking = nums[i] + findMaxSumWithoutAdjacent(nums, i + 2);
	int skipping = findMaxSumWithoutAdjacent(nums, i + 1);
	
	return Integer.max(taking, skipping);
}

Top Down Approach:

private int findMaxSumWithoutAdjacentTopDown(int[] nums){
	int[] caching = new int[nums.length];
	
	return findMaxSumWithoutAdjacentTopDownImpl(nums, 0, caching);
}

private int findMaxSumWithoutAdjacentTopDownImpl(
	int[] nums, 
	int i, 
	int[] caching){
	
	// Base Condition
	if(i >= nums.length) return 0;
	
	// Recurrence relation
	if(caching[i + 2] == 0) 
		caching[i + 2] = findMaxSumWithoutAdjacent(nums, i + 2);
	
	if(caching[i + 1] == 0)
		caching[i + 1] = findMaxSumWithoutAdjacent(nums, i + 1);
		
	int taking = nums[i] + caching[i + 2];
	int skipping = caching[i + 1];
	
	return Integer.max(taking, skipping);
}

Bottom Up Approach:

private int findMaxSumWithoutAdjacentTopDown(int[] nums){
	int[] maxSumWithoutAdj = new int[nums.length + 1];
	
	for(int i = nums.length + 1; i >= 0; i--){
		// Base Condition
		if(i >= nums.length) maxSumWithoutAdj[i] = 0;
		else{
			// Recurrence relation
			int taking = nums[i] + maxSumWithoutAdj[i + 2];
			int skipping = maxSumWithoutAdj[i + 1];
			
			maxSumWithoutAdj[i] = Integer.max(taking, skipping);
		}
	}
	
	return maxSumWithoutAdj[0];
}
```
