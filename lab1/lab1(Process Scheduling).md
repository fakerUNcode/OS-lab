# LAB 1(Fork)

## Explanation of `Fork` code

```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>

int main(int argc, char const* argv[])
{
	pid_t pid; // parent process id
	pid_t cid; // child process id

	printf("Before fork Process id : %d\n",getpid() );

	fork();
  // Fork, which means different branches in English, in process scheduling it 		 means create a new process which plays the role of the child of the 					 current process.

	printf("After fork, Process id : %d\n",getpid());

	pause();

	return 0;
}
```

#### instruction

pid_t is a type of Integer which is secondly defined in sys/types.h 

pid_t is used to ident the process id

#### OUTPUT:

![Screenshot 2024-05-22 at 21.48.03](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405222149736.png?sleflearingnotes)

**We perceive that:**

- After we fork, one process became two processes.
- First one of the new processes keep the original process id, another is the original process id + 1

- `printf("After fork, Process id : %d\n",getpid());` this line seems to be runned two times. WHY?
  - Parent process run one time, and the child also does.







## More research

![Screenshot 2024-05-23 at 15.05.48](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231505088.png?sleflearingnotes)

Run this program again, we get two processes.

Create a new terminal, and use the command:

```shell
ps -al
```

- `ps`: This is the basic command to report a snapshot of the current processes.
- `-a`: This option tells `ps` to display information about all processes with a terminal (tty), except session leaders. A session leader is typically the shell or other program that started a new session.
- `-l`: This stands for "long format." It provides a more detailed output, including additional columns of information about each process.

When you run `ps -al`, you get a detailed list of processes, with columns that typically include the following information:

1. **F**: The flags associated with the process.
2. **S**: The state of the process (e.g., running, sleeping, stopped).
3. **UID**: The user ID of the process owner.
4. **PID**: The process ID.
5. **PPID**: The parent process ID (the ID of the process that started this process).
6. **C**: The processor utilization percentage.
7. **PRI**: The priority of the process.
8. **NI**: The nice value, which affects the priority of the process.
9. **ADDR**: The memory address of the process.
10. **SZ**: The size of the process in memory.
11. **WCHAN**: The name of the kernel function where the process is sleeping (if applicable).
12. **TTY**: The terminal associated with the process.
13. **TIME**: The cumulative CPU time used by the process.
14. **CMD**: The command that started the process.

Get all information of process which is running on Cprogram directory

We perceive that:

- PID 67645 AND PID 67644  EXISTS
  - process 67644 is the parent of process 67645, which is illustrated by the column of **PPID(parent process id)**

![Screenshot 2024-05-23 at 15.07.44](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231507338.png?sleflearingnotes)







## Second Try

```C
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>

int main(int argc, char const* argv[])
{
	pid_t pid;
	pid_t cid;

	printf("Before fork Process id : %d\n",getpid() );

	cid = fork();

	printf("After fork, Process id : %d\n",cid);

	pause();

	return 0;
}
```

### OUTPUT

![Screenshot 2024-05-23 at 16.06.40](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231606434.png?sleflearingnotes)

WHY GET PID 0?

- if we successfully create a new process, fork will return different values to parent and child process
  - For parent process, it returns the value of child's pid
  - For child process, it returns 0

- if we failed to create, it returns -1







## Third Try

```C
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>

int mian()

{
	pid_t cid;
	printf("Before fork process id: %d\n",getpid() );

	cid = fork();

	if(cid == 0)
	{
		printf("Child process id (my parent process pid is %d) : %d\n", getpid(),getpid());
		for (int i = 0; i < 3; ++i)
		{
			printf('hello');
		}
	}else{
		printf('Parent process id: %d\n', getpid());
		for(int i = 0; i < 3; ++i)
		{
			printf('world');
		}
	}


	pause();
}
```

### OUTPUT

![Screenshot 2024-05-23 at 16.37.53](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231638773.png?sleflearingnotes)

Both of the branches of if-else is been runned, it seems to break the usual rule.

WHY?

Because we have two processes.

### Question and answer

- **Q**:In which mechanism the two process run? Parallel or Concurrent?

- **A**:Concurrent

