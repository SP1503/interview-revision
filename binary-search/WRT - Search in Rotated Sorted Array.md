## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351231/assignment/problems/203?navref=cl_tt_lst_nm
## Understanding:
- Given a sorted array which is rotated at some point
- Find the index of the given target
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Have the picot element as first element of the given array
- As the array will be rotated all the elements after some point will be less than pivot element.
- So we need to find the p1 and p2 part of the given array.
### Optimised Approach:
- What are all the cases we can meet with mid and target
	- mid is in p1 and target is in p2
		- mid >= pivot and target < pivot 
			- Move left l = mid + 1
	- mid is in p2 and target is in p1
		- mid < pivot and target >= pivot
			- Move right r = mid - 1
	- mid is in p1 and target is in p1
		- Both in same 
			- if(A[mid] > target) Move left r = mid - 1;
			- else l = mid + 1
	- mid is in p2 and target is in p2
		- Both in same 
			- if(A[mid] > target) Move left r = mid - 1;
			- else l = mid + 1
- Complexity:
	- Time Complexity: O(log N)
	- Space Complexity: O(1)
### Reference:![[WhatsApp Image 2026-02-16 at 7.46.43 AM.jpeg]]
### Code
```Java

// Brute Force
private int findTarget(int[] ele, int target){
	
	int start = findStart(ele);
	int pivot = ele[0];
	
	// Not rotated case
	if(start == -1) return findElement(0, ele.length - 1, ele, target);
	else if(target >= pivot) return findElement(0, start - 1, ele, target);
	else return findElement(start, ele.length - 1, ele, target);
}

private int findElement(int l, int r, int[] ele, int target){
	
	while(l <= r){
		
		int mid = l + (r - l)/2;
		
		if(ele[mid] == target) return mid;
		else if(ele[mid] > target) r = mid - 1;
		else l = mid + 1;
	}
	
	return -1;
}

private int findStart(int[] ele){
	
	// Finding the starting point
	// Search Space
	int l = 0;
	int r = ele.length - 1;
	int pivot = ele[0];
	int start = -1;
	
	while(l <= r){
		
		// Make the guess
		int mid = l + (r - l) / 2;
		
		if(ele[mid] < ele[mid - 1] && ele[mid] < ele[mid + 1]) return mid;
		
		// Based on decision reduce the search space
		if(ele[mid] < pivot){
			start = mid;
			r = mid - 1;
		}
		else{
			l = mid + 1;
		}
	}
	
	return start;
}

// Optimised Approach
private int findTarget(List<Integer> A, int tar){
	// Define search space
	int l = 0;
	int r = A.size() - 1;
	int pivot = A.get(0);
	while(l <= r){
		
		// Making the guess
		int mid = l + (r - l)/2;
		if(A.get(mid) == tar) return mid;
		
		// both mid and target is in same part either p1 or p2
		else if(((tar >= pivot) && (A.get(mid) >= pivot)) 
			|| ((tar < pivot) && (A.get(mid) < pivot))) {
			if(tar > A.get(mid)) l = mid + 1;
			else r = mid - 1;
		}
		// tar is in p1 nad mid is in p2
		else if(tar >= pivot && A.get(mid) < pivot) r = mid - 1;
		else l = mid + 1;
	}
	return -1;
}

```
