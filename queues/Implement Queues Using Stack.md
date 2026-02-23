## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351225/assignment/problems/11439?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- Instructions to execute on the given queue
- To return
	- The answer a queue will give but implement the queue using stack
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

public static class UserQueue{

	private static Stack<Integer> pushSt = null;
	private static Stack<Integer> popSt = null;
	
	UserQueue(){
		pushSt = new Stack<Integer>();
		popSt = new Stack<Integer>();
	}
	
	static void push(int x){
		pushSt.push(x);
	}
	
	static int pop(){
		if(empty()) return -1;
		
		if(popSt.isEmpty()){
			unLoad();
		}
		return popSt.pop();
	}
	
	static int peek(){
		if(empty()) return -1;
		
		if(popSt.isEmpty()){
			unLoad();
		}
		return popSt.peek();
	}
	
	private static void unLoad(){
		if(pushSt.isEmpty()) return;
		while(!pushSt.isEmpty()) popSt.push(pushSt.pop());
	}
	
	static boolean empty(){
		return pushSt.isEmpty() && popSt.isEmpty();
	}

}

```

