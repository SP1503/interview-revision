## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private boolean isReachable(int[] nums){
	int maxReach = 0;
	for(int i = 0; i < nums.length; i++){
		// Check if from current indes what is the max reach
		if(i <= maxReach) maxReach = Integer.max(maxReach, i + nums[i]);
		// If index >= maxReach, 
		// meaning we not able to reach current index hance return false
		else return false;
	}
	return true;
}
```

