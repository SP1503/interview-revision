#Amazon 
## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351216/assignment/problems/239?navref=cl_tt_lst_nm
## Understanding:
- Given 
	- Stream of Data
	- Capacity
- To return
	- Design an algorithm class that can be used to implement least recently used cache for the given capacity M.
## Input and Output:
![[Screenshot 2026-02-12 at 4.36.00 PM.png]]
## Problem Constraints:
## Approach:
### Optimised Approach:
- Idea:
	- For any element we need to search and get the node if present, HashMap can be used: O(1)
	- For any element that to be inserted should be done in O(1).
	- For any element that can be deleted from middle and deleted from first should be done in O(1).
	- We can use DoublyLinkedList where we can insert new element at the end in O(1). Delete the node in mid in O(1) if we know the node previously. Delete the node from front as the head node points it. 
- Complexity:
	- Time Complexity: 
		- Cache hit: O(1), delete from mid and insert at end in O(1)
		- Cache miss: O(1), check if capacity is full, 
			- If yes delete front and add at end.
			- If no, remove add at end
### Reference:
### Code
```Java

class LRUCache {
	class DLLNode{
		int key;
		int val;
		DLLNode prev;
		DLLNode next;
		DLLNode(int key, int val){
			this.key = key;
			this.val = val;
		}
	}
	
	private int capacity;
	private int size;
	private DLLNode head;
	private DLLNode tail;
	private Map<Integer, DLLNode> hash;
	
	public LRUCache(int capacity) {
		this.capacity = capacity;
		this.size = 0;
		this.head = new DLLNode(-1, -1);
		this.tail = new DLLNode(-1, -1);
		this.hash = new HashMap<>();
		this.head.next = this.tail;
		this.tail.prev = this.head;
	}
	
	public int get(int key) {
		if(this.hash.containsKey(key)){
			int value = deleteFromMid(key);
			insert(key, value);
			return value;
		}
		else return -1;
	}
	
	public void put(int key, int value) {
		if(this.hash.containsKey(key)){
			deleteFromMid(key);
			insert(key, value);
		}
		else{
			if(this.capacity == this.size) deleteFromFront();
			insert(key, value);
		}
	}
	
	private void insert(int key, int value){
		DLLNode newNode = new DLLNode(key, value);
		DLLNode prev = this.tail.prev;
		DLLNode next = this.tail;
		newNode.prev = prev;
		newNode.next = next;
		prev.next = newNode;
		next.prev = newNode;
		this.hash.put(key, newNode);
		this.size++;
	}
	
	private int deleteFromMid(int key){
		DLLNode currNode = this.hash.get(key);
		DLLNode prev = currNode.prev;
		DLLNode next = currNode.next;
		prev.next = next;
		next.prev = prev;
		this.hash.remove(key);
		this.size--;
		return currNode.val;
	}
	
	private int deleteFromFront(){
		DLLNode currNode = this.head.next;
		int key = currNode.key;
		DLLNode prev = currNode.prev;
		DLLNode next = currNode.next;
		prev.next = next;
		next.prev = prev;
		this.hash.remove(key);
		this.size--;
		return currNode.val;
	}
}

```

