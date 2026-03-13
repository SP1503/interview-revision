## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351220/assignment/problems/4116?navref=cl_tt_lst_nm
## Understanding:
- Given an integer array and integer K
- Find the first contiguous array whose sum = k
## Input and Output:
- 
## Problem Constraints:
## Approach:
### Brute Force:
- Generate all the subarrays and find its sum 
- If sum == n return that subarray
- Complexity:
	- Time Complexity: O(N * N) contribution technique
	- Space Complexity: O(1)
### Optimised Approach:
- Find the prefix sum
- Prefix sum is always sorted as we are adding up only the positive elements.
- If prefix[i] = n return subarray formed by o to i.
- Two pointers l and R
	- Sorted array
	- p[j] - p[i] > k -> the decision will be ambiguous if l = 0 and r = n  
	- Hence we can use l = 0 and r = 1;
- Check if p[j] - p[i] = n if yes return that subarray
- Complexity:
	- Time complexity: O(N)
	- Space Complexity: O(N)
### Optimised Approach dynamic sliding window:
- 
### Reference:
### Code
```Java
# Find contigous sum using two points

private int[] findContigousSum(int[] ele){
	
	int len = ele.length;
	int[] prefix = new int[len];
	
	prefix[0] = ele[0];
	for(int i = 1; i < len; i++) prefix[i] = ele[i] + prefix[i - 1];
	
	int l = 0;
	int r = 1;
	
	while(r < len){
		if(prefix[r] - prefix[l] == k) return generateSubarray(i, j, ele);
		else if(prefix[r] - prefix[l] > k) l++;
		else r++;
	}
	
	return null;
}

#Find contigous array using sliding window
private int[] findContigousArraySum(int[] ele){

	Deque<Integer> queue = new ArrayDeque<>();
	
	int sum = 0;
	
	for(int i = 0; i < ele.length; i++){
		sum += ele[i];
		
		while(!queue.isEmpty() && sum > k) sum -= queue.removeFirst();
		
		if(sum == k) return queue;	
	}
	
	return null;
}
```
