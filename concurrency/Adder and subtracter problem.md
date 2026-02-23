### Important Notes:
- The expected value is 0, but we will not get 0 always as the value of shared object.
- What happens is
	- sharedValue.value += 1, function is not atomic. It will divide the process into three steps
		- In register of t1, value = 0
		- value += 1
		- Update sharedValue.value = value;
- When both adder and subtracter executes concurrently, the update made by one adder operation will be override by the subtracter operation. Hence the value is not proper.
- **What is the problem happened here ?**
	- Thread t1 and t2 getting executed concurrently but not atomically.
	- **Step 1:** thread t1 reading value from shared resource. Example: t1.v = 1.
	- **Step 2:** thread t2 reading value from shared resource. Example: t2.v = 1.
	- **Step 3:** thread t1 increments the value that t1 read. Example: t1.v = 2.
	- **Step 4:** thread t2 decrements the value that t2 read. Example: t2.v = 0.
	- **Step 5:** Update t1 value in the shared resource. Example value = 2
	- **Step 6:** Update t2 value in the shared resource. Example value = 0.
- At this end the adder thread operation that made t1.v = 1 to 2 is gone.
- Hence this results in data inconsistency. And it required serialisable to solve this.
- **In short:**
	- Reading a truth before starting a process on the truth
	- Working in the truth.
	- While working the read truth getting changed by other thread.
- **Reason for the problem:**
	- Main reason: Context switching between threads when reading and updating the shared data. The reason can be divided into
		- **Critical section:** 
			- The smallest part of the code where the read of shared value and updation of shared value getting happened. 
			- **Can we avoid this ?:** Accessing shared resource is based on business requirement and we cannot decide.
		- **Race Condition:** 
			- Race of completing task. When t1 doing its work and waiting for some process to done, the t2 acquires the CPU that is context getting switched from t1 to t2 as t2 is willing to complete its work.
			- **Can we avoid this ?:** Yes, we can ask the next task to wait until the current task is done using some kind of lock. Provide serialisation only for critical section area.
		- **Pre emptiveness:** 
			- The CPU switching its context from t1 to t2 without completing the t1.
			- **Can we avoid this ?:** We can make the OS to do one task completely then go for the next task. But where is parallelism here.
- Ways to introduce serialisation between threads
	- Mutex: Mutual Exclusive (next page)
	- Synchronised: object level and method level synchronisation
	- Semaphore:

### Reference:
![[WhatsApp Image 2026-01-20 at 6.41.05 PM.jpeg]]

### Code:

```Java

class Adder implements Callable<void>{
	
	private SharedValue value;
	
	public Adder(SharedValue value){
		this.value = value;
	}
	
	void call(){
		for(int i = 1; i <= 1000; i++){
			this.value.value += i;
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
			this.value.value -= i;
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

