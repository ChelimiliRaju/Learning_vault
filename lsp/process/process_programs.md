### 3. Write a C program to demonstrate the use of fork() system call.
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid == 0)
        printf("Child Process, PID = %d\n", getpid());
    else
        printf("Parent Process, PID = %d\n", getpid());
    return 0;
}
```


### 6. Write a C program to illustrate the use of the execvp() function.
```c
#include <unistd.h>

int main() {
    char *args[] = {"ls", "-l", NULL};
    execvp(args[0], args);
    return 0;
}
```


### 10. Write a program in C to create a child process using fork() and print its PID.
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid == 0)
        printf("Child PID: %d\n", getpid());
    else
        printf("Parent PID: %d\n", getpid());
    return 0;
}
```


### 15. Write a C program to create multiple child processes using fork() and display their PIDs.
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    for (int i = 0; i < 3; i++) {
        pid_t pid = fork();
        if (pid == 0) {
            printf("Child %d PID: %d\n", i+1, getpid());
            return 0;
        }
    }
    return 0;
}
```


### 19. Write a program in C to create a zombie process and explain how to avoid it.
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();
    if (pid == 0)
        _exit(0);
    else {
        sleep(10);
        wait(NULL);
    }
    return 0;
}
```


### 22. Write a C program to demonstrate the use of the waitpid() function for process synchronization.
```c
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid == 0) {
        printf("Child running...\n");
        _exit(0);
    } else {
        waitpid(pid, NULL, 0);
        printf("Child completed.\n");
    }
    return 0;
}
```


### 25. Write a program in C to create a daemon process.
```c
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid = fork();
    if (pid > 0) exit(0);

    setsid();
    chdir("/");
    close(STDIN_FILENO);
    close(STDOUT_FILENO);
    close(STDERR_FILENO);

    while (1) {
        sleep(60);
    }
}
```


### 28. Write a C program to demonstrate the use of the system() function for executing shell commands.
```c
#include <stdlib.h>

int main() {
    system("ls -l");
    return 0;
}
```


### 32. Write a C program to create a process using fork() and pass arguments to the child process.
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid == 0) {
        char *args[] = {"echo", "Hello from child", NULL};
        execvp(args[0], args);
    } else {
        wait(NULL);
        printf("Parent finished.\n");
    }
    return 0;
}
```


### 35. Write a program in C to demonstrate process synchronization using semaphores.
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/sem.h>
#include <sys/wait.h>

int main() {
    int semid = semget(IPC_PRIVATE, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, 1);

    struct sembuf p = {0, -1, 0};
    struct sembuf v = {0, 1, 0};

    if (fork() == 0) {
        semop(semid, &p, 1);
        printf("Child entered critical section\n");
        sleep(2);
        printf("Child leaving critical section\n");
        semop(semid, &v, 1);
    } else {
        semop(semid, &p, 1);
        printf("Parent entered critical section\n");
        sleep(2);
        printf("Parent leaving critical section\n");
        semop(semid, &v, 1);
        wait(NULL);
        semctl(semid, 0, IPC_RMID);
    }
    return 0;
}
```


### 39. Write a C program to demonstrate the use of the execvpe() function.
```c
#define _GNU_SOURCE
#include <unistd.h>
#include <stdio.h>

int main() {
    char *env[] = {"MYVAR=123", NULL};
    char *args[] = {"env", NULL};
    execvpe("env", args, env);
    perror("execvpe failed");
    return 1;
}
```


### 42. Write a C program to create a process group and change its process group ID (PGID).
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();
    if (pid == 0) {
        setpgid(0, 0);
        printf("Child PGID: %d\n", getpgrp());
    } else {
        wait(NULL);
        printf("Parent PGID: %d\n", getpgrp());
    }
    return 0;
}
```


### 45. Write a program in C to demonstrate inter-process communication (IPC) using shared memory.
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int shmid = shmget(IPC_PRIVATE, 1024, IPC_CREAT | 0666);
    char *str = (char*) shmat(shmid, NULL, 0);

    if (fork() == 0) {
        sleep(1);
        printf("Child reads: %s\n", str);
    } else {
        sprintf(str, "Shared Memory Message");
        wait(NULL);
        shmctl(shmid, IPC_RMID, NULL);
    }
    return 0;
}
```


### 49. Write a C program to create a child process using vfork() and demonstrate its usage.
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    pid_t pid = vfork();
    if (pid == 0) {
        printf("Child using vfork\n");
        _exit(0);
    } else {
        wait(NULL);
        printf("Parent resumed\n");
    }
    return 0;
}
```

