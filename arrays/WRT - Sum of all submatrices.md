## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351620/assignment/problems/4091/?navref=cl_pb_nv_tb
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:![[WhatsApp Image 2026-02-21 at 4.01.19 PM.jpeg]]
### Code
```Java

private int findSubmatrixSum(int[][] mat){
	int sum = 0;
	int m = mat.length;
	int n = mat[0].length;
	for(int i = 0; i < m; i++){
		for(int j = 0; j < n; j++){
			int noOfStart = ((i + 1) * (j + 1));
			int noOfEnd = ((m - i) * (n - j));
			sum += (noOfStart * noOfEnd * mat[i][j]);
		}
	}
	return sum;
}

```

