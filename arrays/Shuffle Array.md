#Amazon
## Problem link:
- https://leetcode.com/problems/shuffle-an-array/
## Understanding:
- Given an integer array nums
- For every shuffle method call give random arrangement of the given nums
- For reset return the original order.
## Input and Output:
![[Screenshot 2026-02-11 at 9.41.01 PM.png]]
## Problem Constraints:
![[Screenshot 2026-02-11 at 9.41.49 PM.png]]
## Approach:
### Brute Force:
- When ever we are calling Solution constructor, we can initialise the ref array and 
- Generate all the permutations of the given array.
- Have a index = 0;
- For every Shuffle call we can give the possibility one by one and increment the index.
- **Complexity**:
	- **Time Complexity**: O(N!) as to generate permutation we will get N!.
	- **Space Complexity**: O(N!) as there are N! permutations.
### Optimised Approach:
- We can use ==Fisher Yates Algorithm== to generate the permutation of the given nums.
- Algorithms says that
	- Clone the actual array
	- Iterate the given array from last index to index = 0
		- find a random index value that needs to be swapped with index.
		- swap(array, index, random index)
	- return the given array.
- **Complexity**:
	- **Time Complexity**: O(N) for every shuffle call.
	- **Space Complexity**: O(N) as we need new array every time to generate a permutation.
### Reference:
### Code
```Java
class Solution {
	private int[] ref;
	private Random random;
	
	public Solution(int[] nums) {
		this.ref = nums;
		this.random = new Random();
	}
	
	public int[] reset() {
		return ref;
	}
	
	public int[] shuffle() {
		int[] shuffle = ref.clone();
		for(int i = shuffle.length - 1; i >= 0; i--){
			int randomIndex = this.random.nextInt(i + 1);
			swap(shuffle, randomIndex, i);
		}
		return shuffle;
	}
	
	private void swap(int[] shuffle, int a, int b){
		int temp = shuffle[a];
		shuffle[a] = shuffle[b];
		shuffle[b] = temp;
	}

}
```



