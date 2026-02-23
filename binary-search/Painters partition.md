==Always check the predicate method part of Binary Search in answer making mistakes repeatedly==
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/313334/assignment/problems/271?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- A painters
	- B time to paint per unit of board.
	- C[] array represents the length of ith board in given C
## Input and Output:
![[Screenshot 2026-02-16 at 4.42.25 PM.png]] ![[Screenshot 2026-02-16 at 4.42.40 PM.png]]
![[Screenshot 2026-02-16 at 4.43.07 PM.png]]

## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
- We can do binary search on the min time as the time is monotonic
### Reference:
### Code:

```Java

private int findPainterCount(long[] workTime, long timePP ){
	int pCount = 1;
	int index = 0;
	long remTime = timePP;
	
	while(index < workTime.length){
		
		// Check if full time is possible
		if(timePP < workTime[index]) return -1;
		
		// If current time is not sufficient to paint the current board
		if(remTime < workTime[index]){
			pCount++;
			remTime = timePP;
		}
		
		remTime -= workTime[index];
		index++;
	}
	
	return pCount;
}

private long findMinTime(int[] board, int pCount, int tPerB){
	
	// Define Search Space
	int n = board.length;
	long[] workTime = new int[n];
	
	for(int i = 0; i < n; i++) workTime[i] = board[i] * 1l * tPerB;
	
	long minTime = findMax(workTime);
	long maxTime = findSum(workTime);
	
	long finalMinTime = maxTime;
	
	while(minTime < maxTime){
		
		// Make Guess
		long propTime = minTime + (maxTime - minTime) / 2;
		int pCountReq = findPainterCount(workTime, propTime);
		
		// Based on guess reduce the search space
		if(pCountReq == -1) minTime = propTime + 1;
		else if(pCountReq > pCount) minTime = propTime + 1;
		else {
			finalMinTime = propTime;
			maxTime = propTime - 1;
		}
	}
	
	return finalMinTime;
}

private long findMax(int[] work){
	long maxTime = 0;
	for(int currWork : work) maxTime = Integer.max(maxTime, currWork);
	return maxTime;
}

private int findSum(int[] work){
	long totalWork = 0;
	for(int currWork : work) totalWork += currWork;
	return totalWork;
}
```


