### Important Notes:
- The ways to avoid concurrency issues between threads are
	- **Mutex:** Mutual Exclusion
	- **Synchronised:** Applying synchronised keyword in object level (using synchronised block) and method level.
		- The synchronised block is preferred compared to the synchronised method as the synchronised method acquires lock over the object, Hence we are not able to use any other method of that object by any threads that using it.
	- Semaphore:
- **What this will do ?**
	- As accessing the same shared value by different threads is the issue. We will provide a lock to the shared value and the key will be accessed by only one thread at a time.

### Reference for Mutex locks:
![[WhatsApp Image 2026-01-22 at 5.15.33 PM.jpeg]]
### Applying Mutex in Adder Subtractor

```Java

import java.util.concurrent.locks.Lock;

class Adder implements Callable<void>{
	
	private SharedValue value;
	
	private Lock lock;
	
	public Adder(SharedValue value, Lock lock){
		this.value = value;
		this.lock = lock;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			// Critical Section 
			// Smallest piece of code that access shared value.
			// Piece of code that is getting executed serialisably
			this.lock.lock();
			this.value.value += i;
			this.lock.unlock();
		}
	}
}

class Subracter implements Callable<void>{

	private SharedValue value;
	
	private Lock lock;
		
	public Subracter(SharedValue value, Lock lock){
		this.value = value;
		this.lock = lock;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			// Critical Section 
			// Smallest piece of code that access shared value.
			// Piece of code that is getting executed serialisably
			this.lock.lock();
			this.value.value -= i;
			this.lock.unlock();
		}
	}	
}

class SharedValue{
	
	public int value;
	
	public SharedValue(){
		this.value = 0;
	}
}

import java.util.concurrent.locks.ReentrantLock;

class Client{
	
	public static void main(String args[]){
		
		SharedValue value = new SharedValue();
		
		// Creating a lock for locking the shared value
		Lock lock = new ReentrantLock();
		
		Callable adderTask = new Adder(value, lock);
		
		Callable subracterTask = new Subracter(value, lock);
		
		ExecutorService service = Executor.newFixedThreadPool(2);
		
		Future<void> adder = service.submit(adderTask);
		Future<void> subractor = service.submit(subractorTask);
		
		adder.get();
		subractor.get();
		
		System.out.println(value.value);
	}
}

```

### Applying Synchronised in Adder Subtractor

```Java

class Adder implements Callable<void>{
	
	private SharedValue value;
	
	public Adder(SharedValue value){
		this.value = value;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			// Having synchronised block that acquires the lock 
			// and release it will done
			synchronised(this.value){
				this.value.value += i;
			}
		}
	}
}

class Subracter implements Callable<void>{

	private SharedValue value;
		
	public Subracter(SharedValue value){
		this.value = value;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			// Having synchronised block that acquires the lock 
			// and release it will done
			synchronised(this.value){
				this.value.value -= i;
			}
		}
	}	
}

class SharedValue{
	
	public int value;
	
	public SharedValue(){
		this.value = 0;
	}
}

class Client{
	
	public static void main(String args[]){
		
		SharedValue value = new SharedValue();
		
		Callable adderTask = new Adder(value);
		
		Callable subracterTask = new Subracter(value);
		
		ExecutorService service = Executor.newFixedThreadPool(2);
		
		Future<void> adder = service.submit(adderTask);
		Future<void> subractor = service.submit(subractorTask);
		
		adder.get();
		subractor.get();
		
		System.out.println(value.value);
	}
}

```

### Important notes on Synchronisation on Objects:
- Whenever the code finds the synchronisation keyword
	- It will check if the provided object is currency acquired by any other threads.
	- If yes, it will wait.
	- If no then this current thread will acquire the lock for the shared value and release the lock once its work is done.
	- **What happen if any other thread access the same object of the shared resource without using synchronised in it?** 
		- The thread that trying to access the shared resource is not asking for permission, Hence it is allowed to change the state of the shared resource value. But will result in concurrency issue.
- Lock will happen only on objects whether it is object level or object of class where the method is.
- Synchronised will only be on current object.
### Applying Synchronisation on method level in Adder Subtractor

```Java

class Adder implements Callable<void>{
	
	private SharedValue value;
	
	public Adder(SharedValue value){
		this.value = value;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			// Here add value is a synchronised method, 
			// Hence it will acquire lock on the this.value object 
			// and release once the method got executed
			this.value.addValue(i);
		}
	}
}

class Subracter implements Callable<void>{

	private SharedValue value;
		
	public Subracter(SharedValue value){
		this.value = value;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			// Here add value is a synchronised method, 
			// Hence it will acquire lock on the this.value object 
			// and release once the method got executed
			this.value.subtractValue(i);
		}
	}	
}

class SharedValue{
	
	public int value;
	
	public SharedValue(){
		this.value = 0;
	}
	
	synchronised void addValue(int i){
		this.value += i;
	}
	
	synchronised void subtractValue(int i){
		this.value -= i;
	}
	
	void multiply(int i){
		// If some thread try to call the multiply method it won't wait 
		// as it is not synchronised
		this.value *= i;
	}
}

class Client{
	
	public static void main(String args[]){
		
		SharedValue value = new SharedValue();
		
		Callable adderTask = new Adder(value);
		
		Callable subracterTask = new Subracter(value);
		
		ExecutorService service = Executor.newFixedThreadPool(2);
		
		Future<void> adder = service.submit(adderTask);
		Future<void> subractor = service.submit(subractorTask);
		
		adder.get();
		subractor.get();
		
		System.out.println(value.value);
	}
}

```



