#revision-1 #revision-2 #revision-3 #revision-4 #written-code-on-18-jan  #CanBeImplementedWithoutRevisit 
## Problem Link:
https://leetcode.com/problems/find-median-from-data-stream/description/
## Understanding:
- Given an array of integers treated as stream of integer
- For every stream coming in, we need to find the median for current available elements
## Input and Output:
![[Screenshot 2025-12-25 at 9.01.04 AM.png]]
## Problem Constraints:
![[Screenshot 2025-12-25 at 9.01.47 AM.png]]
## Approach:

### Brute Force: 
- Create an array list.
- Insert the element one by one into the list
- Sort the given list
- Find the median of the sorted list
- Add that median into the result arrayList.
- **Time Complexity:**
	- For every element coming in
		- We add it into the list, O(1)
		- Sort the list O(N Log N)
		- Find the median O(1)
	- This process we are doing for N element. Hence TC: O(N * N log n) = O(N ^ 2 log n)
- **Space Complexity:** O(N)
### Optimisation 1:
- Since we know for i th element we already sorted the available list for i-1 th element. Hence we don't need to entirely sort the array instead insert the current ith element into the sorted array using insertion sort.
- For every element comes in
	- Find the index to be inserted 
	- Insert the element
	- Find median
	- Add to result.
- **Time Complexity:**
	- We are doing N time below process
		- Find insertion position O(N)
		- Insert element into index O(1)
		- Find median O(1)
	- Hence the TC: O(N ^2)
- **Space Complexity:** O(N)
### Optimisation 2:
![[WhatsApp Image 2025-12-25 at 9.09.03 AM.jpeg]]
#### Observation:
- Here we need repeated sorting, Can we use heaps here.
- Considering first half of the array in max heap
- Second half of the array is in min heap.
- The median is always the max of first half and min of second half.

#### Approach:
- For every element
	- If A[i] is <= maxHeap.peek() then add to max heap
	- Else add to min Heap
	- Check if both the heap is differed in size only by one.
	- If yes find the median
	- If no then balance and find the median
## Code:
```Java

public ArrayList<Integer> findMedian(ArrayList<Integer> stream){
	PriorityQueue<Integer> minHeap = new PriorityQueue<>();
	PriorityQueue<Integer> maxHeap = 
		new PriorityQueue<>(Collections.reverseOrder());
	
	List<Integer> medianList = new ArrayList<>();
	
	for(Integer ele : stream){
		// insert into the appropriate heap
		if(ele > maxHeap.peek()) minHeap.add(ele);
		else maxHeap.add(ele);
		
		// balance the heap
		int maxSize = maxHeap.size();
		int minSize = minHeap.size();
		if(maxSize - minSize > 1) minHeap.add(maxHeap.poll());
		else if(maxSize - minSize < 0) maxHeap.add(minHeap.poll());
		
		// find the median
		if(maxHeap.size() == minHeap.size()) 
			medianList.add((maxHeap.peek() + minHeap.peek()) / 2.0);
		else 
			medianList.add(maxHeap.peek() / 1.0);
	}
	
	return medianList;
}
```


