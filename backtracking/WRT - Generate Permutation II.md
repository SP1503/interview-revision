## Problem link:
- https://leetcode.com/problems/permutations-ii/
## Understanding:
- Given:
	- String with upper case characters
- To return:
	- find possible permutations without duplicates.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- We can generate all the permutations and store it is a set and return it.
- Set will remove all the duplicate elements.
- Time Complexity: O(N!)
- Space Complexity: O(N)
### Optimised Approach:
- Why the duplicate occurs
	- Let say here my role is to put N places with given N characters
	- If at i th place I put char A again then duplicates is non avoidable.
	- Hence at every level we are filling levelth palce of the permutation, we can avoid adding same value again. using hashing.
	- We can hash all the char as key with its frequency.
	- At every level consume the key only once.
	- this not only gives the permutation without duplicates but also gives the permutations in lexicographically sorted order.
- Time Complexity: O(N!)
- Space Complexity: O(N)
### Reference:![[WhatsApp Image 2026-02-24 at 8.44.33 AM.jpeg]]
![[WhatsApp Image 2026-02-24 at 8.44.33 AM (1) 1.jpeg]]
![[WhatsApp Image 2026-02-24 at 8.44.34 AM.jpeg]]
### Code
```Java

private List<List<Integer>> possPer = new ArrayList<>();

public List<List<Integer>> permuteUnique(int[] nums) {
	int[] hash = new int[21];
	for(int ele : nums) hash[ele + 10]++;
	findPossPermutations(hash, new ArrayList<>(), nums.length);
	return possPer;
}

// Time Complexity: O(N! * N)
// Space Complexity: O(N) Atmax N = 8
private void findPossPermutations(int[] hash, List<Integer> perm, int n){
	// Base condition
	if(perm.size() == n){
		possPer.add(perm);
		return;
	}
	// Recurrence Relation
	for(int i = 0; i < 21; i++){
		if(hash[i] == 0) continue;
		else{
			hash[i]--;
			perm.add(i - 10);
			findPossPermutations(hash, new ArrayList<>(perm), n);
			// Backtracking undo changes
			perm.remove(perm.size() - 1);
			hash[i]++;
		}
	}
}
```

