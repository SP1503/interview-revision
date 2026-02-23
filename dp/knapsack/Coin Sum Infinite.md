#revision-1 #revision-2  #NeedRevisit 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351260/assignment/problems/319/submissions
## Understanding:
- Given an integer array coin[]
- Given an amount
- Find total number of ways we can make the amount with given coins
## Input and Output:
![[Screenshot 2026-01-11 at 2.22.47 PM.png]]
![[Screenshot 2026-01-11 at 2.23.08 PM.png]]
![[Screenshot 2026-01-11 at 2.23.21 PM.png]]

## Problem Constraints:
## Approach:
### Brute Force:
- Need to find all the possibilities, but the possibility should not repeat.
- Recursion can be used.
- **Recurrence relation** F(index, amount) = F(index, amount - coin[index]) + F(index + 1, amount)
	- Taking current: At every step I can choose the current coin, if the current coin is less than amount
	- Skipping: skipping the current coin.
- **Base Condition:** 
	- amount == 0 then we found a way return 1
	- index == n then we exhausted with all the options. Hence return 0
- **Time Complexity:**
	- At every state we are taking two decisions. We are going for n * amount times = O(N * Amount)
- **Space Complexity:** O(N * Amount)
### Optimised Approach:
- In this recursion tree we can find the Optimal substructure and Overlapping sub problems.
- Hence this can be optimised using TopDown Approach and Bottom Up Approach.
- **Forced Order:**
	- index depends on index + 1. Reverse Order
	- Amount depends on amount - coins[index]. Forward Order
- **Dependency graph:** 
	- At every state we are changing either index or amount.
	- Hence we require 2D DP.
- **Time Complexity:** O(N * Amount)
- **Space Complexity:** O(N * Amount)

### Optimised Space Approach:
- **Space Optimisation:**
	- Instead of using a 2D array, 2 arrays of size total amount will suffice as the current element depends on i + 1 index or i index.
- **Time Complexity:** O(N * Amount)
- **Space Complexity:** O(Amount);

### Reference:
![[WhatsApp Image 2026-01-11 at 2.31.16 PM.jpeg]]
### Code
```Java

Recursion:

private int findCoinChange(int[] coins, int amount, int i){
	// Base
	if(amount == 0) return 1;
	else if(i == coins.length) return 0;
	else{
		// Recurrence
		int take = 0;
		if(amount >= coins[i]) take = 
				findCoinChange(coins, amount - coins[i], i);
		int noTake = findCoinChange(coins, amount, i + 1);
		return take + noTake;
	}
}

Top Down Approach:

private int findCoinChangeIterative(int[] coins, int totalAmount){
	int n = coins.length;
	int[][] coinCount = new int[n + 1][totalAmount + 1];
	for(int i = n; i >= 0; i--){
		for(int amount = 0; amount <= totalAmount; amount++){
			// Base
			if(amount == 0) coinCount[i][amount] = 1;
			else if(i == coins.length) coinCount[i][amount] = 0;
			else{
				// Recurrence
				int take = 0;
				if(amount >= coins[i]) take = coinCount[i][amount - coins[i]];
				int noTake = coinCount[i + 1][amount];
				coinCount[i][amount] = take + noTake;
				coinCount[i][amount] %= 1000007;
			}
		}
	}
	return coinCount[0][totalAmount] % 1000007;
}

The space used can be further optimised using 2 array of size amount

private int findCoinChangeIterativeOpt(int[] coins, int totalAmount){
	int n = coins.length;
	int[] a = new int[totalAmount + 1];
	int[] b = new int[totalAmount + 1];
	for(int i = n; i >= 0; i--){
		for(int amount = 0; amount <= totalAmount; amount++){
			// Base
			if(amount == 0) a[amount] = 1;
			else if(i == coins.length) a[amount] = 0;
			else{
				// Recurrence
				int take = 0;
				if(amount >= coins[i]) take = a[amount - coins[i]];
				int noTake = b[amount];
				a[amount] = take + noTake;
				a[amount] %= 1000007;
			}
		}
		
		b = a;
		a = new int[totalAmount + 1];
	}
	return b[totalAmount] % 1000007;
}
```


