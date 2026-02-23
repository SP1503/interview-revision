#revision-1 #revision-2 #revision-3  #CanBeImplementedWithoutRevisit 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/313436/assignment/problems/1302/?navref=cl_pb_nv_tb
## Understanding:
- Given an array of integers nums
- Find the min distance between pair whose nums[i] = nums[j], j - i is minimum
## Input and Output:
![[Screenshot 2025-12-31 at 5.00.36 PM.png]]
![[Screenshot 2025-12-31 at 5.00.55 PM.png]]
## Problem Constraints:
![[Screenshot 2025-12-31 at 5.01.22 PM.png]]
## Approach:
### Brute force:
- For every element nums[i] find the nearest element nums[j] whose nums[i] = nums[j] 
- find j - i and compute the minimum
- **Time Complexity:** O(N * N) internal loops to compare nums[i] with nums[j]
- **Space Complexity:** O(1)
### Optimised Approach:
- We can store the elements with its latest index as reference in hash map.
- For every element check if the element is already seen previously
- If yes compare the index j with i present in the hash map
- Compute the minimum
- Store the latest index j in hash map
- **Time complexity:** O(N)
- **Space Complexity:** O(N) hashmap space
### Reference:
![[WhatsApp Image 2025-12-31 at 5.45.44 PM.jpeg]]
## Code:

```Java
private int findMinDistance(int[] nums){
	Map<Integer, Integer> indexHash = new HashMap<>();
	
	for(int i = 0; i < nums.length; i++){
		if(indexHash.containsKey(nums[i])){
			minDist = Integer.min(minDist, i - indexHash.get())
		}
		indexHash.put(nums[i], i);
	}
	
	return minDist;
}
```



