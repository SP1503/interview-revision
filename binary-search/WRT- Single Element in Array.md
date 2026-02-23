## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351231/assignment/problems/4131?navref=cl_tt_lst_nm
## Understanding:
- Given every element occurs twice except one element. 
- Find the given element that occurs only once.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- A simple XOR operations on the given array will given the unique element
- Complexity: 
	- Time Complexity: O(N)
	- Space Complexity: O(2)
### Optimised Approach:
- If we observe the inputs
	- Observation: As every element occurs twice except one element, Initially the duplicate element will occur at odd index. But after the occurrence of unique element the duplicate element will occur in the even index.
	- Based on the nature of the duplicate element index, we can eliminate half of search space at every decision.
- Complexity:
	- Time Complexity: O(log N)
	- Space Complexity: O(1)
### Reference:![[WhatsApp Image 2026-02-16 at 7.10.36 AM 1.jpeg]]

### Code
```Java

private int findUniqueEle(int[] ele){
	int unqiue = 0;
	for(int element : ele) unique ^= element;
	return unique;
}

private int findUniqueElement(int[] ele){
	
	// Define Search Space
	int l = 0;
	int r = ele.length - 1;
	
	while(l <= r){
		
		// Make Guess
		int mid = l + (r - l) / 2;
		
		if((mid == 0 || ele[mid] != ele[mid - 1]) 
			&& ((mid == ele.length - 1) || ele[mid] != ele[mid + 1])) 
			return ele[mid];
		
		// Reduce search space by 2 based on the decision
		if(mid == 0 || ele[mid] != ele[mid - 1]){
			// Pair is (mid , mid + 1)
			if(mid % 2 == 0) l = mid + 1;
			else r = mid - 1;
		}
		else{
			// Pair is (mid - 1, mid)
			if(mid % 2 == 0) r = mid - 1;
			else l = mid + 1;
		}
	}
	
	return -1;
}

// Time Compelxity: O(log N)
// Space Complexity: O(1)
```

