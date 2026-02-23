## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351233/assignment/problems/159?navref=cl_tt_lst_nm
## Understanding:
- **Given:**
	- A head of linked list that contains nodes with next and random pointers.
- To find:
	- Create a deep copy of the list with same value of nodes but different node address.
## Input and Output:![[Screenshot 2026-02-03 at 10.32.46 AM.png]]
![[Screenshot 2026-02-03 at 10.33.08 AM.png]]
## Problem Constraints:
![[Screenshot 2026-02-03 at 10.33.33 AM.png]]
## Approach:
### Brute Force:
- We need to do deep copy of the given nodes.
- Iterate the given linked list
	- Create a new node with same value of current node.
	- Save old node as key and new node as value to hash.
- Iterate the given linked list again.
	- Get the new node of current node.
	- Get the new node of next node
	- Get the random node of next node.
	- Make connections
	- Proceed with next current.
- Return the new node of given head.
- **Complexity:**
	- **Time Complexity:** O(N) + O(N) for creating new nodes and connecting them
	- **Space Complexity:** O(N) for storing hash map of old and new nodes.
### Optimised Approach:
- We need hash map to know what is the new node that got created for the current old node.
- Can we solve this purpose without having a new hash map ? Yes, but creating the new node next to the curr node.
- Iterate the given linked list
	- For every node, create new node that points next to the curr node.
	- Make the next node point by the new node.
- Iterate the given linked list again.
	- Point the random nodes which is noting but curr.next.random = curr.random.next.
	- curr = curr.next.next
- Now need to point the next nodes correctly.
- Create the dummy head and tail nodes.
- Iterate the given linked list again
	- Save next = curr.next.next;
	- tail.next = curr.next
	- tail = curr
	- curr.next = next
- Now return head.next that is the head of the new node that got created.
- **Complexity**:
	- **Time Complexity**: O(N), iterating the linked list nodes for three time.
	- **Space Complexity:** O(1).
### Reference:![[WhatsApp Image 2026-02-03 at 10.52.39 AM.jpeg]]
![[WhatsApp Image 2026-02-03 at 10.52.39 AM (1).jpeg]]
### Code
```Java

Brute Force:
private RandomListNode copy(RandomListNode head){
	RandomListNode curr = head;
	Map<RandomListNode, RandomListNode> hash = new HashMap<>();
	
	// Creating new nodes and adding it in hash map
	while(curr != null){
		RandomListNode newCurr = new RandomListNode(curr.label);
		hash.put(curr, newCurr);
		curr = curr.next;
	}
	
	// Pointing the random and next pointers to the new node
	curr = head;
	while(curr != null){
		RandomListNode newCurr = hash.get(curr);
		RandomListNode next = curr.next;
		RandomListNode random = curr.random;
		if(next != null)
			newCurr.next = hash.get(next);
		if(random != null)
			newCurr.random = hash.get(random);
		curr = curr.next;
	}
	
	// return the head of the new linked list
	return hash.get(head);
}

Optimised space:

private RandomListNode copyOpt(RandomListNode head){
	RandomListNode curr = head;
	
	// Creating new node and have it next to current node
	while(curr != null){
		RandomListNode newCurr = new RandomListNode(curr.label);
		RandomListNode next = curr.next;
		newCurr.next = next;
		curr.next = newCurr;
		curr = next;
	}
	
	// Mark the random node of the new node. 
	curr = head;
	while(curr != null){
		RandomListNode random = curr.random;
		RandomListNode newCurr = curr.next;
		if(random != null) newCurr.random = random.next;
		curr = curr.next.next;
	}
	
	// Creating dummy node for head and tail
	RandomListNode newHead = new RandomListNode(-1);
	RandomListNode tail = newHead;
	
	// Now split the linked list and mark it.
	curr = head;
	while(curr != null){
		RandomListNode next = curr.next.next;
		tail.next = curr.next;
		tail = curr.next;
		curr.next = next;
		curr = curr.next;
	}
	
	return newHead.next;
}
```


