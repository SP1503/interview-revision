#revision-1 #revision-2 #revision-3 #revision-4  #NeedRevisit #written-code-on-18-jan 
## Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/351244/assignment/problems/94303?navref=cl_tt_lst_nm

## Understanding:
- Given a min Heap and Queries Q
- If Q[0] = 1, pop min Element
- If Q[0] = 2, Push element into min heap
- Return array represents all the extracted min from the min heap.

## Input and Output:

![[Screenshot 2025-12-23 at 6.47.42 PM.png]]
## Constraints:

![[Screenshot 2025-12-23 at 6.47.57 PM.png]]
## Approach:

### Brute force:
- Iterate the given Query Q
- If Q[0] == 1, pop the element from min heap and add it to list
- If Q[1] == 2, push the element into the min heap
- Once iterating is done return the list
- **Time Complexity:** O(Q * log N) where Q is the size of query
- **Space Complexity:** O(N) min heap space.
#### Insert Element (rise):
- Insert the new element into the min heap array at last.
- let index = minHeap.size() - 1
- iterate until index > 0
- let parent = (index - 1)/2;
- check if minHeap[index] < minHeap[parent]
- If yes, swap index, parent;
- If no, break the loop.
#### Get Min:
- minHeap[0] is the smallest element in min heap.
- The min heap array won't collapse only when we access the last element. Hence overwrite the last element as minHeap[0].
- Call heapify(minHeap, 0);
- return min element
- **Time Complexity:** O(H) height of tree, height of complete binary tree is logN
- **Space Complexity:** O(1)

#### Heapify (sink):
- Iterate the index until 2 * index  + 1 < minHeap.size()
- check the smallest element amount the left and right child
- swap current element with smallest element
- make current = smallest element index.
- Do until iteration exists
- **Time Complexity:** O(H) height of tree, height of complete binary tree is logN
- **Space Complexity:** O(1)

## Code:

### Insert element:

```Java

Rise:
public void insert(List<Integer> minHeap, int element){
	minHeap.add(element);
	
	int curr = minHeap.size() - 1;
	
	while(curr > 0){
		int parent = (curr - 1)/2;
		if(minHeap.get(parent) <= minHeap.get(curr)){
			swap(minHeap, curr, parent);
			curr = parent;
		}
		else break;		
	}
}
```

### Main function:

```Java
private ArrayList<Integer> findQueryRes(ArrayList<ArrayList<Integer>> queries){
	
	ArrayList<Integer> res = new ArrayList<>();
	
	List<Integer> minHeap = new ArrayList<>();
	
	for(ArrayList<Integer> query : queries){
		int oper = query.get(0);
		int data = query.get(1);
		if(oper == 2){
			insert(minHeap, data);
		}
		else if(minHeap.size() > 0){
			res.add(getMin(minHeap));
		}
		else res.add(-1);
	}
	
	return res;
}
```

### Get min():

```Java

public int getMin(List<Integer> minHeap){
	if(minHeap.size() == 0) return -1;
	
	int min = minHeap.get(0);
	
	swap(minHeap, 0, minHeap.size() - 1);
	minHeap.remove(minHeap.size() - 1);
	
	heapify(minHeap, 0);
	
	return min;
}

Sink: 
public void heapify(List<Integer> minHeap, int index){
	while(2 * index + 1 < minHeap.size()){
		
		if(2 * index + 2 >= minHeap.size()){
			// Only left child available
			if(minHeap.get(index) < minHeap.get(2 * index + 1)) break;
			else{
				swap(minHeap, index, 2 * index + 1);
				index = 2 * index + 1;
			}
		}
		else{
			// Both child available
			int min = Integer.min(minHeap.get(2 * index + 1), 
						minHeap.get(2 * index + 2));
			if(minHeap.get(index) < min) break;
			else{
				if(minHeap.get(2 * index + 1) < minHeap.get(2 * index + 2)){
					swap(minHeap, index, 2 * index + 1);
					index = 2 * index + 1;
				}
				else{
					swap(minHeap, index, 2 * index + 2);
					index = 2 * index + 1;
				}
			}
		}
	}
}