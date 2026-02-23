#revision-1 #revision-2 #revision-3  #CanBeImplementedWithoutRevisit  
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/351252/assignment/problems/30?navref=cl_tt_lst_nm
## Understanding:
- Given a integer n represents the number of steps.
- Person at step 0.
- We can take 1  step and 2 step at a time.
- Find total number of ways to reach from step 0 to step n.
## Input and Output:
![[Screenshot 2026-01-03 at 8.06.15 AM.png]]
![[Screenshot 2026-01-03 at 8.07.10 AM.png]]
![[Screenshot 2026-01-03 at 8.07.52 AM.png]]
## Problem Constraints:
![[Screenshot 2026-01-03 at 8.08.51 AM.png]]
## Approach:
### Brute Force: 
- Idea here we need to find all the possible ways we can do to reach step n.
- All possibilities = recursion.
- Let f(k) is a function that will say total number of ways required to reach step k from step 0.
- We can reach nth step only by 
	- Taking 1 step from n - 1 step
	- Taking 2 step from n - 2 step
- **Recurrence relation:** *f(n) = f(n - 1) + f(n - 2)*
- **Base condition:** 
	- f(0) = 1 we can reach step 0 by not doing anything that is also a way
	- f(1) = 1 we can reach step 1 by taking 1 step which is the only way
- **Time Complexity:** At every function call we take 2 decisions either take 1 step or take 2 step
- **Space Complexity:** Recursion depth can go till F(N) so O(N)
### Optimised Approach: Top Down Approach
- Here we can see the overlapping subproblems (solving the same problem again and again) in the recursion tree.
- Hence we can cache the same problem solution in a array.
- **Time Complexity:** O(N),  only distinct calls will compute other values are used from cache.
- **Space Complexity:** O(N) for caching + O(N) for recursion depth.
### Optimised Approach: Bottom Up Approach
- **Forced Order**: 
	- f(n) = f(n - 1) + f(n - 2), f(n) depends on f(n - 1) and f(n - 2). 
	- Hence if we need to compute f(k) where k = 3, then we need f(2) and f(1)
	- Hence the order will be i = 0 to i = n.
- Iterate the steps from 0 to N
	- if(step = 0 || step = 1) no of ways = 1;
	- else no of ways = no of ways[step - 1] + no of ways[step - 2]
- return no of ways[n];
- **Time Complexity:** O(N),  Iterating from step 0 to step n.
- **Space Complexity:** O(N) for storing number of steps from 0 to N .
### Reference:

![[WhatsApp Image 2026-01-03 at 8.46.05 AM 1.jpeg]]
### Code:
```Java
Recursion:
private int findTotalNoOfWays(int step){
	if(step == 0 || step == 1) return 1;
	else return findTotalNoOfWays(step - 1) + findNoOfWays(step - 2);
}

Top Down Approach:
private int findTotalNoOfWaysTopDown(int n){
	int[] cache = new int[n + 1];
	cache[0] = 1;
	cache[1] = 1;
	
	for(int i = 2; i <= n; i++) cache[i] = -1;
	
	return findTotalNoOfWays(n, cache);
}

private int findTotalNoOfWays(int step, int[] cache){
	if(step == 0 || step == 1) return 1;
	
	if(cache[step] == -1) 
		cache[step] = findTotalNoOfWays(step - 1, cache) 
				+ findTotalNoOfWays(step - 2, cache);
		
	return cache[step];
}

Bottom Up Approach:
private int findTotalNoOfWaysBottomUp(int n){
	int[] noOfWays = new int[n + 1];
	
	for(int step = 0; step <= n; step++){
		if(step == 0 || step == 1) noOfWays[step] = 1;
		else noOfWays[step] = noOfWays[step - 1] 
			+ noOfWays[step - 2];
	}
	
	return noOfWays[n];
}
```



