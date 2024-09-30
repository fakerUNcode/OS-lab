**Can we write multithreading programs in C?** 

Unlike Java, multithreading is not supported by the language standard. [POSIX Threads (or Pthreads)](http://en.wikipedia.org/wiki/POSIX_Threads) is a POSIX standard for threads. Implementation of pthread is available with gcc compiler. 

# **A simple C program to demonstrate use of pthread basic functions** 

```c
#include <stdio.h> 
#include <stdlib.h> 
#include <unistd.h> //Header file for sleep(). man 3 sleep for details. 
#include <pthread.h> 

// A normal C function that is executed as a thread 
// when its name is specified in pthread_create() 
void *myThreadFun(void *vargp) 
{ 
	sleep(3); 
	printf("Printing GeeksQuiz from Thread \n"); 
	return NULL; 
} 

int main() 
{ 
	pthread_t thread_id; 
	printf("Before Thread\n"); 
	pthread_create(&thread_id, NULL, myThreadFun, NULL); 
	pthread_join(thread_id, NULL); 
	printf("After Thread\n"); 
	exit(0); 
}

```

## Explanation

- It creates a new thread using 

  ```C
  pthread_create
  ```

  .

  - The first argument is a pointer to the `pthread_t` variable where the thread ID will be stored.
  - The second argument specifies thread attributes (here `NULL` for default attributes).
  - The third argument is the thread function (`myThreadFun`).
  - The fourth argument is a pointer to any arguments passed to the thread function (here `NULL`).

- It waits for the created thread to finish execution using 

  ```c
  pthread_join
  ```

  .

  - This function blocks the execution of the calling thread until the specified thread (`thread_id`) terminates.

- After the joined thread completes, it prints "After Thread" and exits the program.

![Screenshot 2024-05-24 at 11.18.01](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405241118358.png?sleflearingnotes)

# How Threads works

```c
#include <stdio.h> 
#include <stdlib.h> 
#include <unistd.h> 
#include <pthread.h> 

// Let us create a global variable to change it in threads 
int g = 0; 

// The function to be executed by all threads 
void *myThreadFun(void *vargp) 
{ 
	// Store the value argument passed to this thread 
	int *myid = (int *)vargp; 

	// Let us create a static variable to observe its changes 
	static int s = 0; 

	// Change static and global variables 
	++s; ++g; 

	// Print the argument, static and global variables 
	printf("Thread ID: %d, Static: %d, Global: %d\n", *myid, ++s, ++g); 
} 

int main() 
{ 
	int i; 
	pthread_t tid; 

	// Let us create three threads 
	for (i = 0; i < 3; i++) 
		pthread_create(&tid, NULL, myThreadFun, (void *)&tid); 

	pthread_exit(NULL); 
	return 0; 
} 
```

## OUTPUT 

![Screenshot 2024-05-24 at 11.18.54](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405241118965.png?sleflearingnotes)

Please note that above is simple example to show how threads work. Accessing a global variable in a thread is generally a bad idea. What if thread 2 has priority over thread 1 and thread 1 needs to change the variable. In practice, if it is required to access global variable by multiple threads, then they should be accessed using a mutex. 





 

# Mutex lock for Linux Thread Synchronization



## **Thread Synchronization Problems**

```c
#include <pthread.h> 
#include <stdio.h> 
#include <stdlib.h> 
#include <string.h> 
#include <unistd.h> 

pthread_t tid[2]; 
int counter; 

void* trythis(void* arg) 
{ 
	unsigned long i = 0; 
	counter += 1; 
	printf("\n Job %d has started\n", counter); 

	for (i = 0; i < (0xFFFFFFFF); i++) 
		;  // Busy wait loop to simulate work 

	printf("\n Job %d has finished\n", counter); 

	return NULL; 
} 

int main(void) 
{ 
	int i = 0; 
	int error; 

	while (i < 2) { 
		error = pthread_create(&(tid[i]), NULL, &trythis, NULL); 
		if (error != 0) 
			printf("\nThread can't be created : [%s]", strerror(error)); 
		i++; 
	} 

	pthread_join(tid[0], NULL); 
	pthread_join(tid[1], NULL); 

	return 0; 
} 
```



## OUTPUT

![Screenshot 2024-05-24 at 11.30.02](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405241130179.png?sleflearingnotes)

**Problem:** From the last two logs, one can see that the log ‘*Job 2 has finished*’ is repeated twice while no log for ‘*Job 1 has finished*’ is seen.

WHY?

- The log ‘*Job 2 has started*’ is printed just after ‘*Job 1 has Started*’ so it can easily be concluded that while thread 1 was processing the scheduler scheduled the thread 2.
- If we take the above assumption true then the value of the ‘*counter*’ variable got incremented again before job 1 got finished.
- So, when Job 1 actually got finished, then the wrong value of counter produced the log ‘*Job 2 has finished*’ followed by the ‘*Job 2 has finished*’ for the actual job 2 or vice versa as it is dependent on scheduler.
- So we see that its **not the repetitive log but the wrong value of the ‘counter’ variable** that is the problem.
- The actual problem was the usage of the variable ‘counter’ by a second thread when the first thread was using or about to use it.
- In other words, we can say that **lack of synchronization between the threads while using the shared resource ‘counter’ caused the problems** or in one word we can say that this problem happened due to ‘Synchronization problem’ between two threads.
- **How to solve it ?**



# MUTEX

- A Mutex is a lock that we set before using a shared resource and release after using it.
- When the lock is set, no other thread can access the locked region of code.
- So we see that even if thread 2 is scheduled while thread 1 was not done accessing the shared resource and the code is locked by thread 1 using mutexes then thread 2 cannot even access that region of code.
- So this ensures synchronized access of shared resources in the code.



**An image to illustrate it:**

![Mutex_lock_for_linux](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405241137759.jpg?sleflearingnotes)



## Code Implements

```c
#include <pthread.h> 
#include <stdio.h> 
#include <stdlib.h> 
#include <string.h> 
#include <unistd.h> 

pthread_t tid[2]; 
int counter; 
pthread_mutex_t lock; 

void* trythis(void* arg) 
{ 
	pthread_mutex_lock(&lock); 

	unsigned long i = 0; 
	counter += 1; 
	printf("\n Job %d has started\n", counter); 

	for (i = 0; i < (0xFFFFFFFF); i++) 
		; 

	printf("\n Job %d has finished\n", counter); 

	pthread_mutex_unlock(&lock); 

	return NULL; 
} 

int main(void) 
{ 
	int i = 0; 
	int error; 

	if (pthread_mutex_init(&lock, NULL) != 0) { 
		printf("\n mutex init has failed\n"); 
		return 1; 
	} 

	while (i < 2) { 
		error = pthread_create(&(tid[i]), 
							NULL, 
							&trythis, NULL); 
		if (error != 0) 
			printf("\nThread can't be created :[%s]", 
				strerror(error)); 
		i++; 
	} 

	pthread_join(tid[0], NULL); 
	pthread_join(tid[1], NULL); 
	pthread_mutex_destroy(&lock); 

	return 0; 
} 

```



## OUTPUT

![Screenshot 2024-05-24 at 11.48.00](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405241148625.png?sleflearingnotes)