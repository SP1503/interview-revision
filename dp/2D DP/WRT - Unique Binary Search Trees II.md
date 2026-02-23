#revision-1 #NeedRevisit #revision-2 #NeedRevisit  #revision-3 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351257/assignment/problems/22?navref=cl_tt_lst_nm
## Understanding:
- Given integer A
- How many structure can be formed using A nodes
## Input and Output:
![[Screenshot 2026-01-07 at 8.54.30 PM.png]]
![[Screenshot 2026-01-07 at 8.54.43 PM.png]]
![[Screenshot 2026-01-07 at 8.54.53 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-07 at 8.55.08 PM.png]]
## Approach:
### Brute Force:
- Catalan Number C3 = C0 * C2 + C1 * C1 + C2 * C0
- In general Cn = sum of Ci * C(N - i - 1) for i = 0 to i <= N - 1
- We need to find F(0) to find F(1) -> recursion
- Recurrence relation: F(N) = sum(F(i) * F(N - i - 1)) for i = 0 to i <= n - 1
- Base Condition: F(0) = 1 F(1) = 1
- Time Complexity: (4^A)
- Space Complexity: O(A)
### Optimised Approach:
- Forced Order: F(N) -> F(N - i - i) so o to N we need to find
- Time complexity: O(A * A)
- Space complexity: O(A)
### Reference:
![[WhatsApp Image 2026-01-07 at 9.33.23 PM.jpeg]]
### Code
```Java
Recursion:

private int findNumTree(int A){
	if(A <= 1) return 1;
	int count = 0;
	for(int i = 0; i <= A - 1; i++){
		count += findNumTree(i) * findNumTree(A - i - 1);
	}
	return count;
}

Bottom Up:

private int findNumTreeBottomUp(int A){
	int[] count = new int[A + 1];
	for(int i = 0; i <= A; i++){
		if(i <= 1) count[i] = 1;
		else{
			int currCount = 0;
			for(int j = 0; j <= i - 1; j++){
				currCount += count[j] * count[i - j - 1];
			}
			count[i] = currCount;
		}
	}
	return count[A];
}
```

