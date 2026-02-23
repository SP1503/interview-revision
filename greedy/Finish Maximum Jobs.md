#revision-1 #revision-2 #revision-3   #CanBeImplementedWithoutRevisit 
## Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/351250/assignment/problems/9291?navref=cl_tt_lst_nm
## Understanding:
- Given two integer lists represents start and end time of N jobs
- Find the maximum number of jobs that we can do with given Jobs.
## Input and Output:
![[Screenshot 2025-12-25 at 9.52.17 AM.png]]
## Problem Constraints:
![[Screenshot 2025-12-25 at 9.52.39 AM.png]]
## Approach
### Brute Force:
#### Observation:
![[WhatsApp Image 2025-12-25 at 10.00.27 AM.jpeg]]

#### Approach:
- Sort two lists based on B;
- Initialise jobCount = 1, currJobEnd = B[0];
- Iterate the list from i to N
	- if(A[i] >= currJobEnd)
		- jobCount++
		- currJobEnd = B[i];
- return jobCount;
- **Time Complexity:**
	- Sort the given list based on end time - O(N Log N)
	- Iterate the time slot - O(N)
		- if possible do the job and increment the count 
	- Finally the time complexity: O(N Log N)
- **Space Complexity:** No space used: Hence O(1).
#### Code:
```Java 
public int findJobCount(
	ArrayList<Integer> startTime, 
	ArrayList<Integer> endTime){
	
	sortBasedOnEndTime(startTime, endTime);
	
	int jobCount = 1;
	int currJobEnd = endTime.get(0);
	
	for(int i = 1; i < endTime.size(); i++){
		if(currJobEnd >= starTime.get(i)){
			jobCount+++;
			currJobEnd = endTime.get(i);
		}
	}
	
	return jobCount;
}
```
