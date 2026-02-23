#revision-1 #revision-2 #NeedRevisit  
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351265/assignment/problems/4704?navref=cl_tt_lst_nm
## Understanding:
- Given a grid of size N * M.
- Having only 0, 1, 2.
- 0 = nothing
- 1 = fresh oranges
- 2 = Rotten Oranges.
- Find minimum time required to rot all the oranges.
- If not able to rot all the oranges return -1
## Input and Output:
![[Screenshot 2026-01-19 at 7.34.56 PM.png]]
![[Screenshot 2026-01-19 at 7.35.23 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-19 at 7.35.36 PM.png]]
## Approach:
### Strategy: 
- Multi Source BFS - Find the shortest path from the given all the sources to the destination.
### Brute Force:
- From every rotten orange, how much minimum time required to rot all the available fresh orange around it. 
- The max time of that comparison is the answer.
- We can count all the fresh oranges first.
- Run BFS every time for each rotten orange and decrement the fresh oranges that are reachable or neighbour to the current rotten orange.
- The max time is the answer.
- **Complexity**:
	- **Time Complexity**: At worst case, we are going to reach every index at max once with the help of visited. Hence the complexity is O(N * M).
	- **Space Complexity**: O(1). We are using the given grid itself as a visited array. Hence we don't need the space complexity.
### Optimised Approach:
- Instead of doing BFS for every rotten oranges individually, we can add all the rotten orange with time to rot as 0 in the queue of single BFS.
- Iterate the BFS until the queue becomes empty or the fresh count becomes 0.
- If fresh orange count becomes zero return the current rotten time + 1
- If queue becomes empty then return -1.
- **Complexity:**
	- **Time Complexity:** O(N * M)
	- **Space Complexity:** O(1)
### Reference:
![[WhatsApp Image 2026-01-19 at 9.41.14 PM.jpeg]]
### Code
```Java
private int[] row = {-1, 1, 0, 0};
private int[] col = {0, 0, -1, 1};


class OrangeInfo{
	public int r;
	public int c;
	public int time;
	public OrangeInfo(int r, int c, int time){
		this.r = r;
		this.c = c;
		this.time = time;
	}
}

Multi Source BFS:
private int findMinTimeToRot(int[][] grid){
	Queue<OrangeInfo> queue = new LinkedList<>();
	int freshCount = 0;
	for(int i = 0; i < grid.length; i++){
		for(int j = 0; j < grid[0].length; j++){
			int val = grid[i][j];
			if(val == 2) queue.add(new OrangeInfo(i, j, 0));
			else if(val == 1) freshCount++;
		}
	}
	return BFS(queue, grid, freshCount);
}

private int BFS(Queue<OrangeInfo> queue, int[][] grid, int freshCount){
	while(!queue.isEmpty()){
		OrangeInfo currOrangeInfo = queue.poll();
		int r = currOrangeInfo.r;
		int c = currOrangeInfo.c;
		int time = currOrangeInfo.time;
		for(int dir = 0; dir < 4; dir++){
			int nextR = r + row[dir];
			int nextC = c + col[dir];
			
			if(nextR >= 0
			&& nextR < grid.length
			&& nextC >= 0
			&& nextC < grid[0].length
			&& grid[nextR][nextC] == 1){
				queue.add(new OrangeInfo(nextR, nextC, time + 1));
				grid[nextR][nextC] = 2;
				freshCount--;
				if(freshCount == 0) return time + 1;
			}
		}
	}
	return -1;
}

```