But if it's concurrent the processes should run by switching. However we get the result that Parent and child start and end at the same time.It's caused by the too small run-times.

As we know, if processes is concurrent, every process get their own time slice.
**Since the run-times get too small, Parent an Child can immediately finish their work in  one slice!**



When run-times approaching a much larger number, we get:

![Screenshot 2024-05-23 at 16.48.30](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231648000.png?sleflearingnotes)

**Evidently, the Parent and Child share the CPU for Concurrency Running!**









## Forth

```C
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>

int main()

{
	pid_t cid;
	printf("before fork process id : %d\n", getpid());

	int value = 100;

	cid = fork();

	if(cid == 0)
	{
		printf("Child process id (my parent process pid is %d) : %d\n", getppid(),getpid());
		for (int i = 0; i < 3; ++i)
		{
			printf("hello\n");
			printf("%d\n",value);
		}
	}else{

		value = 200;
		printf("Parent process id: %d\n", getpid());
		for(int i = 0; i < 3; ++i)
		{
			printf("world\n");
			printf("%d\n",value);
		}
	}
	
}
```

### OUTPUT

![Screenshot 2024-05-23 at 16.59.21](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231659498.png?sleflearingnotes)

We should strongly perceive that:

when we change the variable `value`, Parent process printf the new modified value

but the Child still printf the old one.

WHY?

It's caused by :

> The data in Child is only a duplication of Parent, and won't change when Parent change the original data.







## Some special situation

In some operating system, if we don't add `wait(NULL)` at the branch of Parent, once the Parent ends, child will have no parent.

Child then have a new parent with PID 1, which is the first launched process in OS.

The child is trusteed to the **System Process**. It becomes **Orphan Process**(A process whose parent process no more exists).

 **It's a problem caused by Concurrency.**

### How to solve this?

> Make Parent wait their Childs
>
> `wait()` is a good way, it forces Parent could only to be returned at the time when all Child ends.

```C
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{
    pid_t cid;
    printf("Before fork process id: %d\n", getpid());
    fflush(stdout);  // Ensure the output is printed immediately

    int value = 100;

    cid = fork();

    if (cid == 0) {
        // Child process
        printf("Child process id (my parent process pid is %d): %d\n", getppid(), getpid());
        fflush(stdout);  // Ensure the output is printed immediately
        for (int i = 0; i < 3; ++i) {
            printf("hello\n");
            printf("%d\n", value);
            fflush(stdout);  // Ensure the output is printed immediately
        }
        sleep(5);  // Child process sleeps for 5 seconds
    } else {
        // Parent process
        value = 200;
        printf("Parent process id: %d\n", getpid());
        fflush(stdout);  // Ensure the output is printed immediately
        for (int i = 0; i < 3; ++i) {
            printf("world\n");
            printf("%d\n", value);
            fflush(stdout);  // Ensure the output is printed immediately
        }

        // Wait for the child process to terminate
        printf("Parent process is waiting for the child to terminate...\n");
        fflush(stdout);  // Ensure the output is printed immediately
        wait(NULL);
        printf("Child process terminated. Parent process continues...\n");
        fflush(stdout);  // Ensure the output is printed immediately
    }

    return 0;
}
```

**`wait(NULL)`**: The `wait()` system call makes the parent process wait until any of its child processes has terminated. The `NULL` argument indicates that we do not need any information about how the child process terminated.

### OUTPUT

![da7e409cafe7d803adebf685a7f028ac](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231956357.png?sleflearingnotes)

After the child finish out operations, it sleeped for 5 seconds, and the parent waited for it.







## Some more Questions

We just make some illustrations on the relation of Parent and Child, but only a section about  Concurrency Questions.

There is still question to be solve:

> What if a Child Process finished before Parent?

**Zombie Process:**
A process which has finished the execution but still has entry in the process table to report to its parent process is known as a zombie process. A child process always first becomes a zombie before being removed from the process table. The parent process reads the exit status of the child process which reaps off the child process entry from the process table.

```C
// A C program to demonstrate Zombie Process.  
// Child becomes Zombie as parent is sleeping 
// when child process exits. 
#include <stdlib.h> 
#include <sys/types.h> 
#include <unistd.h> 
int main() 
{ 
    // Fork returns process id 
    // in parent process 
    pid_t child_pid = fork(); 
  
    // Parent process  
    if (child_pid > 0) 
        sleep(50); 
  
    // Child process 
    else        
        exit(0); 
  
    return 0; 
} 
```

