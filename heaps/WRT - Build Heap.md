#NeedRevisit  #revision-1 #revision-2 #revision-3 #written-code-on-18-jan
Code became easy once Heap querie problem is done.
### Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/351244/assignment/problems/90663?navref=cl_tt_lst_nm

### Understanding:
- Given an array of integers
- Convert the array into min heap and return the array
### Input and Output:

![[Screenshot 2025-12-24 at 6.31.21 AM.png]]
### Constraints:

![[Screenshot 2025-12-24 at 6.31.39 AM.png]]
### Approach:
#### Brute Force: 
- sort the given array, A sorted array is always a min heap.
- **Time complexity:** O(N Log N)
- **Space Complexity:** O(1)
#### Brute Force 2:
- Insert the given elements into min heap repeatedly.
- **Time complexity:** O(N Log N)
- **Space Complexity:** O(1), using the given array as heap considering index as a end index of heap.
#### Optimised:
- Iterate the given elements from n/2 to 0
- For every element do heapify(array, index)
- Return the array.
- **Time Complexity:** O(N)
- **Space Complexity:** O(1)
#### Code:

```Java

public int[] buildMinHeap(int[] A){
	
	for(int i = n / 2; i <= 0; i--){
		heapify(A, i);
	}
	
	return A;
}

public void heapify(int[] A, int index){
	if(A.length == 1) return;
	
	while( 2 * index + 1 < A.length){
		// Check if it has both left and right chilf or only left
		if(2 * index + 2 >= A.length){
			if(A[index] < A[2 * index + 1]) return;
			else{
				swap(A, index, 2 * index + 1);
				index = 2 * index + 1;
			}
		}
		else{
			int minEle = Integer.min(A[2 * index + 1], A[2 * index + 2]);
			
			if(A[index] < minEle) return;
			else{
				if(A[2 * index + 1] < A[2 * index + 2]){
					swap(A, index, 2 * index + 1);
					index = 2 * index + 1;
				}
				else{
					swap(A, index, 2 * index + 2);
					index = 2 * index + 2;
				}
			}
		}
	}
}
```


#### Complexity Analysis:


![[WhatsApp Image 2025-12-24 at 6.57.27 AM.jpeg]]
![[WhatsApp Image 2025-12-24 at 6.57.26 AM.jpeg]]