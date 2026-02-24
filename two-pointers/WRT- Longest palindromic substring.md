## Problem link:
- https://leetcode.com/problems/longest-palindromic-substring/
## Understanding:
- Given:
	- String
- To return:
	- Longest palindromic substring that can be formed from the given string
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Generate all the substrings
- Find whether the given string is a palindrome 
	- If yes, store the substring with max length
	- If no return
- Complexity:
	- Time Complexity: O(N * N * N) 
	- Space Complexity: O(1)
### Optimised Approach:
- Two pointers
- Consider every index of the string as a centre point or right point
- Expand the length until the substring stops as a palindrome
- Complexity:
	- Time Complexity: O(N * N)
	- Space Complexity: O(1)
### Reference:
### Code
```Java

public String longestPalindrome(String s) {
	return findLongestPalindrome(s);
}

private String findLongestPalindrome(String s){
	String longestPalin = s.substring(0, 1);
	for(int i = 1; i < s.length(); i++){
		
		// Having current char as center 
		// and find odd number of palindromic substring
		int left = i - 1;
		int right = i + 1;
		
		while(left >= 0
		&& right < s.length()
		&& s.charAt(left) == s.charAt(right)){
			if(right - left + 1 > longestPalin.length()) 
				longestPalin = s.substring(left, right + 1);
			left--;
			right++;
		}
	
		// Having current char as center 
		// and find even number of palindromic substring
		left = i - 1;
		right = i;
		
		while(left >= 0
		&& right < s.length()
		&& s.charAt(left) == s.charAt(right)){
			if(right - left + 1 > longestPalin.length()) 
				longestPalin = s.substring(left, right + 1);
			left--;
			right++;
		}
	}
	return longestPalin;
}
```

