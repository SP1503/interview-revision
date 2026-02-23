## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351239/assignment/problems/1064/?navref=cl_pb_nv_tb
## Understanding:
- **Given**:
	- Number of passes A after which we need to find the ball possession.
	- B represents the person id from whom the ball passing starts.
	- C represents the array that holds the A passes history.
- **To Return**:
	- Find the id of the person who posses the ball after A possesion.
## Input and Output:
![[Screenshot 2026-02-06 at 5.04.56 PM.png]]
![[Screenshot 2026-02-06 at 5.05.11 PM.png]]
![[Screenshot 2026-02-06 at 5.05.32 PM.png]]
## Problem Constraints:
## Approach:
### Brute Force:
- Idea: If we encounter 0 in the posession array we need to know from whom the ball passed from. Hence we need to know the passing history. hence we need a stack to store the same.
- Iterate the given array until A elements.
	- Check if current element = 0
		- If yes, pop the latest passer
		- If no add the current passer to the stack.
- Return the peek element present in the stack.
- **Complexity**:
	- **Time Complexity**: O(N)
	- **Space Complexity**:O(N)
### Reference:![[WhatsApp Image 2026-02-06 at 5.09.43 PM.jpeg]]
### Code
```Java

private int findCurrBallPoss(int A, int B, ArrayList<Integer> passes){
	Stack<Integer> passHistory = new Stack<>();
	passHistory.push(B);
	
	int passingCount = 0;
	int index = 0;
	
	while(passingCount < A){
		// Is the current pass is reverse pop
		if(passes.get(index) == 0) passHistory.pop();
		else passHistory.push(passes.get(index));
		
		index++;
		passingCount++;
	}
	
	return passHistory.peek();
}

```
