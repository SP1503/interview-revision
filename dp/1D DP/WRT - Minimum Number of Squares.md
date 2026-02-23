#revision-1  #revision-2 #revision-3 #written-code-on-18-jan  #NeedRevisit 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/351252/assignment/problems/600?navref=cl_tt_lst_nm
## Understanding:
- Given an integer A.
- Find the minimum number of perfect squares sum up to make A
## Input and Output:
![[Screenshot 2026-01-06 at 8.57.36 AM.png]]
![[Screenshot 2026-01-06 at 8.57.51 AM.png]]

![[Screenshot 2026-01-06 at 8.58.06 AM.png]]
## Problem Constraints:
![[Screenshot 2026-01-06 at 8.58.24 AM.png]]
## Approach:
### Brute Force: 
- Idea: Need to explore all the possible sum required to find the min number of sum required.
- **Recurrence relation:** F(A) = 1 + min(F(A - k * k)) for k = 1 to k * k <= A
- **Base Condition:** F(0) = 0, F(1) = 1, F(2) = 2, F(3) = 3
- **Time Complexity:** At every stage we are making root N decisions. Hence the time complexity is O(Root N power N)
- **Space Complexity:** O(N) taking N recursive depths
### Optimised Approach:
- Repeatedly finding the perfect squares required to make A called overlapping subproblem
- Avoid that using caching top down approach.
- **Forced Order:** F(A) depends on F(A - k) hence we need to find A = 0 to find A = k 
- **Time Complexity:** O(N) as we are iterating from 1 to A
- **Space Complexity:** O(N)
### Reference:
![[WhatsApp Image 2026-01-06 at 9.17.34 AM.jpeg]]
### Code

```Java
Recursion:
private int findMinPerfectSquares(int A){
	// Base condition
	if(A <= 3) return A;
	
	// Recurrence Relation
	int minSquares = Integer.MAX_VALUE;
	for(int i = 1; i * i <= A; i++) 
		minSquares = Integer.min(minSquares, findMinPerfectSquares(A - (i * i)));
	
	return 1 + minSquares;
}

Top Down Approach:
private int findMinPefectSqauresTopDownImpl(int A){
	int[] cache = new int[A + 1];
	
	for(int i = 0; i <= A; i++) cache[i] = Integer.MAX_VALUE;
	
	return findMinPerfectSquaresTopDown(A, cache);
}

private int findMinPerfectSquaresTopDown(int A, int[] cache){
	// Base condition
	if(A <= 3) return A;
	
	// Recurrence Relation
	int minSquares = Integer.MAX_VALUE;
	for(int i = 1; i * i <= A; i++){
		if(cache[A - (i * i)] == Integer.MAX_VALUE) 
			cache[A - (i * i)] = findMinPerfectSquares(A - (i * i)); 
		minSquares = Integer.min(minSquares, cache[A - (i * i)]);
	} 
		
	cache[A] = 1 + minSquares 
	return cache[A];
}

Bottom Up Approach:
private int findMinPerfectSquareBottomUp(int A){
	int[] minSquareCount = new int[A + 1];
	
	for(int i = 0; i <= A; i++){
		if(i <= 3) minSquareCount[i] = i;
		else{
			// Recurrence Relation
			int minSquares = Integer.MAX_VALUE;
			for(int j = 1; j * j <= i; j++) 
				minSquares = 
					Integer.min(minSquares, minSquareCount[A - (j * j)]));
			
			minSquareCount[i] = 1 + minSquares;
		}
	}
	
	return minSquareCount[A];
}

```
