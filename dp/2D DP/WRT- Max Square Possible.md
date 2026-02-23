## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private int[] row = {-1, -1, 0};
private int[] col = {0, -1, -1};

public int maximalSquare(char[][] matrix) {
	int m = matrix.length;
	int n = matrix[0].length;
	int[][] maxSquarePoss = new int[m][n];
	int maxSquare = 0;
	for(int i = 0; i < m; i++){
		for(int j = 0; j < n; j++){
			if(matrix[i][j] == '1'){
			if(i == 0 || j == 0) maxSquarePoss[i][j] = 1;
			else 
				maxSquarePoss[i][j] = 
					1 + Math.min(maxSquarePoss[i-1][j], 
					Math.min(maxSquarePoss[i-1][j-1], maxSquarePoss[i][j-1]));
			}
			maxSquare = Math.max(maxSquare, maxSquarePoss[i][j]);
		}
	}
	return maxSquare * maxSquare;
}
```

