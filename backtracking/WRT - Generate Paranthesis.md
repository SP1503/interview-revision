## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351227/assignment/problems/139?navref=cl_tt_lst_nm
## Understanding:
- Given
	- A number of pairs of parenthesis output can have
- To return:
	- Return list of all valid parenthesis of size 2N.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Here we need to generate all the possible valid parenthesis list of size 2N.
- At every point we have two decisions
	- Adding (  in the current place
	- Adding ) in the current place
- There are some constraints
	- We need to generate valid parenthesis
	- If open braces count > N then it is not valid
	- If close braces count > openCount then also it is not valid.
	- Hence need to handle those cases.
- Complexity:
	- Time Complexity: Number of function calls * time complexity of each call
	- No of function calls = At every stage we are taking 2 decision and there are 2N stages
	- Complexity of each call is O(1)
	- TC: O(2 ^ 2N) * O(1) = O(2 ^ 2N)
	- SC: O(2N)
### Reference:![[WhatsApp Image 2026-02-19 at 7.10.41 PM.jpeg]]
### Code
```Java

private List<String> validParanthesis = new ArrayList<>();

// Time Complexity: O(2 ^ 2N)
// Space Complexity: O(2N) 
private void findValidParanthesis(
	StringBuilder currPar, 
	int openCnt, 
	int closeCnt,
	int n){
	
	// Base Condition
	if(openCnt == n && closeCnt == n){
		validParanthesis.add(currPar.toString());
		return;
	}
	
	// Recurrence Relation
	// At every point I have two decision adding ( or adding )
	// Adding open
	if(openCnt < n){
		currPar.append('(');
		findValidParanthesis(
			new StringBuilder(currPar), 
			openCnt + 1, 
			closeCnt, 
			n);
		currPar.removeCharAt(currPar.length() - 1);
	}
	
	// Adding close braces
	if(closeCnt < openCnt){
		currPar.append('(');
		findValidParanthesis(
			new StringBuilder(currPar), 
			openCnt, 
			closeCnt + 1, 
			n);
		currPar.removeCharAt(currPar.length() - 1);
	}
}

```
