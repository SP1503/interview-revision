## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351225/assignment/problems/50/?navref=cl_pb_nv_tb
## Understanding:
- Given 
	- Array A[]
	- K - represents the window size
- To return
	- Find the max element in every window of size K from A[]
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
- Maintain a Data structure the stores the potential max value for the current and upcoming windows.
- If the DS having element <= current Element then that element will not be future max Element.
### Reference:
### Code
```Java


// Time Complexity: O(N)
// Space Compelxity: O(N)
private int[] findSlidingWindowMax(int[] ele, int k){
	
	int n = ele.length;
	Deque<Integer> wid = new ArrayDeque<>();
	
	int[] maxEle = new int[n - k + 1];
	
	wid.addLast(0);
	
	for(int i = 1; i < k; i++){
		while(!wid.isEmpty() && ele[wid.getLast()] <= ele[i]) wid.removeLast();
		
		wid.addLast(i); 
	}
	
	maxEle[0] = ele[wid.getFirst()];
	
	for(int end = k; end < n; end++){
		
		int remIndex = end - k;
		if(wid.getFirst() == remIndex) wid.removeFirst();
		
		while(!wid.isEmpty() && ele[wid.getLast()] <= ele[i]) wid.removeLast();
	
		wid.addLast(i);
		 
		maxEle[remIndex + 1] = ele[wid.getFirst()];
	}
}

```


