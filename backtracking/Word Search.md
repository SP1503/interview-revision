## Problem link:
- https://leetcode.com/problems/word-search/description/
## Understanding:
- Given
	- A grid of characters of size M * N and a word.
- Return:
	- Find whether given word is present in the given grid
### Brute Force:
- This is kind of pruning the search problem
- Hence we are using backtracking here.
- At every state we have 4 decisions
	- Go Top
	- Go Down
	- Go Left
	- Go Right

### Reference:![[WhatsApp Image 2026-02-23 at 11.19.53 AM.jpeg]]
### Code
```Java
class Solution {

	private int[] row = {-1, 1, 0, 0};
	private int[] col = {0, 0, -1, 1};
	
	
	public boolean exist(char[][] board, String word) {
		return isWordExists(board, word);
	}
	
	// Time Complexity: O(N * M) * O(4 ^ (M * N))
	// Space Complexity: O(M * N)
	private boolean isWordExists(char[][] board, String word){
		int m = board.length;
		int n = board[0].length;
		boolean[][]]]]] visited = new boolean[m][n];
		for(int r = 0; r < m; r++){
			for(int c = 0; c < n; c++){
				boolean isPresent = isWordPresent(board, visited, word, 0, r, c);
				if(isPresent) return true;
			}
		}
		return false;
	}
	
	private boolean isWordPresent(
	char[][] board, 
	boolean[][] visited, 
	String word, 
	int index, 
	int r, 
	int c){
		// Base Condition
		if(index == word.length()) return true;
		else if(visited[r][c] || word.charAt(index) != board[r][c]) return false;
		else{
			int m = board.length;
			int n = board[0].length;
			
			if(index == word.length() - 1) return true;
			
			// Recurrence Relation
			visited[r][c] = true;
			for(int dir = 0; dir < 4; dir++){
				int nextR = r + row[dir];
				int nextC = c + col[dir];
							
				if(nextR >= 0
				&& nextR < m
				&& nextC >= 0
				&& nextC < n
				&& !visited[nextR][nextC]){
					boolean isPresent = isWordPresent(board, visited, 
						word, 
						index + 1, 
						nextR, 
						nextC);
						
					if(isPresent) return true;
				}
			}
		}
		
		// Backtracking undo changes
		visited[r][c] = false;
		
		return false;
	}
}
```

