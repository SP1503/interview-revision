## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351231/assignment/problems/57468?navref=cl_tt_lst_nm
## Understanding:4
- Given two sorted arrays
- Find the median of two sorted arrays
## Input and Output:![[Screenshot 2026-02-16 at 8.18.42 AM.png]]
![[Screenshot 2026-02-16 at 8.19.05 AM.png]]![[Screenshot 2026-02-16 at 8.19.14 AM.png]]
## Problem Constraints:
## Approach:
### Brute Force:
- Merge the two sorted arrays into one array which is sorted.
- Find the median of the merged arrays.
- **Complexity:**
	- **Time Complexity:** O(N1 + N2)
	- **Space Complexity:** O(N1 + N2)
### Optimised Approach:
- Median of an even length array will be (max of first half + min of second half) / 2
- Median of odd length array will be max of first half.
- Based on this we can do a binary search that says how much number of elements from array A can be part of first half and remaining will be from first half of B.
### Reference:
### Code
```Java
// Brute Force
private int findMedian(int[] arr1, int[] arr2){
	
	int m = arr1.length;
	int n = arr2.length;
	int[] merged = new int[m + n];
	
	int p1 = 0;
	int p2 = 0;
	int k = 0;
	
	while(p1 < m && p2 < n){
		if(arr1[p1] <= arr2[p2]) merged[k++] = arr1[p1++];
		else merged[k++] = arr2[p2++];
	}
	
	while(p1 < m) merged[k++] = arr1[p1++];
	
	while(p2 < n) merged[k++] = arr2[p2++];
	
	int mergedSize = n + m;
	if(mergedSize % 2 == 0){
		return merged[mergedSize / 2] + merged[mergedSize / 2 - 1];
	}
	else merged[mergedSize / 2];
}

// Optimised
public int solve(int[] A, int[] B) {
// If A length is large then cut at B can become negative
	return A.length > B.length 
			? findMedian(B, A) 
			: findMedian(A, B);
}

private int findMedian(int[] A, int[] B){
	
	// Define the search space
	int m = A.length;
	int n = B.length;
	int minEle = 0;
	int maxEle = m;
	
	while(minEle < maxEle){
		// Make Guess
		int eleFromA = (minEle + maxEle) / 2;
		int eleFromB = (m + n) / 2 - eleFromA; 
		
		// Handle edge case
		int maxFromA = eleFromA == 0 ? Integer.MIN_VALUE : A[eleFromA - 1];
		int maxFromB = eleFromB == 0 ? Integer.MIN_VALUE : B[eleFromB - 1];
		int minFromA = eleFromA == m ? Integer.MAX_VALUE : A[eleFromA];
		int minFromB = eleFromB == n ? Integer.MAX_VALUE : B[eleFromB];
		
		// Check if guess
		if(maxFromA <= minFromB && maxFromB <= minFromA){
			// Is odd
			if((m + n)%2 != 0) return Math.max(maxFromA, maxFromB);
			else 
				return (Math.max(maxFromA, maxFromB) 
				+ Math.min(minFromA, minFromB)) / 2;
		}
		// Based on the guess discard left space or right space
		else if(maxFromA > minFromB) maxEle = eleFromA - 1;
		else minEle = eleFromA + 1;
		// Based on the guess discard left space or right space
	}
}
```




