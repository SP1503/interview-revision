## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/313322/assignment/problems/63?navref=cl_tt_lst_nm
## Understanding:
- Given an matrix. print elements in spiral order.
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
### Code
```Java

private int[][] spiral = null;
private int val = 1;

public int[][] generateMatrix(int A) {
	this.spiral = new int[A][A];
	addSpiral(A);
	return this.spiral;
}

private void addSpiral(int N){
	int r = 0;
	int c = 0;
	while(N > 1){
		addBoundary(r, c, N);
		r += 1;
		c += 1;
		N -= 2;
	}
	if(N == 1){
		this.spiral[r][c] = val++;
	}
}

  

private void addBoundary(int r, int c, int N){

	// Adding left to right boundary
	for(int count = 1; count < N; count++){
		this.spiral[r][c] = val++;
		c++;
	}
	
	// Adding top to bottom boundary
	for(int count = 1; count < N; count++){
		this.spiral[r][c] = val++;
		r++;
	}
	
	// Adding right to left boundary
	for(int count = 1; count < N; count++){
		this.spiral[r][c] = val++;
		c--;
	}
	
	// Adding bottom to top boundary
	for(int count = 1; count < N; count++){
		this.spiral[r][c] = val++;
		r--;
	}
}
```


