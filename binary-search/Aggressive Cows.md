
==Always check the predicate method part of Binary Search in answer making mistakes repeatedly==
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351230/assignment/problems/4129/?navref=cl_pb_nv_tb
## Understanding:
- Given
	- Stall location array
	- Number of cows to be placed
- To return
	- Find the max Distance where the n cows can be placed that is minimum
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private int findCowsCount(int[] stalls, int dist){
	
	int cowCnt = 1;
	int lastPlacedStall = stalls[0];
	
	for(int index = 1; index < stalls.length; index++){
		
		// Less than considering distance
		if(stalls[index] - lastPlacedStall >= dist){
			lastPlacedStall = stalls[index];
			cowCnt++;
		}
	}
	
	return cowCnt;
}

private int findMaxDist(int[] stalls, int cowCnt){

	// Sort the stalls to place cows in order
	Arrays.sort(stalls);
	
	// Search Space
	int n = stalls.length;
	int minDist = 1;
	int maxDist = stalls[n - 1] - stalls[0];
	
	int finalMaxDist = 1;
	
	while(minDist <= maxDist){
		
		// Make Guess
		int propDist = minDist + (maxDist - minDist) / 2;
		int simCwCnt = findCowCount(stalls, propDist);
		
		// Based on Guess make decision
		if(simCwCnt >= cowCnt){
			// Can place more than or equal cow cnt
			finalMaxDist = propDist;
			minDist = propDist + 1;
		}
		else maxDist = propDist - 1;
	}
	
	return finalMaxDist;
}
```



