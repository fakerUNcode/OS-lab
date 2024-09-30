# FCFS

```c
// C program for implementation of FCFS  
// scheduling 
#include<stdio.h> 
// Function to find the waiting time for all  
// processes 
void findWaitingTime(int processes[], int n,  
                          int bt[], int wt[]) 
{ 
    // waiting time for first process is 0 
    wt[0] = 0; 
   
    // calculating waiting time 
    for (int  i = 1; i < n ; i++ ) 
        wt[i] =  bt[i-1] + wt[i-1] ; 
} 
   
// Function to calculate turn around time 
void findTurnAroundTime( int processes[], int n,  
                  int bt[], int wt[], int tat[]) 
{ 
    // calculating turnaround time by adding 
    // bt[i] + wt[i] 
    for (int  i = 0; i < n ; i++) 
        tat[i] = bt[i] + wt[i]; 
} 
   
//Function to calculate average time 
void findavgTime( int processes[], int n, int bt[]) 
{ 
    int wt[n], tat[n], total_wt = 0, total_tat = 0; 
   
    //Function to find waiting time of all processes 
    findWaitingTime(processes, n, bt, wt); 
   
    //Function to find turn around time for all processes 
    findTurnAroundTime(processes, n, bt, wt, tat); 
   
    //Display processes along with all details 
    printf("Processes   Burst time   Waiting time   Turn around time\n"); 
   
    // Calculate total waiting time and total turn  
    // around time 
    for (int  i=0; i<n; i++) 
    { 
        total_wt = total_wt + wt[i]; 
        total_tat = total_tat + tat[i]; 
        printf("   %d ",(i+1));
        printf("       %d ", bt[i] );
        printf("       %d",wt[i] );
        printf("       %d\n",tat[i] ); 
    } 
    float s=(float)total_wt / (float)n;
    float t=(float)total_tat / (float)n;
    printf("Average waiting time = %f",s);
    printf("\n");
    printf("Average turn around time = %f ",t); 
} 
   
// Driver code 
int main() 
{ 
    //process id's 
    int processes[] = { 1, 2, 3}; 
    int n = sizeof processes / sizeof processes[0]; 
   
    //Burst time of all processes 
    int  burst_time[] = {10, 5, 8}; 
   
    findavgTime(processes, n,  burst_time); 
    return 0; 
} 
// This code is contributed by Shivi_Aggarwal
```

Certainly! Here is a detailed note to help you understand the FCFS (First-Come, First-Served) scheduling program written in C:

### FCFS Scheduling in C

#### Code Breakdown:

1. **Include the Standard I/O Library:**
   ```c
   #include<stdio.h>
   ```
   This line includes the standard input-output library necessary for using `printf` and other standard I/O functions.

2. **Function to Find Waiting Time:**
   
   ```c
   void findWaitingTime(int processes[], int n, int bt[], int wt[])
   {
       wt[0] = 0; // Waiting time for the first process is always 0
   
       // Calculate waiting time for each process
       for (int i = 1; i < n; i++)
           wt[i] = bt[i-1] + wt[i-1];
   }
   ```
   - **Parameters:**
     - `processes[]`: Array of process IDs.
     - `n`: Number of processes.
     - `bt[]`: Burst time of each process.
     - `wt[]`: Array to store waiting time for each process.
   - **Explanation:**
     - The waiting time for the first process is 0 because it starts immediately.
     - For subsequent processes, the waiting time is calculated as the burst time of the previous process plus the waiting time of the previous process.
   
3. **Function to Find Turnaround Time:**
   
   ```c
   void findTurnAroundTime(int processes[], int n, int bt[], int wt[], int tat[])
   {
       for (int i = 0; i < n; i++)
           tat[i] = bt[i] + wt[i];
   }
   ```
   - **Parameters:**
     - `processes[]`: Array of process IDs.
     - `n`: Number of processes.
     - `bt[]`: Burst time of each process.
     - `wt[]`: Waiting time of each process.
     - `tat[]`: Array to store turnaround time for each process.
   - **Explanation:**
     - Turnaround time is the total time taken for a process to complete after it has been submitted. It is calculated as the burst time plus the waiting time for each process.
   
