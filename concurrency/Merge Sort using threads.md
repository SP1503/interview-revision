### Important Notes:
- Execution service will help to reuse the same threads that we are creating inside the thread pool for the tasks that we are assigning to it.
- Executor service will contain
	- Worker Threads
	- First In First Out Queue
	- Thread Pool Manager
- The thread pool types are
	- FixedThreadPool()
	- CacheThreadPool()
	- SingleThreadExecuter
	- ScheduledThreadPool()
	- WorkStealingThreadPool()
- Hence this will reduce the overhead of creating 1000 threads and executing 1000 tasks instead it provides x thread and reuse it for whatever tasks we assigned to it.
- This threads count helps to achieve Concurrency.
- **Executor.newFixedThreadPool(x)** will create x number of threads in the thread pool and will use these number of threads for the assigned tasks that is present in the task queue of the thread scheduler.
- These executor service will be used to manage the life cycle of the threads that is created inside the thread pool.
- If we assign 10 tasks to the executor service with 5 threads, then the first 5 tasks will be handled by the threads we created and the next 5 tasks will wait in the queue until the thread becomes free.
- **Executor.newCacheThreadPool()** will create number of threads that are required as per the current task allocation status. If  there is a situation where a task allocated and no thread are free at that time, CacheThreadPool will create a new thread for the task to execute.
- The life cycle status of thread are INITIATED, SCHEDULED, RUNNING, TERMINATED.
- **Deadlock:** Let say for the current Merge sort function we are creating only one thread inside it and assigning the current task to do. The current task creates two more tasks and wait for the thread to execute. But there is only one thread that is what created new task and waiting for other thread which needs to execute this task that is not happening. This is called DeadLock. 
- In the below code the left array and right array is getting sorted parallel. Thus it improves the performance of the code.
- Need to add executor.shutdown().
### Reference:![[WhatsApp Image 2026-01-20 at 6.34.06 PM.jpeg]]![[WhatsApp Image 2026-01-20 at 6.37.02 PM.jpeg]]

### Code:

```Java
class Client{
	
	public static void main(String args[]){
		
		ExecutorService service = Executors.newCacheThreadPool();
		
		MergeSort task1 = 
			new MergeSort(new int[] {8, 4, 5, 7, 9, 2, 1}, service);
			
		Future<List<Integer>> sortedList = service.submit(task1);
		
		System.out.println("The sorted list is " + sortedList.get());
		
		service.shutdown();
	}
}

class MergeSort implements Callable<List<Integer>>{
	
	private List<Integer> arrayToSort;
	
	private ExecutorService service;
	
	public MergeSort(List<Integer> arrayToSort, ExecutorService service){
		this.arrayToSort = arrayToSort;
		this.service = service;
	}
	
	public List<Integer> call(){
	
		// Base Condition
		if(arrayToSort.size() == 1) return arrayToSort;
	
		// Partition the array into two parts
		int midIndex = arrayToSort.size() / 2;
		
		List<Integer> leftHalf = new ArrayList<>();
		List<Integer> rightHalf = new ArrayList<>();
		
		for(int i = 0; i < midIndex; i++){
			leftHalf.add(arrayToSort.get(i));
		}
		
		for(int i = midIndex; i < arrayToSort.size(); i++){
			rightHalf.add(arrayToSort.get(i));
		}
		
		// Sort both half individually
		MergeSort sortTask1 = new MergeSort(leftHalf);
		MergeSort sortTask2 = new MergeSOrt(rightHalf);
		
		Future<List<Integer>> sortedLeftHalf = service.submit(sortTask1);
		Future<List<Integer>> sortedRightHalf = service.submit(sortTask2);
		
		leftHalf = sortedLeftHalf.get();
		rightHalf = sortedRightHalf.get();
		
		// Merge both
		int p1 = 0;
		int p2 = 0;
		int index = 0;
		
		List<Integer> sortedList = new ArrayList<>();
		
		while(p1 < leftHalf.size() && p2 < rightHalf.size()){
			if(leftHalf.get(p1) <= rightHalf.get(p2)){
				sortedList.add(leftHalf.get(p1));
				p1++;
			}
			else{
				sortedList.add(rightHalf.get(p2));
				p2++;
			}
		}
		
		while(p1 < leftHalf.size()){
			sortedList.add(leftHalf.get(p1));
			p1++;
		}
		
		while(p2 < rightHalf.size()){
			sortedList.add(rightHalf.get(p2));
			p2++;
		}
		
		return sortedList;
	}
}

```




