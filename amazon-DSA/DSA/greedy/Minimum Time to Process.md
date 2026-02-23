## Problem link:
- https://leetcode.com/problems/minimum-processing-time/description/
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private int findMinTime(List<Integer> proTime, List<Integer> tasks){
	
	int maxTime = 0;
	
	Collections.sort(tasks);
	Collections.sort(proTime, (a, b) -> b - a);
	
	for(int i = 0; i < tasks.size(); i++)
		maxTime = Integer.max(maxTime, proTime.get(i / 4) + tasks.get(i));
	
	return maxTime;
}

Time Complexity: O(N log N) + O(M Log M)
Space Complexity: O(1)

```


