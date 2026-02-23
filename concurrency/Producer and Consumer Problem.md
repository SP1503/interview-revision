### Important Note:
- **Producer:** Will produce the object only if at-least one shelf is empty.
- **Consumer:** Consumer can consume the object only if at least one object is present in the store.
- **Store:** will contain shelf of some max value that will hold the objects.

**Producer Consumer Problem:**

```Java

class Store{
	
	private int maxValue;
	
	private List<Object> shelves;
	
	public Store(int maxValue){
		this.maxValue = maxValue;
		this.shelves = new ArrayList<>();
	}
	
	public int getMaxValue(){
		return this.maxValue;
	}
	
	public List<Object> getShelves(){
		return this.shelves;
	}
	
}

class Producer implements Runnable{

	private Store store;
	
	public Producer(Store store){
		this.store = store;
	}
	
	public void run(){
		while(true){
			if(this.store.getShelves().size() < this.store.getMaxValue()){
				this.store.getShelves().add(new Object());
			}
		}
	}
	
}

class Consumer implements Runnable{

	private Store store;
	
	public Consumer(Store store){
		this.store = store;
	} 
	
	public void run(){
		while(true){
			if(this.store.getShelves().size() > 0){
				this.store.getShelves()
					.remove(this.store.getShelves().size() - 1);
			}
		}
	}
	
}

class Client{
	
	public static void main(String args[]){
		
		ExecuterService ex = Executers.newCacheThreadPool();
		
		// Creating Shared object
		Store store = new Store(6);
		
		// Create task
		for(int i = 1; i <= 8; i++){
			Producer prod = new Producer(store);
			ex.execute(prod);
		}
		
		for(int i = 1; i <= 10; i++){
			Consumer consumer = new Consumer(store);
			ex.execute(consumer);
		}
		
	}
}

Output: this code will result in error: "Index -1 out of bound of length 0"

```


### Notes
- **Why the above error occurred?**
	- let say shelves having 3 objects currently.
	- 4 threads are started into the consumer.
	- T1, T2, T3, T4.
	- All the T1, T2, T3, T4 check if the size of the list is > 0 yes for all the 4 threads.
	- Now t1 will remove Object 1.
	- t2 will remove Object 2.
	- t3 will remove Object 3
	- the t4 will end up with the error: "**Index -1 out of bound of length 0**".
- **Why this happened?**
	- because while t4 reading the list size it had 3 objects.
	- but as the context switching happened and the other threads removed the 3 objects. t4 will result in error.
- **How to fix this?**
	- Define the critical area.
	- Acquire the lock on the shared object access.
	- Now there will be no issue.
	- But is this improves the performance. No even though we have 3 objects to consume we are allowing one thread into the shared resource to consume the object which is making the current execution serialised.
- **How we can have condition on how much threads can enter into the shared resource based on the content of the shared resource?**
	- Example: Here let say list of shelves having 2 objects and 4 empty shelves.
	- Hence at a time 2 object can be consumed by 2 consumers and 4 empty shelves can be populated by objects by Producer thread.
	- 2 Objects, 4 empty = 2 Consumer + 4 producer can enter into the shared resource. This can be done using **Semaphore.**

### Applying Synchronised in Producer and Consumer Problem:

```Java

class Store{
	
	private int maxValue;
	
	private List<Object> shelves;
	
	public Store(int maxValue){
		this.maxValue = maxValue;
		this.shelves = new ArrayList<>();
	}
	
	public int getMaxValue(){
		return this.maxValue;
	}
	
	public List<Object> getShelves(){
		return this.shelves;
	}
	
}

class Producer implements Runnable{

	private Store store;
	
	public Producer(Store store){
		this.store = store;
	}
	
	public void run(){
		while(true){
			synchronised(this.store){
				if(this.store.getShelves().size() < this.store.getMaxValue()){
					this.store.getShelves().add(new Object());
				}
			}
		}
	}
	
}

class Consumer implements Runnable{

	private Store store;
	
	public Consumer(Store store){
		this.store = store;
	} 
	
	public void run(){
		while(true){
			synchronised(this.store){
				// Let say 4 threads are executing this line concurrently
				// having 3 objects , 
				if(this.store.getShelves().size() > 0){
					// All the 4 threads will have condition true
					// First 3 threads will remove the object correctly.
					// 4th thread will lead into exception.
					this.store.getShelves()
						.remove(this.store.getShelves().size() - 1);
				}
			}
		}
	}
	
}

class Client{
	
	public static void main(String args[]){
		
		ExecuterService ex = Executers.newCacheThreadPool();
		
		// Creating Shared object
		Store store = new Store(6);
		
		// Create task
		for(int i = 1; i <= 8; i++){
			Producer prod = new Producer(store);
			ex.execute(prod);
		}
		
		for(int i = 1; i <= 10; i++){
			Consumer consumer = new Consumer(store);
			ex.execute(consumer);
		}
		
	}
}

Output: this code will result in error: "Index -1 out of bound of length 0"

```

### Reference for producer and consumer problem:
![[WhatsApp Image 2026-01-22 at 6.13.10 PM.jpeg]]