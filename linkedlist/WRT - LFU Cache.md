## Problem link:
- https://leetcode.com/problems/lfu-cache/description/
## Understanding:
- Given:
	- Design LFC cache, queries to execute
- To return:
	- The answer for the queries in the given LFU.
### Brute Force:
- What is LFU?
	- Least frequently used
		- If eviction needs to be done, evict the one which is accessed least amount of frequency.
- Get:
	- Check if any node with key exists, 
	- If yes 
		- Update the frequency of node.
		- Move to current frequency bucket.
		- Update the minimum frequency calculation as we are removing a node to update its bucket
		- return the value present
	- If No,
		- Return -1
- Put:
	- Check if the node with key already exists.
		- If yes, update the value of the node.
		- Update the frequency of node.
		- Move to current frequency bucket.
		- Update the minimum frequency calculation as we are removing a node to update its bucket
	- If not exists
		- Check the capacity
			- If capacity is full
				- Evict the LRU from min frequency bucket
				- Decrease the size
			- Insert the new node for min frequency as 1
			- Increase the capacity
### Reference:![[WhatsApp Image 2026-02-23 at 1.41.29 PM.jpeg]]
![[WhatsApp Image 2026-02-23 at 1.41.30 PM.jpeg]]
### Code
```Java
class LFUCache{

	class Node{
		int key;
		int val;
		int freq;
		Node prev;
		Node next;
		
		public Node(int key, int val){
			this.key = key;
			this.val = val;
			this.freq = 1;
		}
	}
	
	class LRU{
		
		private Node head;
		private Node tail;
		private int size;
		
		public LRU(){
			this.head = new Node(-1, -1);
			this.tail = new Node(-1, -1);
			
			this.head.next = this.tail;
			this.tail.prev = this.head;
			this.size = 0;
		}
		
		public void insert(Node node){
			Node prev = this.tail.prev;
			Node next = this.tail;
			
			node.next = this.tail;
			node.prev = prev;
			
			prev.next = node;
			next.prev = node;
			
			this.size++;
		}
		
		public void delete(Node node){
			Node prev = this.node.prev;
			Node next = this.node.next;
			
			prev.next = next;
			next.prev = prev;
			
			this.size--;
		}
		
		public Node evict(){
			// If current LRU is empty return
			if(this.size == 0) return null;
			
			// Get the LRU for current order
			Node LRU = this.head.next;
			
			// evict the node
			delete(LRU);
			return LRU;
		}
	}
	
	private int minFreq = 0;
	private int capacity = 0;
	private int currSize = 0;
	
	private Map<Integer, LRU> freqMap;
	private Map<Integer, Node> keyNode;
	
	public LFUCache(int capacity){
		this.capacity = capacity;
		freqList = new HashMap<>();
		keyNode = new HashMap<>();
	}
	
	public int get(int key){
		if(keyNode.containsKey(key)){
			Node node = keyNode.get(key);
			update(node);
			return node.val;
		}
		else return -1;
	}
	
	
	public void put(int key, int val){
		if(this.keyNode.containsKey(key)){
			Node node = keyNode.get(key);
			node.val = val;
			update(node);
		}
		else{
			// Check capacity
			if(this.capacity == currSize){
				LRU currlru = freqMap.get(minFreq);
				Node evictedNode = currlru.evict();
				keyMap.remove(evictedNode.key);
				currSize--;
			}
			
			Node newNode = new Node(key, val);
			keyMap.put(newNode);
			
			minFreq = 1;
			
			freqMap.putIfAbsent(1, new LRU());
			freqMap.get(1).insert(newNode);
			
			currSize++;
		}
	}
	
	private void update(Node node){
		int freq = node.freq;
		LRU lru = freqMap.get(freq);
		
		lru.remove(node);
		
		if(freq == minFreq && lru.size == 0) minFreq++;
		
		node.freq++;
		
		freqMap.putIfAbsent(node.freq, new LRU());
		freqMap.get(node.freq).insert(node);
	}
}

```