4. **Function to Calculate Average Times and Print Details:**
   
   ```c
   void findavgTime(int processes[], int n, int bt[])
   {
       int wt[n], tat[n], total_wt = 0, total_tat = 0;
   
       // Calculate waiting time for all processes
       findWaitingTime(processes, n, bt, wt);
   
       // Calculate turnaround time for all processes
       findTurnAroundTime(processes, n, bt, wt, tat);
   
       // Print process details
       printf("Processes   Burst time   Waiting time   Turn around time\n");
   
       for (int i = 0; i < n; i++)
       {
           total_wt += wt[i];
           total_tat += tat[i];
           printf("   %d        %d            %d               %d\n", processes[i], bt[i], wt[i], tat[i]);
       }
   
       float avg_wt = (float)total_wt / (float)n;
       float avg_tat = (float)total_tat / (float)n;
       printf("Average waiting time = %f\n", avg_wt);
       printf("Average turn around time = %f\n", avg_tat);
   }
   ```
   - **Explanation:**
     - This function calls `findWaitingTime` and `findTurnAroundTime` to get the waiting and turnaround times for all processes.
     - It then calculates the total waiting time and total turnaround time.
     - The process details, including burst time, waiting time, and turnaround time, are printed for each process.
     - Finally, the average waiting time and average turnaround time are calculated and printed.
   
5. **Main Function (Driver Code):**
   ```c
   int main()
   {
       // Process IDs
       int processes[] = {1, 2, 3};
       int n = sizeof(processes) / sizeof(processes[0]);
   
       // Burst time of all processes
       int burst_time[] = {10, 5, 8};
   
       findavgTime(processes, n, burst_time);
       return 0;
   }
   ```
   - **Explanation:**
     - This is the starting point of the program.
     - It initializes the process IDs and burst times.
     - It calls `findavgTime` to calculate and display the waiting time, turnaround time, and their averages.

#### Example Output:
```
Processes   Burst time   Waiting time   Turn around time
   1        10            0               10
   2         5           10               15
   3         8           15               23
Average waiting time = 8.333333
Average turn around time = 16.000000
```



















# SJF

```c
// CPU-Scheduling-Algorithm-In-C
// Shortest Job First(SJF) Scheduling Algorithm(Non Pre-emptive)

#include<stdio.h>
#include<malloc.h>

void main()
{
	int n, i, j, pos, temp, *bt, *wt, *tat, *p;
	float avgwt = 0, avgtat = 0;
	printf("\n Enter the number of processes : ");
    scanf("%d", &n);

    p = (int*)malloc(n*sizeof(int));
    bt = (int*)malloc(n*sizeof(int));
    wt = (int*)malloc(n*sizeof(int));
    tat = (int*)malloc(n*sizeof(int));
 	
 	printf("\n Enter the burst time for each process \n");
    for(i=0; i<n; i++)
    {
        printf(" Burst time for Process%d : ", i);
        scanf("%d", &bt[i]);
        p[i] = i;
    }

    for(i=0; i<n; i++)
    {
    	pos = i;
    	for(j=i+1; j<n; j++)
    	{
    		if(bt[j] < bt[pos])
    		{
    			pos = j;
    		}
    	}
    	temp = bt[i];
    	bt[i] = bt[pos];
    	bt[pos] = temp;

    	temp = p[i];
    	p[i] = p[pos];
    	p[pos] = temp;
    }

    wt[0] = 0;
    tat[0] = bt[0];
    for(i=1; i<n; i++)
    {
        wt[i] = wt[i-1] + bt[i-1];  //waiting time[p] = waiting time[p-1] + Burst Time[p-1]
        tat[i] = wt[i] + bt[i];     //Turnaround Time = Waiting Time + Burst Time
    }

    for(i=0; i<n; i++)
    {
        avgwt += wt[i];
        avgtat += tat[i]; 
    }
    avgwt = avgwt/n;
    avgtat = avgtat/n;

    printf("\n PROCESS \t BURST TIME \t WAITING TIME \t TURNAROUND TIME \n");
    printf("--------------------------------------------------------------\n");
    for(i=0; i<n; i++)
    {
        printf(" P%d \t\t %d \t\t %d \t\t %d \n", p[i], bt[i], wt[i], tat[i]);
    }

    printf("\n Average Waiting Time = %f \n Average Turnaround Time = %f \n", avgwt, avgtat);

    printf("\n GAANT CHART \n");
    printf("---------------\n");
    for(i=0; i<n; i++)
    {
        printf(" %d\t|| P%d ||\t%d\n", wt[i], p[i], tat[i]);
    }
}
```

### SJF Scheduling (Non-Preemptive) in C

#### Code Breakdown:

