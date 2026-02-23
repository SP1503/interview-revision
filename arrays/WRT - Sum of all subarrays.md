## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/274977/assignment/problems/14363?navref=cl_tt_lst_nm
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Generate all the subarray and find its sum
- Complexity:
	- O(N * N * N)
### Optimised Approach:
### Reference:![[WhatsApp Image 2026-02-21 at 3.57.13 PM.jpeg]]
### Code
```Java

private long findSum(int[] A){
	long sum = 0l;
	for(int i = 0; i < A.length; i++){
		long noOfStartInd = i + 1l;
		long noOfEndInd = A.length - i;
		sum += (noOfStartInd * noOfEndInd * A[i]);
	}
	return sum;
}

```
