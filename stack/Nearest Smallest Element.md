## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351239/assignment/problems/332/?navref=cl_pb_nv_tb
## Understanding:
- Given an array A with N integers
- Find the nearest smallest integer at left of the current element
## Input and Output:![[Screenshot 2026-02-06 at 6.03.46 PM.png]]
![[Screenshot 2026-02-06 at 6.04.07 PM.png]]
## Problem Constraints:
![[Screenshot 2026-02-06 at 6.08.23 PM.png]]
## Approach:
### Brute Force:
- Idea: for any current element from the array
	- If the stack peek element is > curr then it must be greater for future coming elements.
	- Hence we can pop that
	- We can follow the same until stack becomes empty for current element.
- If stack is empty there is no nearest smallest left element.
- If stack have element that is the nearest smallest left element of current add it to the answer.
- **Complexity**:
	- **Time Complexity**: O(N) Every element is added and removed from stack at most once
	- **Space Complexity**: O(N) Every element will be added in descending array
### Reference:
### Code
```Java

private ArrayList<Integer> findNearestSmallestLeft(ArrayList<Integer> A){
	Stack<Integer> stack = new Stack<>();
	ArrayList<Integer> ans = new ArrayList<>();
	for(int i = 0; i < A.size(); i++){
		while(!stack.isEmpty() && A.get(stack.peek()) >= A.get(i)) stack.pop();
		if(stack.isEmpty()) ans.add(-1);
		else ans.add(A.get(stack.peek()));
		stack.push(i);
	}
	return ans;
}

```