1. **Include Necessary Libraries:**
   ```c
   #include<stdio.h>
   #include<malloc.h>
   ```
   - `stdio.h`: Standard input-output library for using `printf` and `scanf`.
   - `malloc.h`: Memory allocation library for using `malloc`.

2. **Main Function:**
   
   ```c
   void main()
   {
       int n, i, j, pos, temp, *bt, *wt, *tat, *p;
       float avgwt = 0, avgtat = 0;
       printf("\n Enter the number of processes : ");
       scanf("%d", &n);
   
       p = (int*)malloc(n*sizeof(int));
       bt = (int*)malloc(n*sizeof(int));
       wt = (int*)malloc(n*sizeof(int));
       tat = (int*)malloc(n*sizeof(int));
   ```
   - **Explanation:**
     - Variables are declared for the number of processes (`n`), loop counters (`i` and `j`), position (`pos`), temporary storage (`temp`), burst time array (`bt`), waiting time array (`wt`), turnaround time array (`tat`), and process ID array (`p`).
     - The `avgwt` and `avgtat` variables are initialized to store the average waiting and turnaround times.
     - Memory is *allocated* for the arrays using `malloc`.
   
3. **Input Burst Times:**
   ```c
       printf("\n Enter the burst time for each process \n");
       for(i=0; i<n; i++)
       {
           printf(" Burst time for P%d : ", i);
           scanf("%d", &bt[i]);
           p[i] = i;
       }
   ```
   - **Explanation:**
     - The program prompts the user to enter the burst time for each process.
     - Burst times are stored in the `bt` array, and process IDs are stored in the `p` array.

4. **Sorting Burst Times and Processes:**
   ```c
       for(i=0; i<n; i++)
       {
           pos = i;
           for(j=i+1; j<n; j++)
           {
               if(bt[j] < bt[pos])
               {
                   pos = j;
               }
           }
           temp = bt[i];
           bt[i] = bt[pos];
           bt[pos] = temp;
   
           temp = p[i];
           p[i] = p[pos];
           p[pos] = temp;
       }
   ```
   - **Explanation:**
     - The outer loop iterates through each process.
     - The inner loop finds the process with the smallest burst time from the unsorted portion of the array.
     - Burst times and corresponding process IDs are swapped to sort the burst times in ascending order.

5. **Calculate Waiting Time and Turnaround Time:**
   ```c
       wt[0] = 0;
       tat[0] = bt[0];
       for(i=1; i<n; i++)
       {
           wt[i] = wt[i-1] + bt[i-1];  // Waiting time for current process = waiting time of previous process + burst time of previous process
           tat[i] = wt[i] + bt[i];     // Turnaround time = waiting time + burst time
       }
   ```
   - **Explanation:**
     - The waiting time for the first process is 0.
     - The turnaround time for the first process is its burst time.
     - For each subsequent process, the waiting time is the sum of the waiting time and burst time of the previous process.
     - The turnaround time is calculated as the sum of the waiting time and burst time.

6. **Calculate Averages:**
   ```c
       for(i=0; i<n; i++)
       {
           avgwt += wt[i];
           avgtat += tat[i];
       }
       avgwt = avgwt / n;
       avgtat = avgtat / n;
   ```
   - **Explanation:**
     - Sum the waiting times and turnaround times of all processes.
     - Calculate the average waiting time and average turnaround time by dividing the totals by the number of processes.

7. **Print Results:**
   ```c
       printf("\n PROCESS \t BURST TIME \t WAITING TIME \t TURNAROUND TIME \n");
       printf("--------------------------------------------------------------\n");
       for(i=0; i<n; i++)
       {
           printf(" P%d \t\t %d \t\t %d \t\t %d \n", p[i], bt[i], wt[i], tat[i]);
       }
   
       printf("\n Average Waiting Time = %f \n Average Turnaround Time = %f \n", avgwt, avgtat);
   ```
   - **Explanation:**
     - Print the burst time, waiting time, and turnaround time for each process.
     - Print the average waiting time and average turnaround time.

8. **Print Gantt Chart:**
   ```c
       printf("\n GAANT CHART \n");
       printf("---------------\n");
       for(i=0; i<n; i++)
       {
           printf(" %d\t|| P%d ||\t%d\n", wt[i], p[i], tat[i]);
       }
   }
   ```
   - **Explanation:**
     - Print a Gantt chart to visualize the process execution order and their start and end times.

### Example Input and Output:

