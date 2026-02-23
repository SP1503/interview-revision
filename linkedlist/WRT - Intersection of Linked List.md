## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351216/assignment/problems/44?navref=cl_tt_lst_nm
## Understanding:
- **Given**
	- heads of two linked lists
- **To return** 
	- Find the intersecting point where two linked lists intersects each other.
![[Screenshot 2026-02-12 at 1.42.33 PM.png]]
## Input and Output:
![[Screenshot 2026-02-12 at 1.42.59 PM.png]]
![[Screenshot 2026-02-12 at 1.43.08 PM.png]]
## Problem Constraints:
![[Screenshot 2026-02-12 at 1.43.23 PM.png]]
## Approach:
### Brute Force:
- Traverse both linked lists and store both the nodes at a time in a hash set know the visited nodes.
- If we find a node while inserting other then we found the first intersection return that.
- If no intersection was found return null.
- Complexity:
	- Time Complexity: O(max(N, M))
	- Space Complexity: O(2N)
### Optimised Approach:
- Find the length of both the linked lists.
- find the diff between l1 and l2.
- iterate diff count in the largest linked list.
- Now we are starting from same point in both linked list.
	- Move both pointers to next
	- Check if both are equal, 
	- If yes return intersection
	- If no, proceed.
- Return null.
- **Complexity**:
	- **Time Complexity**: O(max(n, m))
	- **Space Complexity**: O(1)
### Reference:![[WhatsApp Image 2026-02-12 at 1.58.12 PM.jpeg]]
### Code
```Java
private ListNode findIntersection(ListNode A, ListNode B){
	// Find difference in length
	ListNode p1 = A;
	ListNode p2 = B;
	
	int l1 = findLength(p1);
	int l2 = findLength(p2);
	
	int diff = Math.abs(l1, l2);
	
	if(l1 > l2){
		int count = 0;
		while(p1 != null && count < diff){
			p1 = p1.next;
			count++;
		}
	}
	else{
		int count = 0;
		while(p2 != null && count < diff){
			p2 = p2.next;
			count++;
		}
	}
	
	// Find the first meeting point
	while(p1 != null && p2 != null){
		if(p1 == p2) return p1;
		p1 = p1.next;
		p2 = p2.next;
	}
	
	return null;
}

private int findLength(ListNode node){
	int len = 0;
	ListNode p1 = node;
	while(p1 != null){
		len++;
		p1 = p1.next;
	}
	return len;
}
```

