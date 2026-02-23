## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private List<List<Integer>> subsets = new ArrayList<>();
  
public List<List<Integer>> subsetsWithDup(int[] nums) {
	Arrays.sort(nums);
	findSubsets(nums, 0, new ArrayList<>());
	return subsets;
}

private void findSubsets(int[] nums, int index, List<Integer> subset){
	// Base Condition
	if(index == nums.length){
		subsets.add(subset);
		return;
	}
	else{
		// Recurrence Relation
		
		// Include
		subset.add(nums[index]);
		findSubsets(nums, index + 1, new ArrayList<>(subset));
		subset.remove(subset.size() - 1);
		
		// Exclude
		int ind = index + 1;
		// Duplicates occur when in branch excluding x + including y 
		// where x == y, for duplicate value we have only exclude option under
		// exclude branch 
		while(ind < nums.length && nums[ind] == nums[ind - 1]) ind++;
		findSubsets(nums, ind, new ArrayList<>(subset));
	}
}

```
