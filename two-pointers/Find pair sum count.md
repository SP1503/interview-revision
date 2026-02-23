## Problem link
- https://www.scaler.com/academy/mentee-dashboard/class/351234/assignment/problems/5097?navref=cl_tt_lst_nm
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java


// Time Compelxity: O(N)
// Space Complexity: O(1)
private int findPairSumCount(int[] ele, int target){
	
	// Search Space
	int l = 0;
	int r = ele.length - 1;
	
	long pairSumCount = 0l;
	
	while(l < r){
		
		// Make a guess and reduce space
		int sum = ele[l] + ele[r];
		
		if(sum == target){
			
			if(ele[l] == el[r]){
				long count = r - l + 1;
				long possPairs = (count * (count - 1))/2;
				
				pairSumCount += possPairs;
				pairSumCount %= 1000000007;
				
				break;
			}
			else{
				int a = ele[l];
				int b = ele[r];
				
				int aStart = l;
				int bStart = r;
				
				while(ele[aStart] == a) aStart++;
				
				while(ele[bStart] == b) bStart--;
				
				long aCount = aStart - (l * 1l);
				long Count = r - (bStart * 1l);
				
				pairSumCount += (acount * bCount);
				pairSumCount %= 1000000007;
				
				l = aStart;
				r = bStart;
			}
			
		}
		else if(sum > target) r--;
		else l++;
	}
	
	return (int) pairSumCount % 1000000007;
}

```

