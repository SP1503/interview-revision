## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351220/assignment/problems/169?navref=cl_tt_lst_nm
## Understanding:
- Given an array[] heights where height[i] represents the height of ith wall
- Find the largest area that can be formed with the wall
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- find all the combinations of different i and j
- find the area that can be formed with given i and j
- find the max out of it.
- Complexity:
	- Time Complexity: O(N * N)
	- Space Complexity : O(1)
### Optimised Approach:
- We can use two pointer here.
- To increase the area we need to increase the height or increase the width.
- let say l = 0, r = n - 1 Now the width is high
	- find the area = Math.min(height[r], height[l]) * r - l;
- Now we need to reduce the weight, we can have max height
### Reference:![[WhatsApp Image 2026-03-06 at 10.25.21 AM.jpeg]]
### Code
```Java

private int findMaxArea(int[] ele){
	int l = 0;
	int r = ele.length - 1;
	int maxArea = 0;
	
	while(l < r){
		maxArea = Math.max(maxArea, r - l * Math.min(height[l], height[r]));
		if(height[l] < height[r]) l++;
		else if(height[l] > height[r]) r--;
		else {
			l++;
			r--;
		}
	}
	
	return maxArea;
}

```
