#revision-1 #revision-2 #revision-3 #NeedRevisit #written-code-on-18-jan  
## Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/351249/assignment/problems/188953?navref=cl_tt_lst_nm
## Understanding:
- Given two strings S and T.
- Find the minimum window of S that have all the characters of T.
## Input and Output:
![[Screenshot 2026-01-01 at 1.02.23 PM.png]]
![[Screenshot 2026-01-01 at 1.02.41 PM.png]]
![[Screenshot 2026-01-01 at 1.03.06 PM.png]]
## Problem Constrains:
![[Screenshot 2026-01-01 at 1.03.32 PM.png]]
## Approach:
### Brute force:
- Generate all the substrings of S.
- Iterate the substrings
	- Check if substring having all char of T
	- If yes, compute the substring with minimum length
- **Time Complexity:** O(N * N) to find all the substrings * O(M) to check if all the M characters is in the current substring = O(N * N * M)
- **Space Complexity:** O(1)
### Optimal approach:
- Idea: Generating substrings can be optimised by using a sliding window
- Create a tMap that have the characters and its frequencies.
- Store the characters and its frequencies from t string.
- Create a sMap to store characters and its frequencies.
- Iterate the string S from o to S.length()
	- add the curr char to the sMap
	- Check if sMap having all the characters with its frequencies of tMap. 
	- if yes, start shrinking
	- While shrinking compute the smallest substring that have all char of T
	- If no proceed with next character
- return the computed substring.
- **Time Complexity:** O(M) + O(N * M) = O(N * M)
- **Space Complexity:** O(N + M)
### Reference:

![[WhatsApp Image 2026-01-01 at 1.12.42 PM.jpeg]]
## Code:

```Java

private String findMinWindow(String s, String t){
	Map<Character, Integer> tMap = new HashMap<>();
	
	for(char ch : t.toCharArray()) tMap.put(ch, tMap.getOrDefault(ch, 0) + 1);
	
	Map<Character, Integer> sMap = new HashMap<>();
	
	int start = 0;
	int end = 0;
	
	int resLen = s.length();
	String res = new String();
	
	while(end < s.length()){
		char ch = s.charAt(end);
		sMap.put(ch, sMap.getOrDefault(ch, 0) + 1);
		
		while(isMapEqual(sMap, tMap)){
			if(end - start + 1 <= resLen){
				resLen = end - start + 1;
				res = s.substring(start, end + 1);
			}
			
			char remChr = s.charAt(start);
			if(sMap.get(remChr) == 1) sMap.remove(remChr);
			else sMap.put(remChr, sMap.get(remChr) - 1);
			
			start++;
		}
		end++;
	}
	return res;
}

  

private boolean isMapEqual(
	Map<Character, Integer> sMap, 
	Map<Character, Integer> tMap){
	
	for(Character chr : tMap.keySet()){
		if(!sMap.containsKey(chr) || sMap.get(chr) < tMap.get(chr)) return false;
	}
	return true;
}
```
