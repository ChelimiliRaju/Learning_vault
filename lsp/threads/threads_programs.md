### 1.Create Thread to print Hello World
```c
#include <stdio.h>
#include <pthread.h>

void* myfunction(void* msg) {
  printf("Hello World\n");
  return NULL;
}
int main() {
  pthread_t thread;
  pthread_create(&thread, NULL, myfunction, NULL);
  pthread_join(thread, NULL);
  return 0;
}
