#revision-1 #revision-2 #revision-4  #NeedRevisit #written-code-on-18-jan 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/155435/assignment/problems/368/?navref=cl_pb_nv_tb
## Understanding:
 - Given two Integers A and B.
 - A represents number of digits
 - B represents sum
 - Need to find total number of values whose digits = A and sum = B
## Input and Output:
![[Screenshot 2026-01-07 at 7.48.59 PM.png]]
![[Screenshot 2026-01-07 at 7.49.07 PM.png]]
![[Screenshot 2026-01-07 at 7.49.23 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-07 at 7.49.32 PM.png]]
## Approach:
### Brute Force:
- We need find all the possible values with digit A to find the total number of values
- Recurrence relation: F(i, j) = summation of F(i - 1, j - k) where k can be 0 to 9.
- Base Condition: 
	- if digit == 0 and sum == 0 return 1
	- if digit == 0 return 0
	- if digit == 1 and sum > 0 and sum <= 9 return 1
- **Time Complexity:** O(9 ^ digit)
- **Space Complexity:** O(digit)
### Optimised Approach:
- Having optimal subproblem and overlapping subproblems
- Hence trying Bottom up.
- Forced Order:
	- Recurrence relation = F(i, j) = F(i - 1, j - k) where k can be 0 to 9
	- i depends on i - 1
	- j depends on j - k
	- Hence we need i -1 to find i and j - k to find j
- **Time Complexity:** O(digit * sum)
- **Space Complexity:** O(sum)
### Reference:
### Code
```Java
Recursion:

private int findDigitCount(int digit, int sum){
// Base Condition
	if(digit == 0) return sum == 0 ? 1 : 0;
	else if(digit == 1) return sum >= 0 && sum <= 9 ? 1 : 0;
	else{
		// Recurrence relation
		int count = 0;
		for(int i = 0; i <= 9; i++){
		if(sum - i >= 0) count += findDigitCount(digit - 1, sum - i);
	}
	return count;
}

Bottom Up:

private int findDigitCountBottomUp(int digit, int sum){
	long[] a = new long[sum + 1];
	long[] b = new long[sum + 1];
	for(int i = 0; i <= digit; i++){
		for(int j = 0; j <= sum; j++){
			if(i == 0) b[j] = j == 0 ? 1 : 0;
			else if(i == 1) b[j] = j > 0 && j <= 9 ? 1 : 0;
			else{
				// Recurrence relation
				long currCount = 0;
				for(int val = 0; val <= 9; val++){
					if(j - val >= 0){
						currCount += a[j - val];
						currCount %= 1000000007;
					}
					else break;
				}
				b[j] = currCount;
			}
		}
		a = b;
		b = new long[sum + 1];
	}
	return (int) a[sum];
}
```

