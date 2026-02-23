#Amazon 
## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code:
```Java

public int[][] employeeFreeTime(int[][] meetings) {
	return findFreeTime(meetings);
}

private int[][] findFreeTime(int[][] meetings){
	Arrays.sort(meetings, (a, b) -> {
		if(a[0] != b[0]) return a[0] - b[0];
		else return a[1] - b[1];
	});

	List<int[]> free = new ArrayList<>();
	int maxEnd = meetings[0][1];
	for(int i = 1; i < meetings.length; i++){
		if(maxEnd >= meetings[i][0]){
			maxEnd = Math.max(maxEnd, meetings[i][1]);
		}
		else{
			free.add(new int[]{maxEnd, meetings[i][0]});
			maxEnd = meetings[i][1];
		}
	}
	
	int[][] freeArray = new int[free.size()][2];
	for(int i = 0; i < free.size(); i++) freeArray[i] = free.get(i);
	return freeArray;
}

```





