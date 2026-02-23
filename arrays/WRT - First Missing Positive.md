## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351624/assignment/problems/65/?navref=cl_pb_nv_tb
## Understanding:
- Given 
	- int[] 
- Return:
	- Find first missing positive
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Sort the array
- Avoid the ele <= 0 
- check if all the elements from 1 to N exists
- If not return that element
- Complexity:
	- Time Complexity: O(N log N)
	- Space Complexity: O(1)
### Optimised Approach:
- Observation: We know that our answer lies between 1 to len + 1
- Hence we can use cyclic sort to sort the given elements and find the element that is != index + 1
- Complexity: O(N)
### Reference:
### Code
```Java

// Time Complexity: O(N)
// Space Complexity: O(1)
private int findFirstMissingPositive(int[] A){
	int len = A.length;
	int index = 0;
	while(index < len){
		int ele = A[index];
		int reqIndex = ele - 1;
		if(ele > 0 && ele <= len && A[reqIndex] != ele) swap(A, index, reqIndex);
		else index++;
	}
	for(int i = 0; i < len; i++){
		if(A[i] != i + 1) return i + 1;
	}
	return len + 1;
} 

private void swap(int[] A, int a, int b){
	int temp = A[a];
	A[a] = A[b];
	A[b] = temp;
}

```

