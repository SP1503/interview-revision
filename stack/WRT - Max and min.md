## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351223/assignment/problems/7042/?navref=cl_pb_nv_tb
## Understanding:
- Given
	- An integer[] arr
- To return:
	- Sum of the difference between max and min of all the subarrays that can be formed from the array arr.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Generate all the subarrays.
- Find min and max out of it.
- Find difference and sum it.
- Complexity: 
	- Time Complexity: O(N * N)
	- Space Complexity: O(1)
### Optimised Approach:
- For every given array A[i] can we compute how much time A[i] contributed as max and A[i] contributed as min with this can we able to find.
- For every i
	- Find subarray range where the A[i] is part of as max
		- Find the number of start index count
		- Find the number of end index count
		- Start index * end index gives number of subarrays where the A[i] is max
	-  Find subarray range where the A[i] is part of as min
		- Find the number of start index count
		- Find the number of end index count
		- Start index * end index gives number of subarrays where the A[i] is min
	- Sum += A[i] * (max - min)
- return sum;
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(N)
### Reference:
### Code
```Java

// Time Complexity: O(N)
// Space Complexity: O(N)
private int findSumOfMaxAdnMinDiff(int[] ele){
	
	int len = ele.length;
	int sum = 0;
	
	int[] startAsMax = findNearestGreatestFromLeft(ele);
	int[] endAsMax = findNearestGreatestFromRight(ele);
	int[] startAsMin = findNearestSmallestFromLeft(ele);
	int[] endAsMin = findNearestSmallestFromRight(ele);
	
	for(int i = 0; i < len; i++){
		int maxCont = findMaxCont(ele, i, startAsMax, endAsMax);
		int minCont = findMinCont(ele, i, startAsMin, endAsMin);
		
		sum += (ele[i] * (maxCont - minCont));
	}
	
	return sum;
}

private int[] findNearestSmallestFromLeft(int[] ele){
	
	int n = ele.length;
	int[] NSL = new int[n];
	
	Stack<Integer> stack = new Stack<>();
	
	NSL[0] = -1;
	
	stack.push(0);
	
	for(int i = 1; i < n; i++){
		
		while(!stack.isEmpty() && ele[stack.peek()] >= ele[i]) stack.pop();
		
		if(stack.isEmpty()) ans[i] = -1;
		else NSL[i] = stack.peek();
		
		stack.push(i);
	}
	
	return NSL;
}

private int[] findNearestGreatestFromLeft(int[] ele){
	
	int n = ele.length;
	int[] NGL = new int[n];
	
	Stack<Integer> stack = new Stack<>();
	
	NSL[0] = -1;
	
	stack.push(0);
	
	for(int i = 1; i < n; i++){
		
		while(!stack.isEmpty() && ele[stack.peek()] <= ele[i]) stack.pop();
		
		if(stack.isEmpty()) ans[i] = -1;
		else NGL[i] = stack.peek();
		
		stack.push(i);
	}
	
	return NGL;
}

private int[] findNearestSmallestFromRight(int[] ele){
	
	int n = ele.length;
	int[] NSR = new int[n];
	
	Stack<Integer> stack = new Stack<>();
	
	NSR[n - 1] = n;
	
	stack.push(n - 1);
	
	for(int i = n - 1; i >= 0; i--){
		
		while(!stack.isEmpty() && ele[stack.peek()] >= ele[i]) stack.pop();
		
		if(stack.isEmpty()) NSR[i] = n;
		else NSR[i] = stack.peek();
		
		stack.push(i);
	}
	
	return NSR;
}

private int[] findNearestGreatestFromRight(int[] ele){
	
	int n = ele.length;
	int[] NGR = new int[n];
	
	Stack<Integer> stack = new Stack<>();
	
	NGR[n - 1] = n;
	
	stack.push(n - 1);
	
	for(int i = n - 1; i >= 0; i--){
		
		while(!stack.isEmpty() && ele[stack.peek()] <= ele[i]) stack.pop();
		
		if(stack.isEmpty()) NSR[i] = n;
		else NGR[i] = stack.peek();
		
		stack.push(i);
	}
	
	return NGR;
}

private int findMaxCont(int[] ele, int ind, int[] startAsMax, int[] endAsMax){
	// Find subarray range where the element is contributing as max
	int startCount = ind - startAsMax[ind];
	int endCount = endAsMax[ind] - ind;
	
	// Total number of subarrays where ele[ind] is max
	return startCount * endCount;
}

private int findMinCont(int[] ele, int ind, int[] startAsMin, int[] endAsMin){
	int startCount = ind - startAsMin[ind];
	int endCount = endAsMin[ind] - ind;
	
	// Total number of subarrays where ele[ind] is min
	return startCount * endCount;
}

```


