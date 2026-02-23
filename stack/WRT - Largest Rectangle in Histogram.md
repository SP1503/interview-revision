## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351223/assignment/problems/49?navref=cl_tt_lst_nm
## Understanding:
- **Given**:
	- Heights of  N bars that is placed in a histogram
- **To Return**:
	- Find the max rectangle that we can make using the bars that we have
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Find all the subarrays
- For each subarray find its min height and its width.
- Compare with max = Math.max(max, height * width)
- Return the answer.
- Complexity:
	- Time Complexity: O(N * N * N) and can be optimised to O(N * N) as we need to find the subarray
	- Space Complexity: O(1)
### Optimised Approach:
- Determine the height
- Find the possible width for the current determined height
- Find max = Math.max(max, height * width);
- return max
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(N)
### Reference:
### Code
```Java

// Brute Force
private int findMaxRect(int[] heights){
	int n = heights.length;
	int maxRect = 0;
	for(int i = 0; i < n; i++){
		for(int j = 0; j < n; j++){
			int minHeight = Integer.MAX_VALUE;
			for(int k = i; k <= j; k++){
				minHeight = Integer.min(minHeight, heights[k]);
			}
			
		}
	}
	return maxRect;
}

// Can be optimised used contribution tech
private int findMaxRect(int[] heights){
	int n = heights.length;
	int maxRect = 0;
	for(int i = 0; i < n; i++){
		int minHeight = heights[i];
		for(int j = 0; j < n; j++){
			minHeight = Integer.min(minHeight, heights[j]);
			maxRect = Math.max(maxRect, minHeight * (j - i + 1))
		}
	}
	return maxRect;
}

// The above cnanot be optimised further as we are finding min height for 
// different width and finding the max rect possible. Possiblity of getting 
// different width is O(N * N)

// Instead of determining width and finding poss height, we can determine height and find poss width.


// Time Complexity: O(N)
// Space Complexity: O(N)
private int findMaxRect(int[] heights){
	
	int n = heights.length;
	int[] NSL = findNearestSmallestLeft(heights);
	int[] NSR = findNearestSmallestRight(heights);
	
	int maxRect = 0;
	
	for(int i = 0; i < n; i++){
		int exclusiveLeft = NSL[i];
		int exclusiveRight = NSR[i];
		maxRect = Math.max(maxRect, 
			heights[i] * (exclusiveRight - exclusiveLeft - 1));	
	}
	
	return maxRect;
}

private int[] findNearestSmallestLeft(int[] heights){
	int n = heights.length;
	int[] NSL = new int[n];
	
	Stack<Integer> stack = new Stack<>();
	
	NSL[0] = -1;
	stack.push(0);
	
	for(int i = 1; i < n; i++){
		while(!stack.isEmpty() && heights[stack.peek()] >= heights[i])
			stack.pop();
		if(stack.isEmpty()) NSL[i] = -1;
		else NSL[i] = stack.peek();
		
		stack.push(i); 
	}
	
	return NSL;
}

private int[] findNearestSmallestRight(int[] heights){
	int n = heights.length;
	int[] NSR = new int[n];
	
	Stack<Integer> stack = new Stack<>();
	
	NSR[n - 1] = n;
	stack.push(n - 1);
	
	for(int i = n - 2; i >= 0; i--){
		while(!stack.isEmpty() && heights[stack.peek()] >= heights[i])
			stack.pop();
		if(stack.isEmpty()) NSR[i] = n;
		else NSR[i] = stack.peek();
		
		stack.push(i); 
	}
	
	return NSR;
}

```


