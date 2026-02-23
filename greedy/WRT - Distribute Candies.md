#NeedRevisit #revision-1 #revision-2  #revision-3 #revision-4 #written-code-on-18-jan 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/351255/assignment/problems/3?navref=cl_tt_lst_nm
## Understanding:
- Given array of integer represents the rank of n children
- We need to distribute candies to those children in such a way that the number of candies is minimum.
- Every children should get at least 1 candy
- Children should have more candies they have rank greater than left or right children.
## Input and Output:
![[Screenshot 2025-12-29 at 1.51.44 PM.png]]

![[Screenshot 2025-12-29 at 1.52.06 PM.png]]
## Problem constraints:
![[Screenshot 2025-12-29 at 1.52.17 PM.png]]
## Approach:
### Brute Force: Greedy thinking
- Every children should get atleast one candy. Distribute one candy to every children
- Iterate from left index 1 to right index n
	- if(rank[i] > rank[i - 1]) chocolate[i]++;
- Iterate from right index N - 2 to left index 0
	- if(rank[index] > rank[index + 1])
		- currentChocolate = chocolate[index];
		- if(chocolate[index] <= chocolate[index + 1]) chocolate[index]++;
- Iterate from 0 to N
	- Add all the chocolates
- return chocolateCount;
- **Time Complexity:** O(N + N + N) = O(N)
- **Space Complexity:** O(N) to store chocolate count.

### Reference:

![[WhatsApp Image 2025-12-29 at 2.06.16 PM.jpeg]]
## Code:

```Java
private int findChocolateCount(int[] rank){
	
	int[] chocolates = new int[rank.length];
	
	// Distribute 1 candy to all children
	for(int i = 0; i < rank.length; i++) chocolate[i]++;
	
	// Compare left neighbour and increment chocolate count
	for(int i = 1; i < rank.length; i++){
		if(rank[i] > rank[i - 1]) chocolate[i] = chocolate[i - 1] + 1;
	}
	
	// Compare right neighbour and increment chocolate count
	for(int i = rank.length - 2; i >= 0; i--){
		if(rank[i] > rank[i + 1]){
			if(chocolate[i] <= chocolate[i + 1]) {
				chocolate[i] = chocolate[i + 1] + 1;
			}
		}
	}
	
	// Find total chocolate count
	int totalChocoCount = 0;
	for(int i = 0; i < rank.length; i++) totalChocoCount += chocolate[i];
	
	return totalChocoCount;
}
```




