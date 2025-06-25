### 1.Creating File and printing content with total bytes
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
#include<stdlib.h>

int main(){
        int fd, ret;
        char msg[] = "Hello world its  new file ";
        fd = open("newfilepr.txt", O_WRONLY | O_CREAT, 0660);
       if(fd<0){
               printf("file failed to open\n");
               return;
       }
       printf("file opened successfully\n");
        ret = write( fd, msg, strlen(msg));
        if(ret<0){
                printf("failed to write in file\n");
                return;
        }
        printf("message is: %s , written %d bytes\n", msg,ret);
        close(fd);
        return 0;
}
```
### 2.Open & Read File content
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    int fd = open("newfilepr.txt", O_RDONLY);
    if (fd < 0) {
        perror("open");
        return 1;
    }

    char msg[1024];
    ssize_t bytes;
    while ((bytes = read(fd, msg, sizeof(msg))) > 0)
    {
        write(STDOUT_FILENO, msg, bytes);
    }

    close(fd);
    return 0;
}
```
### 3.Make a New Test Directory
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
#include<stdlib.h>

int main(){
        if(mkdir("Test", 0755) ==0)
                        printf("directory is created\n");
        else {
                        printf("failed to creare directory\n");
                        perror("failed");
                        return 0;
        }
}
```
### 4.Write a C program to check if a file named "sample.txt" exists in the current directory?
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
#include<stdlib.h>

int main(){
        if(access("samplecheck.txt",F_OK)==0){
                printf("file exists in current directory\n");
        }
        else printf("file doesnot exist\n");
        return 0;
}
```
### 5.Rename a file from "oldname.txt" to "newname.txt"?
```c
#include<stdio.h>

int main(){
        if(rename("14_change_permission.c", "17_change_permission.c")==0)
                printf("file renamed successfully\n");
        else perror("rename");
        return 0;
}
```
