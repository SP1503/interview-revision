## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351253/assignment/problems/152?navref=cl_tt_lst_nm
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

// Time Complexity: O(N)
// Space Complexity: O(N)
private int findLongestConsecutiveSeq(int[] eles){
	
	Set<Integer> hash = new HashSet<>();
	
	for(int ele : eles) hash.add(ele);
	
	int maxCount = 0;
	for(int ele : eles){
		if(hash.contains(ele - 1)) continue;
		else{
			int curr = ele;
			int count = 0;
			while(hash.contains(curr)){
				count++;
				curr++;
			}
			maxCount = Integer.max(maxCount, count);
		}
	}
	
	return maxCount;
}
```

