## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351220/assignment/problems/317?navref=cl_tt_lst_nm
## Understanding:
- Given an string s
- Find its sorted permutation rank
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Generate all the permutations in sorted order and find its index
- return index + 1;
- Complexity:
	- Time Complexity: O(N!)
	- Space Complexity: O(N!)
### Optimised Approach:
- **Intuition**: Literally we need to find number of permutations that can be possible and lexicographically less than current string.
- For every element at given index i, find the number of elements from i + 1 to n -1 which is less than given element
- Find number of permutations that is possible from i + 1 to N - 1.
- Find its product and add to rank
- Return rank + 1.
- Complexity:
	- Time Complexity: O(N * N)
	- Space Complexity : O(1)
### Reference:
![[WhatsApp Image 2026-03-05 at 5.13.59 PM.jpeg]]
### Code
```Java

#Finding the lexicographcally sorted rank

// Time Complexity: O(N * N)
// Space Complexity: O(1)
private int findSortedRank(String s){
	
	int len = s.length();
	
	int[] fact = findFact(len);4
	
	int rank = 0;
	
	for(int i = 0; i < len; i++){
		
		int noOfSmallChar = 0;
		for(int j = i + 1; j < len; j++){
			if(s.charAt(i) > s.charAt(j)) noOfSmallChar++;
		}
		
		int remEleToFill = len - i - 1;
		rank += (noOfSmallChar * fact[remEleToFill]); 
	}
	
	return rank + 1;
}

private int[] findFact(int n){
	int[] fact = new int[n];
	
	fact[0] = 1;
	fact[1] = 1;
	
	for(int i = 2; i < n; i++) fact[i] = i * fact[i - 1];
	
	return fact;
}
```

