## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351234/assignment/problems/9323?navref=cl_tt_lst_nm
## Understanding:
- Given sorted array
- Find the count of pairs whose difference = target
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Using nested loops to find pairs with sum TC: O(N * N) SC: O(1)
- Binary Search: since the elements are sorted we can use binary search TC:O(N log N) SC:O(1)
- Hashing: We can hash the elements and find target - A[i] in the has TC: O(N) SC: O(N)
### Optimised Approach:
- We can use two pointers here
- Option 1: pointing to different corners
	- If i++ diff decreases
	- If j-- difference decreases
	- hence this decision making is ambiguous
- Option 2 : pointing to same corners
	- If j++ difference increases
	- If i++ difference decreases
	- We can use this approach
### Reference:
### Code
```Java

private int findPairDiffCount(int[] ele, int tar){
	
	// Search Space
	int l = 0;
	int r = 1;
	
	while(r < ele.length){
		
		// Make a guess and reduce the search space
		int diff = ele[r] - ele[l];
		if(diff == target){
			pairCount++;
			if(A.get(l).equals(A.get(r))){
				int a = A.get(l);
				while(l < A.size() && a == A.get(l)){
					l++;
				}
				r = l + 1;
			}
			else{
				int a = A.get(l);
				int b = A.get(r);
				while(l < r && a == A.get(l)){
					l++;
				}
				while(r < A.size() && b == A.get(r)){
					r++;
				}
			}
				
		}
		else if(l == r) r++;
		else if(diff < target) r++;
		else l++:
	}
}
```

