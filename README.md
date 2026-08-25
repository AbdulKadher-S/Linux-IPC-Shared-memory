# Linux-IPC-Shared-memory
Ex06-Linux IPC-Shared-memory

# AIM:
To Write a C program that illustrates two processes communicating using shared memory.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Shared Memory

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that illustrates two processes communicating using shared memory.
```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/wait.h>

int main()
{
    int shmid;
    char *shared_memory;

    // Create shared memory
    shmid = shmget(IPC_PRIVATE, 1024, IPC_CREAT | 0666);

    if (shmid == -1)
    {
        perror("shmget");
        exit(1);
    }

    // Attach shared memory
    shared_memory = (char *)shmat(shmid, NULL, 0);

    if (shared_memory == (char *)-1)
    {
        perror("shmat");
        exit(1);
    }

    if (fork() == 0)
    {
        // Child process
        sleep(1);

        printf("Child Process\n");
        printf("Message received: %s\n", shared_memory);

        // Detach shared memory
        shmdt(shared_memory);

        exit(0);
    }
    else
    {
        // Parent process
        strcpy(shared_memory, "Hello from Parent Process!");

        printf("Parent Process\n");
        printf("Message written: %s\n", shared_memory);

        wait(NULL);

        // Detach shared memory
        shmdt(shared_memory);

        // Remove shared memory
        shmctl(shmid, IPC_RMID, NULL);
    }

    return 0;
}

```




## OUTPUT

<img width="1827" height="861" alt="image" src="https://github.com/user-attachments/assets/e206ef0b-d82c-4d36-8505-b73833203393" />

<img width="1600" height="983" alt="image" src="https://github.com/user-attachments/assets/92c52c35-f827-443c-bc41-f0bed2eb400e" />


# RESULT:
The program is executed successfully.