The child finishes its execution using exit() system call while the parent sleeps for 50 seconds, hence doesn’t call [wait()](https://en.wikipedia.org/wiki/Wait_(system_call)) and the child process’s entry still exists in the process table.

### How to prevent this 

- using wait()

```c
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{
    // Fork returns process id in parent process
    pid_t child_pid = fork();
  
    // Parent process
    if (child_pid > 0) {
        // Wait for the child to terminate 
      	wait(NULL);
        sleep(50);
    } 
    // Child process
    else {
        exit(0);
    }
  
    return 0;
}

```

After child calling "exit()", The NULL argument indicates that we do not need any information about how the child process terminated. The child is just been directly deleted form process table.So there is no zombie process exists.



- Alternative: using waitpid()

  - Using `waitpid()` allows more control over which child process to wait for, but for this simple case, `wait(NULL)` is sufficient.

  - The `waitpid` function is a more flexible version of `wait` that allows you to specify which process to wait for and provides additional options for how to wait. The function prototype for `waitpid` is:

    ```c
    pid_t waitpid(pid_t pid, int *status, int options);
    ```

```c
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{
    // Fork returns process id in parent process
    pid_t child_pid = fork();
  
    // Parent process
    if (child_pid > 0) {
        // Wait for the specific child to terminate
        waitpid(child_pid, NULL, 0);
        sleep(50);
    } 
    // Child process
    else {
        exit(0);
    }
  
    return 0;
}

```



















# Inference and Theory Research

## What happens when fork() is called

In many operating systems, the fork system call is an essential operation. The fork system call allows the creation of a new process. **When a process calls the [fork](https://www.geeksforgeeks.org/fork-system-call/)(), it duplicates itself, resulting in two processes running at the same time.** The new process that is created is called a [child process](https://www.geeksforgeeks.org/difference-between-process-parent-process-and-child-process/). It is a copy of the parent process. The [fork ](https://www.geeksforgeeks.org/fork-system-call/)system call is required for [process ](https://www.geeksforgeeks.org/introduction-of-process-management/)creation and enables many important features such as parallel processing, multitasking, and the creation of complex process hierarchies.

It develops an entirely new process with a distinct execution setting. **The new [process ](https://www.geeksforgeeks.org/process-table-and-process-control-block-pcb/)has its own [address space](https://www.geeksforgeeks.org/logical-and-physical-address-in-operating-system/), and memory, and is a perfect duplicate of the caller process.**

Only on a specific condition the copy memory will refresh: a child process trying to modify the shared memory.



There is an essential terminologies :

- Copy-on-write

Fork() system call use the strategy called "copy-on-write".

### COPY-ON-WRITE(COW)

1. **Copy**:
   - This refers to the action of creating a duplicate of a memory page.
   - In the context of COW, this copying happens **only** when necessary, i.e., when a write operation is attempted on a shared memory page.
2. **On**:
   - This is the condition or trigger for the copying action.
   - In COW, 'on' signifies that the copying occurs at the moment a specific event happens.
   - The specific event in COW is a write attempt to a shared memory page.
3. **Write**:
   - This refers to the action of modifying or writing to a memory page.
   - In the context of COW, the write operation is what triggers the copying of the shared memory page.

#### Explanation in Context

- **Shared Memory Pages**: Initially, when a new process is created (like through `fork()`), it shares the same memory pages with its parent process. This is efficient because it avoids unnecessary duplication of memory.
- **Modification Attempt (Write)**: When one of these processes tries to write to a shared memory page, it triggers the need for a copy.
- **Trigger (On)**: The act of attempting to write to a shared page is the trigger that initiates the copying process.
- **Copy Creation**: When the write attempt is detected, the operating system creates a separate copy of the memory page specifically for the process attempting the modification. This ensures that the original shared memory page remains unchanged for the other process.

#### Visual Representation

1. **Initial State** (Shared Memory, No Copy):

   ```
   lua
   Copy code
   +------------------+
   | Parent Process   | ---> [ Shared Memory Page ]
   +------------------+
                        |
   +------------------+ | 
   | Child Process    | ---> [ Shared Memory Page ]
   +------------------+
   ```

2. **Write Attempt by Child Process**:

   - The child process tries to modify (write to) the shared memory page.

3. **Trigger (On Write)**:

   - The write attempt triggers the copying process.

4. **Copy Creation (Copy)**:

   ```
   lua
   Copy code
   +------------------+
   | Parent Process   | ---> [ Shared Memory Page ]
   +------------------+
                        |
   +------------------+ | 
   | Child Process    | ---> [ New Copy of Memory Page ] (modified)
   +------------------+
   ```

So, in essence, Copy-On-Write (COW) is a strategy to delay the copying of memory pages until the moment they are written to, thus optimizing memory usage and improving efficiency by only creating copies when absolutely necessary.







## Return values of fork()

The fork system call’s return value gives both the parent and child process information. It [assists ](https://www.geeksforgeeks.org/project-idea-assist-bot/)in handling mistakes during process formation and determining the execution route.



![Fork() System call](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OS202405231536721.png?sleflearingnotes)

The return value of the fork call can be used by the parent to determine the execution path of the child process. If it returns 0 then it is executing the [child process](https://www.geeksforgeeks.org/difference-between-process-parent-process-and-child-process/), if it returns -1 then there is some error; and if it returns some positive value, then it is the [PID ](https://www.geeksforgeeks.org/node-js-process-pid-property/)of the child process.







## Advantages of Fork System Call

- Creating new processes with the fork system call facilitates the running of several tasks concurrently within an [operating system](https://www.geeksforgeeks.org/what-is-an-operating-system/). The system’s efficiency and multitasking skills are improved by this [concurrency](https://www.geeksforgeeks.org/concurrency-in-operating-system/).
- Code reuse: The child process **inherits an exact duplicate of the parent process, including every [code segment](https://www.geeksforgeeks.org/memory-layout-of-c-program/),** when the fork system call is used. By using existing code, this feature encourages code reuse and **streamlines the creation of complicated programmes**.
- Memory Optimisation: When using the fork system call, the copy-on-write method **optimises the use of memory**. ***Initial memory overhead is minimised since parent and child processes share the same physical [memory ](https://www.geeksforgeeks.org/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/)pages***. Only when a process changes a shared memory page, improving memory efficiency, does copying take place.
- Process isolation is achieved by giving each process started by the [fork system](https://www.geeksforgeeks.org/fork-system-call/) call its own memory area and set of resources. System stability and security are improved because of this isolation, which [prevents processes](https://www.geeksforgeeks.org/zombie-processes-prevention/) from interfering with one another.





## Disadvantages of Fork System Call

- Memory Overhead: The fork system call has memory overhead even with the copy-on-write optimisation. The [parent process](https://www.geeksforgeeks.org/difference-between-process-parent-process-and-child-process/) is first copied in its entirety, including all of its memory, which increases memory use.
- Duplication of Resources: When a process forks, the[ child process](https://www.geeksforgeeks.org/node-js-child-process/) duplicates all open file descriptors, network connections, and other resources. This duplication may waste resources and perhaps result in inefficiencies.
- Communication complexity: The [fork system call ](https://www.geeksforgeeks.org/fork-system-call/)generates independent processes that can need to coordinate and communicate with one another. To enable data transmission across processes, interprocess communication methods, such as pipes or shared memory, must be built, which might add complexity.
- Impact on System Performance: Forking a process duplicates memory allocation, [resource management](https://www.geeksforgeeks.org/automatic-resource-management-java/), and other system tasks. Performance of the system may be affected by this, particularly in situations where processes are often started and stopped.





## Conclusion

In conclusion, the [fork system](https://www.geeksforgeeks.org/fork-system-call/) call in operating systems offers a potent mechanism for process formation. It enables the execution of numerous tasks concurrently and the effective use of system resources by replicating the parent process. It’s crucial for the creation of reliable and effective [operating systems](https://www.geeksforgeeks.org/what-is-an-operating-system/) to comprehend the complexities of the[ fork system call.](https://www.geeksforgeeks.org/fork-system-call/)