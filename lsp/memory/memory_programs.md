### 86. Demonstrate dynamic memory allocation using `malloc()`
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Allocate memory for one integer
    int *ptr = (int *)malloc(sizeof(int));

    // Check if memory allocation was successful
    if (ptr == NULL) {
        printf("Memory not allocated.\n");
        return 1;
    }

    *ptr = 10; // Assign value
    printf("Value = %d\n", *ptr);

    free(ptr); // Free allocated memory
    return 0;
}
```

---

### 87. Allocate memory for an array dynamically using `calloc()`
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr;
    int n = 5;

    // Allocate memory for 5 integers and initialize to zero
    arr = (int *)calloc(n, sizeof(int));

    if (arr == NULL) {
        printf("Memory not allocated.\n");
        return 1;
    }

    // Print default values (should be 0s)
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);

    free(arr); // Free memory
    return 0;
}
```

---

### 88. Resize dynamically allocated memory using `realloc()`
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Allocate memory for 3 integers
    int *arr = malloc(3 * sizeof(int));
    if (arr == NULL) return 1;

    for (int i = 0; i < 3; i++)
        arr[i] = i + 1; // Fill initial values

    // Resize memory to hold 5 integers
    arr = realloc(arr, 5 * sizeof(int));
    if (arr == NULL) return 1;

    for (int i = 3; i < 5; i++)
        arr[i] = i + 1; // Fill new slots

    // Print full array
    for (int i = 0; i < 5; i++)
        printf("%d ", arr[i]);

    free(arr); // Free memory
    return 0;
}
```

---

### 89. Allocate memory for a linked list node dynamically
```c
#include <stdio.h>
#include <stdlib.h>

// Define a node structure
struct Node {
    int data;
    struct Node *next;
};

int main() {
    // Dynamically allocate memory for one node
    struct Node *node = (struct Node *)malloc(sizeof(struct Node));

    if (node == NULL) {
        printf("Memory allocation failed.\n");
        return 1;
    }

    node->data = 10;      // Set data
    node->next = NULL;    // Set next pointer to NULL

    printf("Node data = %d\n", node->data);

    free(node); // Free the node
    return 0;
}
```
---

### 90. First-Fit Memory Allocation Simulation
```c
#include <stdio.h>

int main() {
    int mem[5] = {100, 500, 200, 300, 600};
    int process[4] = {212, 417, 112, 426};
    
    for (int i = 0; i < 4; i++) {
        int allocated = 0;
        for (int j = 0; j < 5; j++) {
            if (mem[j] >= process[i]) {
                printf("Process %d allocated in block %d\n", i + 1, j + 1);
                mem[j] -= process[i];
                allocated = 1;
                break;
            }
        }
        if (!allocated)
            printf("Process %d not allocated\n", i + 1);
    }

    return 0;
}
```

---

### 91. Best-Fit Memory Allocation Simulation
```c
#include <stdio.h>

int main() {
    int mem[5] = {100, 500, 200, 300, 600};
    int process[4] = {212, 417, 112, 426};

    for (int i = 0; i < 4; i++) {
        int best = -1;
        for (int j = 0; j < 5; j++) {
            if (mem[j] >= process[i]) {
                if (best == -1 || mem[j] < mem[best])
                    best = j;
            }
        }
        if (best != -1) {
            printf("Process %d allocated in block %d\n", i + 1, best + 1);
            mem[best] -= process[i];
        } else {
            printf("Process %d not allocated\n", i + 1);
        }
    }

    return 0;
}
```

---

### 92. Worst-Fit Memory Allocation Simulation
```c
#include <stdio.h>

int main() {
    int mem[5] = {100, 500, 200, 300, 600};
    int process[4] = {212, 417, 112, 426};

    for (int i = 0; i < 4; i++) {
        int worst = -1;
        for (int j = 0; j < 5; j++) {
            if (mem[j] >= process[i]) {
                if (worst == -1 || mem[j] > mem[worst])
                    worst = j;
            }
        }
        if (worst != -1) {
            printf("Process %d allocated in block %d\n", i + 1, worst + 1);
            mem[worst] -= process[i];
        } else {
            printf("Process %d not allocated\n", i + 1);
        }
    }

    return 0;
}
```

---

### 93. Next-Fit Memory Allocation Simulation
```c
#include <stdio.h>

int main() {
    int mem[5] = {100, 500, 200, 300, 600};
    int process[4] = {212, 417, 112, 426};
    int last = 0;

    for (int i = 0; i < 4; i++) {
        int j = last, count = 0;
        while (count < 5) {
            if (mem[j] >= process[i]) {
                printf("Process %d allocated in block %d\n", i + 1, j + 1);
                mem[j] -= process[i];
                last = j;
                break;
            }
            j = (j + 1) % 5;
            count++;
        }
        if (count == 5)
            printf("Process %d not allocated\n", i + 1);
    }

    return 0;
}
```

---

### 94. Buddy System Allocator (Simplified)
```c
#include <stdio.h>
#include <math.h>

int nextPowerOf2(int n) {
    int power = 1;
    while (power < n) power *= 2;
    return power;
}

int main() {
    int req = 50;
    int block = nextPowerOf2(req);
    printf("Allocating block of size: %d (buddy system)\n", block);
    return 0;
}
```

---

### 95. Custom Allocator Simulation (Fixed-size blocks)
```c
#include <stdio.h>

#define BLOCK_SIZE 64
#define TOTAL_BLOCKS 10

int blocks[TOTAL_BLOCKS] = {0};