#### Example Input:

```c
perl
Copy code
Enter the number of processes: 3
Enter the burst time for each process
Burst time for P1: 8
Burst time for P2: 4
Burst time for P3: 2
```

#### Example Output:

```c
PROCESS         BURST TIME      WAITING TIME    TURNAROUND TIME
--------------------------------------------------------------
 P3             2               0               2
 P2             4               2               6
 P1             8               6               14

Average Waiting Time = 2.666667 
Average Turnaround Time = 7.333333 

GAANT CHART 
---------------
 0      || P3 ||        2
 2      || P2 ||        6
 6      || P1 ||        14
```





















# Priority

```c
#include<stdio.h>
#include<stdlib.h>

int main() {
    int n, i, j, pos, temp, *bt, *wt, *tat, *p, *pt; // Declaring pointers for burst time, waiting time, turnaround time, process IDs, and priorities
    float avgwt = 0, avgtat = 0; // Variables to store average waiting time and average turnaround time

    // Getting the number of processes from the user
    printf("\n Enter the number of processes : ");
    scanf("%d", &n);

    // Allocating memory dynamically for arrays
    p = (int*)malloc(n * sizeof(int));
    bt = (int*)malloc(n * sizeof(int));
    wt = (int*)malloc(n * sizeof(int));
    tat = (int*)malloc(n * sizeof(int));
    pt = (int*)malloc(n * sizeof(int));

    // Getting burst time and priority for each process from the user
    printf("\n Enter the burst time and priority for each process ");
    for (i = 0; i < n; i++) {
        printf("\n Burst time of P%d : ", i);
        scanf("%d", &bt[i]);
        printf(" Priority of P%d : ", i);
        scanf("%d", &pt[i]);
        p[i] = i; // Assigning process IDs
    }

    // Sorting processes based on priority using selection sort
    for (i = 0; i < n; i++) {
        pos = i;
        for (j = i + 1; j < n; j++) {
            if (pt[j] < pt[pos]) { // Finding process with minimum priority
                pos = j;
            }
        }
        // Swapping burst time, priority, and process IDs
        temp = pt[i];
        pt[i] = pt[pos];
        pt[pos] = temp;

        temp = bt[i];
        bt[i] = bt[pos];
        bt[pos] = temp;

        temp = p[i];
        p[i] = p[pos];
        p[pos] = temp;
    }

    // Calculating waiting time and turnaround time for each process
    wt[0] = 0;
    tat[0] = bt[0];
    for (i = 1; i < n; i++) {
        wt[i] = wt[i - 1] + bt[i - 1];  // Waiting time = Previous waiting time + Previous burst time
        tat[i] = wt[i] + bt[i];          // Turnaround time = Waiting time + Burst time
    }

    // Calculating total waiting time and total turnaround time
    for (i = 0; i < n; i++) {
        avgwt += wt[i];
        avgtat += tat[i];
    }
    // Calculating average waiting time and average turnaround time
    avgwt = avgwt / n;
    avgtat = avgtat / n;

    // Displaying process details including priority, burst time, waiting time, and turnaround time
    printf("\n PROCESS \t PRIORITY \t BURST TIME \t WAITING TIME \t TURNAROUND TIME \n");
    printf("--------------------------------------------------------------\n");
    for (i = 0; i < n; i++) {
        printf(" P%d \t\t %d \t\t %d \t\t %d \t\t %d \n", p[i], pt[i], bt[i], wt[i], tat[i]);
    }

    // Displaying average waiting time and average turnaround time
    printf("\n Average Waiting Time = %f \n Average Turnaround Time = %f \n", avgwt, avgtat);

    // Displaying Gantt chart
    printf("\n GAANT CHART \n");
    printf("---------------\n");
    for (i = 0; i < n; i++) {
        printf(" %d\t|| P%d ||\t%d\n", wt[i], p[i], tat[i]);
    }

    // Freeing dynamically allocated memory
    free(p);
    free(bt);
    free(wt);
    free(tat);
    free(pt);

    return 0;
}

```

Let's elaborate on each part of the program:

```c
#include<stdio.h>
#include<stdlib.h>

int main() {
    int n, i, j, pos, temp, *bt, *wt, *tat, *p, *pt; // Declaring pointers for burst time, waiting time, turnaround time, process IDs, and priorities
    float avgwt = 0, avgtat = 0; // Variables to store average waiting time and average turnaround time
```

