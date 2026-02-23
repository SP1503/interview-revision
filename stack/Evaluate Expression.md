## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351239/assignment/problems/46/?navref=cl_pb_nv_tb
## Understanding:
- Given a list of string represents operands and operators.
- Find the result of the expression.
## Input and Output:
![[Screenshot 2026-02-06 at 5.46.53 PM.png]]
![[Screenshot 2026-02-06 at 5.47.09 PM.png]]
## Problem Constraints:
## Approach:
### Brute Force:
- Idea: 
	- While encountering an operand we need to know what are all the operators that are passed before. Hence we need a stack.
- Iterate the given string
	- Check if the string is a operator
		- if yes, pop the last 2 operands and compute the arithmetic result and store in stack.
		- If no, put the operand in stack.
- Return the last value stored in stack.
- **Complexity:**
	- **Time Complexity:** O(N) At most every element is processed at max once and will be pop'ed from stack at max once.
	- **Space Complexity:** O(N) adding every element into stack from at most once.
### Reference:![[WhatsApp Image 2026-02-06 at 5.53.38 PM.jpeg]]
### Code
```Java

private int findResult(ArrayList<String> A){
	Stack<Integer> stack = new Stack<>();
	for(String str : A){
		if(!isOperand(str)) stack.push(Integer.parseInt(str));
		else{
			int operand2 = stack.pop();
			int operand1 = stack.pop();
			int result = computeOperation(operand1, operand2, str);
			stack.push(result);
		}
	}
	return stack.peek();
}

private boolean isOperand(String str){
	return str.equals("+") 
	|| str.equals("-") 
	|| str.equals("*") 
	|| str.equals("/");
}

private int computeOperation(int oprnd1, int oprnd2, String ch){
	switch(ch){
		case "+": return oprnd1 + oprnd2;
		case "-": return oprnd1 - oprnd2;
		case "*": return oprnd1 * oprnd2;
		default : return oprnd1 / oprnd2;
	}
}

```
