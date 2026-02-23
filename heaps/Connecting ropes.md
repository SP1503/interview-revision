#revision-1 #revision-2  #revision-3 #revision-4  #StratightForward #CanBeImplementedWithoutRevisit
## Problem Link: 
https://www.scaler.com/academy/mentee-dashboard/class/351244/assignment/problems/4385?navref=cl_tt_lst_nm

## Understanding:
- Given array represents the ropes length
- Cost of connecting = length of rope A + length pf rope B
- Find the minimum amount required to connect all ropes

## Input and Output:

![[Screenshot 2025-12-23 at 5.25.02 PM.png]]

## Constraints:

![[Screenshot 2025-12-23 at 5.26.17 PM.png]]
 
## Approach:

### Brute Force:

- If the cost needs to be low then, the rope length that we are giving needs to be low.
- Sort the given array
- Find first two elements and connect it.
- Insert the connected rope into the array using insertion sort.
- Do this until the array length becomes 1
- return the final length
- **Time complexity:** O(N * logN + N * N) = O(N Log N)
- **Space complexity:** O(1)

## Optimised:

- Here repeated sorting is required.
- Hence we can use hashing here, were sorted order is managed.
- **Time complexity:**  O(N log N)
- **Space Complexity:** O(N)

## Code:

```Java

private int findMinCostToJoinRopes(ArrayList<Integer> ropes){

	int costToJoin = 0;
	
	PriorityQueue<Integer> ropeOrganiser = new PriorityQueue<>();
	
	for(Integer rope : ropes){
	
		ropeOrganiser.add(rope);
	
	}
	
	while(ropeOrganiser.size() > 1){
	
		int minSizedRope1 = ropeOrganiser.remove();
		
		int minSizedRope2 = ropeOrganiser.remove();
		
		int joinedRope = minSizedRope1 + minSizedRope2;  
		
		costToJoin += joinedRope;
		
		ropeOrganiser.add(joinedRope);
	}
	
	return costToJoin;
}
```

