## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351216/assignment/problems/4226?navref=cl_tt_lst_nm
## Understanding:
- Given:
	- head of a linked list with cycle.
- Return
	- Remove the cycle from the linked list
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
- Iterate the given linked list
	- Check if current node is already visited
		- If yes make prev.next = null;
	- On every iteration add the current node to the hash set to say mark as visited.
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(N), using hash set to store the visited nodes.
## Optimised Solution:
- Use Slow and Fast pointer to find the starting point of the cycle.
- Have prev pointer to know the prev of the starting point.
- Make prev.next = null.
- Complexity:
	- Time Complexity: O(N)
	- Space Complexity: O(1)
### Reference:![[WhatsApp Image 2026-02-12 at 2.16.43 PM.jpeg]]
### Code
```Java

private ListNode removeLoop(ListNode node){
	Set<ListNode> visited = new HashSet<>();
	ListNode p1 = node;
	ListNode prev = null;
	while(p1 != null){
		if(visited.contains(p1)){
			prev.next = null;
			break;
		}
		
		visited.add(p1);
		
		prev = p1;
		p1 = p.next;
	}
	
	return node;
}

private ListNode removeLoop(ListNode node){
	
	ListNode meetPoint = findMeetPoint(node);
	
	// We know X = Z
	ListNode p1 = node;
	ListNode p2 = meetPoint;
	
	ListNode prev = null;
	while(p1 != p2){
		
		prev = p2;
		
		p1 = p1.next;
		p2 = p2.next;
	}
	
	prev.next = null;
	
	return node;
}

private ListNode findMeetPoint(ListNode node){
	ListNode slow = node;
	ListNode fast = node;
	
	while(fast != null && fast.next != null){
		slow = slow.next;
		fast = fast.next.next;
		
		if(slow == fast) return slow;
	}
	
	return null;
}