int allocate() {
    for (int i = 0; i < TOTAL_BLOCKS; i++) {
        if (!blocks[i]) {
            blocks[i] = 1;
            return i;
        }
    }
    return -1;
}

int main() {
    int block = allocate();
    if (block != -1)
        printf("Block %d allocated\n", block);
    else
        printf("No blocks available\n");

    return 0;
}
```

---

### 96. Demonstrate memory mapping using `mmap()`
```c
#include <stdio.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <unistd.h>

int main() {
    int fd = open("file.txt", O_RDONLY);
    char *data = mmap(NULL, 100, PROT_READ, MAP_PRIVATE, fd, 0);
    if (data == MAP_FAILED) return 1;

    write(1, data, 100);  // write to stdout
    munmap(data, 100);
    close(fd);
    return 0;
}
```

---

### 97. Read and write to a memory-mapped file
```c
#include <stdio.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <string.h>
#include <unistd.h>

int main() {
    int fd = open("map.txt", O_RDWR | O_CREAT, 0666);
    ftruncate(fd, 100);

    char *data = mmap(NULL, 100, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (data == MAP_FAILED) return 1;

    strcpy(data, "Memory-mapped I/O works!");
    munmap(data, 100);
    close(fd);
    return 0;
}
```

---

### 98. Shared memory using `shmget()` and `shmat()`
```c
#include <stdio.h>
#include <sys/shm.h>
#include <string.h>

int main() {
    int shmid = shmget(1234, 1024, IPC_CREAT | 0666);
    char *data = shmat(shmid, NULL, 0);

    strcpy(data, "Shared memory working!");
    printf("Data: %s\n", data);

    shmdt(data);
    shmctl(shmid, IPC_RMID, NULL);
    return 0;
}
```

---

### 99. Create a shared memory segment and synchronize access using semaphores
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <string.h>
#include <unistd.h>

union semun {
    int val;
};

int main() {
    key_t key = ftok("shmfile", 65);
    int shmid = shmget(key, 1024, 0666|IPC_CREAT);
    char *data = shmat(shmid, NULL, 0);

    // Create semaphore
    int semid = semget(key, 1, 0666 | IPC_CREAT);
    union semun u;
    u.val = 1;
    semctl(semid, 0, SETVAL, u);

    struct sembuf lock = {0, -1, 0}; // wait
    struct sembuf unlock = {0, 1, 0}; // signal

    // Lock before writing
    semop(semid, &lock, 1);
    strcpy(data, "Shared memory with semaphore sync!");
    printf("Written: %s\n", data);
    semop(semid, &unlock, 1); // Unlock

    shmdt(data);
    shmctl(shmid, IPC_RMID, NULL);
    semctl(semid, 0, IPC_RMID);
    return 0;
}
```

---

### 100. Simulate page replacement algorithms: FIFO, LRU, Optimal

---

#### FIFO (First-In-First-Out)
```c
#include <stdio.h>

int main() {
    int pages[] = {1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5};
    int n = 12, frames = 3, f[3] = {-1, -1, -1}, index = 0, faults = 0;

    for (int i = 0; i < n; i++) {
        int found = 0;
        for (int j = 0; j < frames; j++) {
            if (f[j] == pages[i]) {
                found = 1;
                break;
            }
        }
        if (!found) {
            f[index] = pages[i];
            index = (index + 1) % frames;
            faults++;
        }
    }

    printf("Page faults (FIFO): %d\n", faults);
    return 0;
}
```

---

#### LRU (Least Recently Used)
```c
#include <stdio.h>

int search(int frame[], int key, int size) {
    for (int i = 0; i < size; i++)
        if (frame[i] == key) return i;
    return -1;
}

int main() {
    int pages[] = {7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3}, n = 12, frames = 3;
    int frame[3] = {-1, -1, -1}, time[3] = {0}, t = 0, faults = 0;

    for (int i = 0; i < n; i++) {
        t++;
        int pos = search(frame, pages[i], frames);
        if (pos == -1) {
            int lru = 0;
            for (int j = 1; j < frames; j++)
                if (time[j] < time[lru]) lru = j;

            frame[lru] = pages[i];
            time[lru] = t;
            faults++;
        } else {
            time[pos] = t;
        }
    }

    printf("Page faults (LRU): %d\n", faults);
    return 0;
}
```

---

#### Optimal Page Replacement
```c
#include <stdio.h>

int findReplaceIndex(int frame[], int pages[], int curr, int n, int f) {
    int idx = -1, farthest = curr;
    for (int i = 0; i < f; i++) {
        int j;
        for (j = curr + 1; j < n; j++) {
            if (frame[i] == pages[j]) {
                if (j > farthest) {
                    farthest = j;
                    idx = i;
                }
                break;
            }
        }
        if (j == n) return i;
    }
    return (idx == -1) ? 0 : idx;
}

int main() {
    int pages[] = {7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3}, n = 12, frames = 3;
    int frame[3] = {-1, -1, -1}, faults = 0;

    for (int i = 0; i < n; i++) {
        int found = 0;
        for (int j = 0; j < frames; j++) {
            if (frame[j] == pages[i]) {
                found = 1;
                break;
            }
        }
        if (!found) {
            int empty = -1;
            for (int j = 0; j < frames; j++) {
                if (frame[j] == -1) {
                    empty = j;
                    break;
                }
            }
            if (empty != -1)
                frame[empty] = pages[i];
            else
                frame[findReplaceIndex(frame, pages, i, n, frames)] = pages[i];

            faults++;
        }
    }

    printf("Page faults (Optimal): %d\n", faults);
    return 0;
}
```
