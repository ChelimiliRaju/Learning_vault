### 1.Creating File and printing content with total bytes
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
