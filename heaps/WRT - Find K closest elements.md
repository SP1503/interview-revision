#revision- #revision-2 #revision-3 #NeedRevisit #written-code-on-18-jan 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/313436/assignment/problems/9264?navref=cl_tt_lst_nm
## Understanding:
- Given array nums and integer k.
- nums[] is nearly sorted. (Every element will be present at most k distance away from its sorted position)
- Return the sorted array.
## Input and Output:
![[Screenshot 2025-12-31 at 4.08.14 PM.png]]
![[Screenshot 2025-12-31 at 4.08.25 PM.png]]
## Problem Constrains:
![[Screenshot 2025-12-31 at 4.08.48 PM.png]]
## Approach:
### Brute force:
- Sort the given array
- **Time Complexity:** O(N log N)
- **Space Complexity:** O(1)
### Optimised Approach:
- Given that for element at index 0, the element will be present between [0 - k]
- element at index 1, the element will be present between [0, k + 1]
- element at index 2, the element will be present between [0, k + 2]
- Hence the approach = growing sliding window + min heap
- For the first window iterate the elements from o to k index
	- Insert into minHeap.
- The 0th element = min element of min heap.
- Iterate the given array from k + 1 to N
	- Add the current element into min heap.
	- nums[currIndex - k + 1] = minHeap.poll()
- return the nums array.
- **Time Complexity:** O(N log k) when k value is small like k = 2, log 2 = 1 = O(N)
- **Space Complexity:** O(k)
### Reference:![[WhatsApp Image 2025-12-31 at 4.33.38 PM.jpeg]]

![[WhatsApp Image 2025-12-31 at 4.33.38 PM (1).jpeg]]
## Code:

```Java
private int[] findSortedArray(int[] nums, int k){

	if(k == 0) return nums;

	PriorityQueue<Integer> minHeap = new PriorityQueue<>();
	
	int i;
	for(i = 0; i <= k; i++) minHeap.add(nums[i]);
	
	nums[0] = minHeap.poll();

	for(i = k + 1; i < nums.length; i++){
		minHeap.add(nums[i]);
		nums[i - k] = minHeap.poll();
	}
	
	while(!minHeap.isEmpty()) nums[i++] = minHeap.poll();
	
	return nums;
}
```