- The program begins by including necessary header files, `stdio.h` for input/output operations and `stdlib.h` for dynamic memory allocation.
- Variables and pointers are declared to store the number of processes (`n`), process IDs (`p`), burst times (`bt`), priorities (`pt`), waiting times (`wt`), and turnaround times (`tat`).
- `avgwt` and `avgtat` are initialized to store the average waiting time and average turnaround time respectively.

```c
    printf("\n Enter the number of processes : ");
    scanf("%d", &n);
```

- The user is prompted to enter the number of processes.

```c
    p = (int*)malloc(n * sizeof(int));
    bt = (int*)malloc(n * sizeof(int));
    wt = (int*)malloc(n * sizeof(int));
    tat = (int*)malloc(n * sizeof(int));
    pt = (int*)malloc(n * sizeof(int));
```

- Memory is dynamically allocated for arrays to store process IDs, burst times, waiting times, turnaround times, and priorities based on the number of processes entered by the user.

```c
    printf("\n Enter the burst time and priority for each process ");
    for (i = 0; i < n; i++) {
        printf("\n Burst time of P%d : ", i);
        scanf("%d", &bt[i]);
        printf(" Priority of P%d : ", i);
        scanf("%d", &pt[i]);
        p[i] = i; // Assigning process IDs
    }
```

- The user is prompted to enter the burst time and priority for each process.
- Values entered by the user are stored in the respective arrays `bt` and `pt`.
- Process IDs (`p`) are assigned incrementally.

```c
    for (i = 0; i < n; i++) {
        pos = i;
        for (j = i + 1; j < n; j++) {
            if (pt[j] < pt[pos]) { // Finding process with minimum priority
                pos = j;
            }
        }
        // Swapping burst time, priority, and process IDs
        temp = pt[i];
        pt[i] = pt[pos];
        pt[pos] = temp;

        temp = bt[i];
        bt[i] = bt[pos];
        bt[pos] = temp;

        temp = p[i];
        p[i] = p[pos];
        p[pos] = temp;
    }
```

- The processes are sorted based on priority using the selection sort algorithm.
- The arrays `bt`, `pt`, and `p` are sorted together to maintain the association between burst time, priority, and process IDs.

```c
    wt[0] = 0;
    tat[0] = bt[0];
    for (i = 1; i < n; i++) {
        wt[i] = wt[i - 1] + bt[i - 1];  // Waiting time = Previous waiting time + Previous burst time
        tat[i] = wt[i] + bt[i];          // Turnaround time = Waiting time + Burst time
    }
```

- Waiting time and turnaround time are calculated for each process.
- Waiting time for the first process is set to 0, and for subsequent processes, it's the sum of previous waiting time and previous burst time.
- Turnaround time for each process is the sum of waiting time and burst time.

```c
    for (i = 0; i < n; i++) {
        avgwt += wt[i];
        avgtat += tat[i];
    }
```

- Total waiting time and total turnaround time are calculated by summing up the values for all processes.

```c
    avgwt = avgwt / n;
    avgtat = avgtat / n;
```

- Average waiting time and average turnaround time are computed by dividing the respective totals by the number of processes.

```c
    printf("\n PROCESS \t PRIORITY \t BURST TIME \t WAITING TIME \t TURNAROUND TIME \n");
    printf("--------------------------------------------------------------\n");
    for (i = 0; i < n; i++) {
        printf(" P%d \t\t %d \t\t %d \t\t %d \t\t %d \n", p[i], pt[i], bt[i], wt[i], tat[i]);
    }
```

- Process details including process ID, priority, burst time, waiting time, and turnaround time are displayed in tabular format.

```c
    printf("\n Average Waiting Time = %f \n Average Turnaround Time = %f \n", avgwt, avgtat);
```

- Average waiting time and average turnaround time are displayed.

```c
    printf("\n GAANT CHART \n");
    printf("---------------\n");
    for (i = 0; i < n; i++) {
        printf(" %d\t|| P%d ||\t%d\n", wt[i], p[i], tat[i]);
    }
```

- A Gantt chart representation of the processes' execution order and time intervals is printed.

```c
    free(p);
    free(bt);
    free(wt);
    free(tat);
    free(pt);

    return 0;
}
```

- Dynamically allocated memory is freed before the program terminates to avoid memory leaks.





# More and Conclusion

We could get a important caution :

Whatever mechanism the algorithm use, we just follow the rule and *put the process should be executed promptly in the front of the ready queue*.