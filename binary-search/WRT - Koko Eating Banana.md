==Always check the predicate method part of Binary Search in answer making mistakes repeatedly==
## Problem link:
## Understanding:
![[Screenshot 2026-02-16 at 7.39.37 PM.png]]
## Input and Output:
![[Screenshot 2026-02-16 at 7.39.51 PM.png]]
## Problem Constraints:
![[Screenshot 2026-02-16 at 7.40.05 PM.png]]
## Approach:

### Brute Force:
- Need to find speed.
- Need to iterate on the speed value only by one and check if koko can eat all the banana with K speed within h time.
	- If yes, we can return
	- If not we need to increase the speed.
- We are doing linear search. Here we know the minSpeed and maxSpeed
- Can we do Binary Search on speed as the speed range is monotonic ?
### Optimised Approach: Binary Search
- Here the answer lies in a range
- We can simulate whether the guessed answer is working.
- If the current answer works the current answer + 1 can also work. The answer is monotonous.
- speed = 5B/hr then 6B/hr also works
- **Complexity**:
	- **Time Complexity**: O(n log K) K is the different between max and min
	- **Space Complexity**: O(1)
### Reference:
![[WhatsApp Image 2026-02-16 at 7.45.42 PM.jpeg]]
### Code
```Java

// Time Compelxity: O(N Log H) H is the diff between minBanana and maxBanana
// Space Complexity: O(1)
private int findMinBananaToEat(int[] piles, int hour){
	
	// Search Space
	long minBanana = 1;
	long maxBanana = findMax(piles);
	
	long finalMinBanana = 1;
	
	while(minBanana <= maxBanana){
		
		// Making the guess
		long propBanana = minBanana + (maxBanana - minBanana) / 2;
		int hourReq = findReqHour(piles, propBanana);
		
		// Based on the decision reducing the search space
		if(hourReq <= hour){
			finalMinBanana = propBanana;
			maxBanana = propBanana - 1;
		}
		else{
			minBanana = propBanana + 1;
		}
	}
	
	return (int) finalMinBanana;
}

private int findReqHours(int[] piles, long bananaPH){
	long hours = 0;
	for(int pile : piles) hours += (pile + bananaPH - 1) / bananaPH;
	return (int) hours;
}
```




