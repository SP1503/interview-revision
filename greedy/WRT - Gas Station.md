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

private int findIsPossible(int[] gas, int[] cost){
	// Check whether the solution is Possible or not
	int gasSum = 0;
	int costSum = 0;
	for(int i = 0; i < gas.length; i++){
		gasSum += gas[i];
		costSum += cost[i];
	}
	if(gasSum < costSum) return -1;
	// The loop is possible, find the starting point
	int start = -1;
	int remGas = 0;
	for(int i = 0; i < gas.length; i++){
		int totalGasAvail = remGas + gas[i];
		if(totalGasAvail - cost[i] >= 0){
			// Determine the start point
			if(remGas == 0) start = i;
			// Carry forward the gas present
			remGas = totalGasAvail - cost[i];
		}
		else{
			// Not able to determine the start point
			start = -1;
			// Mark gas as 0
			remGas = 0;
		}
	}
	return start;
}

```

