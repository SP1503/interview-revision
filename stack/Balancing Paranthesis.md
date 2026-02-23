## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351239/assignment/problems/678/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- String A contains open and closed braces.
- To return:
	- Check whether the given expression is balanced.
## Input and Output:
![[Screenshot 2026-02-06 at 5.21.22 PM.png]]

## Problem Constraints:
## Approach:
### Brute Force:
- Idea:
	- All the latest type open braces should be closed first.
	- All the open braces should have its corresponding closed braces.
- Iterate the given string
	- Check if the current ch is a open braces.
	- If yes, add it to the stack.
	- If no check then latest open braces stored in the stack is of same type compared to the current closing braces.
		- If no return false.
		- If yes pop that open braces from the stack.
- At the end of the iteration check if stack is empty.
	- If yes, return true balanced.
	- If no return false.
- **Complexity**:
	- **Time Complexity:** O(N) processing every element one time.
	- **Space Complexity:** O(N) adding all the elements into stack is all the given character is a open braces.
### Reference:![[WhatsApp Image 2026-02-06 at 5.25.30 PM.jpeg]]

### Code
```Java

private boolean isBalanced(String A){
	Stack<Character> stack = new Stack<>();
	for(char ch : A.toCharArray()){
		if(isOpen(ch)) stack.push(ch);
		else{
			if(!stack.isEmpty() && isEqual(ch, stack.peek())) stack.pop();
			else return false;
		}
	}
	return stack.isEmpty();
}

private boolean isOpen(Character ch){
	return ch == '{' || ch == '[' || ch == '(';
}

private boolean isEqual(Character ch, Character open){
	switch(ch){
		case '}':
		return open == '{';
		case ']':
		return open == '[';
		default:
		return open == '(';
	}
}

```

