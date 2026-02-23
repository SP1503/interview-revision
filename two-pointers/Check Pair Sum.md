## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351234/assignment/problems/21202/?navref=cl_pb_nv_tb
## Understanding:
- Given sorted array.
- Find whether there are any pairs with sum exists
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Using nested loops to find pairs with sum TC: O(N * N) SC: O(1)
- Binary Search: since the elements are sorted we can use binary search TC:O(N log N) SC:O(1)
- Hashing: We can hash the elements and find target - A[i] in the has TC: O(N) SC: O(N)
### Optimised Approach:
### Reference:
### Code
```Java

// Time Complexity: O(N)
// Space Compelxity: O(1)

// At two pointers two questions to answer
// Where to keep the two pointers -> same corners or different corners
// What is the condition using which we can reduce the search space 
private boolean isPairExists(int[] ele, int target){
	
	// Search space
	int l = 0;
	int r = ele.length - 1;
	
	while(l < r){
		int sum = ele[l] + ele[r];
		
		// Make a guess and reduce search space
		if(sum == target) return true;
		else if(sum < target) l++;
		else r--;
	}
} 

```
