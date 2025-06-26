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
