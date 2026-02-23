## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351227/assignment/problems/138/submissions
## Understanding:
- Given:
	- List contains integer A
- To return:
	- Find all the permutations of the given list A.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- We are given with N elements in the given list
- We need to fill N places with different elements of A
- At 0 th place have all the N possibilities
- At 1 th place have all the N - 1 possibilities
- So we have N * N! possibilities.
- Complexity:
	- TC: O(N!) * O(N) , N + N * N - 1 + N * N - 1 * N - 2 + .. 1 = N!
	- Space Complexity: O(N)
### Reference:![[WhatsApp Image 2026-02-19 at 12.06.50 PM.jpeg]]
![[WhatsApp Image 2026-02-19 at 12.06.54 PM.jpeg]]
### Code:
```Java

private List<List<Integer>> permutations = new ArrayList<>();


// Time Complexity: Numbe rof function calls * time compelxity of one call
// No of functions: (N) + (N * N - 1) +  (N * N - 1 * N - 2) * .. 1 = N! 
// TC of one call: O(N)
// Total TC: O(N!) * O(N) = O(N!)
// Space Complexity: O(N) recursion space, visited array, permutation list
private void findPerm(
	List<Integer> ele, 
	boolean[] visited, 
	List<Integer> currPer){
	
	// Base Condition
	if(currPer.size() == ele.length){
		permutations.add(currPer);
		return;
	}
	else{
		// Recurrence Relation
		for(int i = 0; i < ele.size(); i++){
			if(!visited[i]){
				currPer.add(ele.get(i));
				visited[i] = true;
				
				findPerm(ele, visited, new ArrayList<>(currPer));
				
				// Back tracking state changes
				currPer.remove(currPer.size() - 1);
				visited[i] = false;
			}
		}
	}
}

```

