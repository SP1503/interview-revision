#NeedRevisit #revision-1 #CanBeImplementedWithoutRevisit 
## Problem Link:
https://leetcode.com/problems/sort-an-array/description/
## Understanding:
- Given an array of integers
- Sort the integer array in ascending order using smallest space possible
## Input and Output:

![[Screenshot 2025-12-24 at 6.35.59 PM.png]]

## Problem Constrains:

![[Screenshot 2025-12-24 at 6.36.12 PM.png]]
## Approach:

### Brute Force: (Using Heap sort)
- Build a min heap using the input array
- Create a new array to store min values from min heap created.
- Iterate for element count
- Get min element from heap repeatedly and populate it new array using index
- return the new array
- **Time Complexity:** 
	- O(N) to build a min heap using heapify
	- O(N Log N) to get min element from heap repeatedly for N elements. 
	- Finally it is O(N log N)
- **Space Complexity:** O(N) storing the min element from heap. We need this array to store the min elements from heap.

### Optimal: (Using Heap sort with optimised memory)
- The time complexity cannot be reduced as we are using heap here, But space complexity can be reduced by reusing the same heap array instead of creating a new one.
- Build a max Heap(Planning to reuse the last element space from max heap, So last element should have max element in it. Hence building max heap)
- Initialise endIndex = nums.length - 1 
- Iterate until endIndex > 0
- swap the first element (max element) with the end index element.
- decrement end index (decrement heap size)
- Heapify the new replaced element.
- **Time Complexity:** 
	- O(N) to build max heap.
	- O(N log N) to replace the last element with current max element from max heap.
	- Finally it is O(N Log N)
- **Space Complexity:**
	- O(1) as we are reusing the max heap array to store the ordered elements from max heap.

## Code:
```Java

private void sortArrayImpl(int[] nums){
	// Building max heap as we are using the last deleting element place to store ordered elements
	buildMaxHeap(nums);
	
	// Determine the endIndex of heap
	int end = nums.length - 1;
	
	// Iterate until heap exists
	while(end > 0){
		
		// Swap current max with last index
		swap(nums, 0, end);
		
		// Decrement the heap size
		end--;
		
		// heapify(sink) the newly replaced element
		heapify(nums, 0, end);
	}
} 

private void buildMaxHeap(int[] nums){
	for(int index = nums.length/ 2 - 1; index >= 0; index--){
		heapify(nums, index, nums.length - 1);
	}
}

private void heapify(int[] nums, int index, int end){
	while( 2 * index + 1 <= end){
		if(2 * index + 2 > end){
		// Only left child exists
			if(nums[index] > nums[2 * index + 1]) return;
			else{
				swap(nums, index, 2 * index + 1);
				index = 2 * index + 1;
			}
		}
		else{
		// Both left and right exists
			int maxElement = Integer.max(nums[2 * index + 1]
								, nums[2 * index + 2]);
			if(nums[index] > maxElement) return;
			else{
				if(nums[ 2 * index + 1] > nums[2 * index + 2]){
					swap(nums, index, 2 * index + 1);
					index = 2 * index + 1;
				}
				else{
					swap(nums, index, 2 * index + 2);
					index = 2 * index + 2;
				}
			}
		}
	}
}

private void swap(int[] nums, int a, int b){
	int temp = nums[a];
	nums[a] = nums[b];
	nums[b] = temp;
}
