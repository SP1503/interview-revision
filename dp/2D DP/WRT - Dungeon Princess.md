#revision-1 #revision-2  #NeedRevisit #written-code-on-18-jan 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/155435/assignment/problems/17/?navref=cl_pb_nv_tb
## Understanding:
- Given an 2D array dungeon
- The princess is imprisoned at bottom right corner of the dungeon
- We need to start from (0, 0) to save the princess
- Every room dungeon(i, j) will have both positive and negative values
	- Positive: Boost energy
	- Negative: consume energy
- We need to find what is the minimum energy required by the prince from 0,0 to save the princess
## Input and Output:
![[Screenshot 2026-01-07 at 6.36.21 PM.png]]
![[Screenshot 2026-01-07 at 6.37.41 PM.png]]
![[Screenshot 2026-01-07 at 6.38.02 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-07 at 6.38.18 PM.png]]
## Approach:
### Brute Force:
- Need to try every possible directions to find the minimum of all.
- Let us assume x is the energy I have when reaching F(i, j)
	- energy required to be at i, j = x + energy consumed at i, j
	- x + energy consumed at i, j = Min(energy at down,. energy at right)
	- x = min(energy at down, energy at right) + energy consumed at i, j;
- **Recurrence relation:** F(i, j) = Math.min(F(i + 1, j), F(i, j+ 1)) - currEnergyRequ
- **Base Condition:** 
	- if i == n -1 && j == m - 1 At least I need 1 energy to be in the room. Hence 1 - currentEnergy at i and j
	- If i == n - 1 then I can go only right. Hence F(i, j + 1) - currentEnergy at i and j
	- If j = m - 1 then I can go only down. Hence F(i + 1, j) - currentEnergy at i and j.
- **Time Complexity:** At every stage I taking two decision * Total N * M levels = 2 ^ n * m
- **Space Complexity:** O(N * M)
### Optimised Approach:
- We can see overlapping subproblems in the recursion tree and the recursion tree is in optimal substructure.
- We can go for Bottom up:
- **Forced Order:** F(i, j) = Math.min(F(i + 1, j), F(i, j + 1)) - currentEnergy at i and j
	- i depends on i + 1
	- j depends on j + 1
	- so we need j + 1 to find j
	- we need i + 1 to find i
	- Have iteration in reverse order.
- **Time Complexity:** O(N * M)
- **Space Complexity:** O(M) as we need only F(i + 1, j) element and F(i , j + 1) element
	- F(i + 1, j) is the previous index array that we filled.
	- F(i, j + 1) is the array that we are filling just before.
### Reference:![[WhatsApp Image 2026-01-07 at 7.03.32 PM.jpeg]]
### Code:

```Java

Recursion:

private int findMinEnergyReq(int[][] dungeon, int i, int j){

	if(i == dungeon.length - 1 && j == dungeon[0].length - 1){
		int minEnergyReq = 1 - dungeon[i][j];
		return minEnergyReq <= 0 ? 1 : minEnergyReq;
	}
	else if(i == dungeon.length - 1){
		int right = findMinEnergyReq(dungeon, i, j + 1);
		int minEnergyReq = right - dungeon[i][j];
		return minEnergyReq <= 0 ? 1 : minEnergyReq;
	}
	else if(j == dungeon[0].length - 1){
		int down = findMinEnergyReq(dungeon, i + 1, j);
		int minEnergyReq = down - dungeon[i][j];
		return minEnergyReq <= 0 ? 1 : minEnergyReq;
	}
	else{
		// Recurrence relation
		int down = findMinEnergyReq(dungeon, i + 1, j);
		int right = findMinEnergyReq(dungeon, i, j + 1);
		int minEnergyReq = Math.min(down, right) - dungeon[i][j];
		return minEnergyReq <= 0 ? 1 : minEnergyReq;
	}
}

Bottom Up:

private int findMinEnergyReqBottomUp(int[][] dungeon){
	int n = dungeon.length;
	int m = dungeon[0].length;
	int[] currMinCal = new int[m];
	int[] calculatedMin = new int[m];
	for(int i = n - 1; i >= 0; i--){
		for(int j = m - 1; j >= 0; j--){
			// Base Condition
			if(i == n - 1 && j == m - 1){
				int currMinEnergyReq = 1 - dungeon[i][j];
				currMinCal[j] = currMinEnergyReq <= 0 ? 1 : currMinEnergyReq;
			}
			else if(i == dungeon.length - 1){
				int right = currMinCal[j + 1];
				int currMinEnergyReq = right - dungeon[i][j];
				currMinCal[j] = currMinEnergyReq <= 0 ? 1 : currMinEnergyReq;
			}
			else if(j == dungeon[0].length - 1){
				int down = calculatedMin[j];
				int currMinEnergyReq = down - dungeon[i][j];
				currMinCal[j] = currMinEnergyReq <= 0 ? 1 : currMinEnergyReq;
			}
			else{
				// Recurrence relation
				int down = calculatedMin[j];
				int right = currMinCal[j + 1];
				int currMinEnergyReq = Math.min(down, right) - dungeon[i][j];
				currMinCal[j] = currMinEnergyReq <= 0 ? 1 : currMinEnergyReq;
			}
		}
		calculatedMin = currMinCal;
		currMinCal = new int[m];
	}
	return calculatedMin[0];
}

```

