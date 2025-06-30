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
```
### 2.Multilple Thread each printing its own message
```c
#include <stdio.h>
#include <string.h>
#include <pthread.h>

void* multiplethread(void* arg){
        char* msg = (char*) arg;
        printf("%s\n",msg);
        return NULL;
}

int main() {
        pthread_t tid[3];
        char* msg[] = {"thread 1", "thread 2","thread 3"};
        for (int i=0; i < 3; i++)
                pthread_create(&tid[i], NULL, multiplethread, msg[i]);
        for (int i=0; i < 3; i++)
                pthread_join(tid[i], NULL);

        return 0;
}
```
### 3. Two threads that print numbers 1 to 10
```c
#include <pthread.h>
#include <stdio.h>

void* printNumbers(void* arg) {
    for (int i = 1; i <= 10; i++) {
        printf("Thread %ld: %d\n", (long)arg, i);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_create(&t1, NULL, printNumbers, (void*)1);
    pthread_create(&t2, NULL, printNumbers, (void*)2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    return 0;
}
```
