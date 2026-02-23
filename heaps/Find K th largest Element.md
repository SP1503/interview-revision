#revision-1 #revision-2 #revision-3 #revision-4  #CanBeImplementedWithoutRevisit  #written-code-on-18-jan 
## Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/351255/assignment/problems/989?navref=cl_tt_lst_nm
## Understanding:
- Given an array of integer nums
- Given K
- Find the Kth largest element for every window of size k from index 0.
## Input and Output:
![[Screenshot 2025-12-29 at 12.28.14 PM.png]]
## Problem Constraints:
![[Screenshot 2025-12-29 at 12.28.30 PM.png]]

## Approach:

### Brute Force:
- Generate all the subarray of size k
- In every subarray find the k th largest element
- store it in a array
- Return array
- Time Complexity: Generating subarray = O(N * N) * find Kth largest O(N) = O(N * N * N)
- Space Complexity: O(N) to store K elements to find the kth largest
### Optimal Solution: 
- **Observation:** For array with size k the first smallest is the kth largest element. Example [1, 2, 3] k = 3. Kth largest = 1
- Have a sliding window of size k
- Iterate the given array from o to n
	- If minHeap.size() < k minHeap.add(nums[i])
	- else if nums[i] > minHeap.peek() (The largest value will change only when the more largest value enters into the heap)
		- minHeap.poll();
		- minHeap.add(nums[i])
- **Time Complexity:** O(N * log k)
- **Space Complexity:** O(k)
## Code:

```Java 
private ArrayList<Integer> findKthLargest(ArrayList<Integer> nums, int k){
	
	List<Integer> kthLargest = new ArrayList<>();
	
	PriorityQueue<Integer> minHeap = new PriorityQueue<>();
	
	for(int i = 0; i < k; i++){
		minHeap.add(nums[i]);
	}
	
	kthLargest.add(minHeap.peek());
	
	for(int i = k; i < nums.length; i++){
		if(nums[i] > minHeap.peek()){
			minHeap.poll();
			minHeap.add(nums[i]);
		}
		
		kthLargest.add(minHeap.poll());
	}
	
	return kthLargest;
} 
```


