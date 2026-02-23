## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351223/assignment/problems/968?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- String A
- To return 
	- String without adjacently equal characters
## Input and Output:
## Problem Constraints:
## Approach:
### Optimised Approach:
- We can place the current characters into the stack
	- For every character
		- Check if the stack.top() == current char
			- If yes, pop from stack do this continuously until the stack.top is not equal to current char
			- If No place the char in stack.
- **Complexity**:
	- **Time Complexity**: O(N)
	- **Space Complexity**: O(N)
### Reference:
### Code
```Java

private String findUniqueCharStr(String str){
	
	Stack<Character> stack = new Stack<>();
	
	for(Character ch : str.toCharArray()){
		while(!stack.isEmpty() && stack.peek().equals(ch)) stack.pop();
		
		stack.push(ch);
	}
	
	StringBuilder uniqueStr = new StringBuilder();
	
	while(!stack.isEmpty()) uniqueStr.append(stack.pop());
	
	return uniqueStr.reverse().toString();
}

```


