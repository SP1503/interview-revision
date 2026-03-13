## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351220/assignment/problems/76?navref=cl_tt_lst_nm
## Understanding:
- Given an integer index
- Find the value that is represented by integer index in the excel column
### Brute Force:
- Here we can see that like how in decimal every value is represented from 0 to 9, in the same way the given value will be represented from 0 to 26.
- Hence making % 26 gives the last character that can be formed from the index given as input.
- **Reason**: Why -1 from given input 1 represents A but as per code 0 represents A
### Reference:![[WhatsApp Image 2026-03-05 at 4.32.28 PM.jpeg]]
### Code
```Java
#Brute Force

// Time Complexity: O(log26)
// Space Complexity: O(1)
private String findColumnName(int index){
	
	StringBuilder str = new StringBuilder();
	
	while(index > 0){
		index -= 1;
		int rem = index % 26;
		
		str.append((char) ('A' + rem));
		
		index /= 26;
	}
	
	return str.reverse().toString();
}
```

