### 1.create a new text file and write "Hello, World!" to it?
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
### 2. Develop a C program to open an existing text file and display its contents?
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
### 3.Implement a C program to create a new directory named "Test" in the current
directory?
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
### 4. Write a C program to check if a file named "sample.txt" exists in the current directory?
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
### 5. Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
```c
#include<stdio.h>

int main(){
	if(rename("14_change_permission.c", "17_change_permission.c")==0)
		printf("file renamed successfully\n");
	else perror("rename");
	return 0;
}
```
### 6. Implement a C program to delete a file named "delete_me.txt"?
```c
#include<unistd.h>
#include<stdio.h>
int main(){
	if(unlink("a.out") == 0)
		printf("file deleted successfully\n");
	else   
	       	perror("rename");
	return 0;
}
```
### 7. Write a C program to copy the contents of one file to another?
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>

int main(){
	int src = open("sample.txt", O_RDONLY);
	int dest = open("copy.txt", O_WRONLY | O_CREAT ,0644);

	if ( src < 0 || dest < 0){
		printf("file failed open");
		return 1;
	}

	char msg[64];
	ssize_t bytes;
	while((bytes = read(src, msg, sizeof(msg))) > 0)
		write(dest,  msg, bytes);
	printf("file copied successfully/n");
	close(src);
	close(dest);
	return 0;
}
```
### 8. Develop a C program to move a file from one directory to another?
```c
#include<unistd.h>
#include<stdio.h>

int main(){
	if (rename("movingfile.txt", "Test/movingfile.txt"))
			printf("file moved\n");
	else
			perror("rename");
	return 0;
}
```
### 9. Implement a C program to list all files in the current directory?
```c
#include<stdio.h>
#include<dirent.h>

int main() {
	DIR *d;
	d = opendir(".");
	if (!d){
		perror("opendir");
		return 1;
	}
	struct dirent *entry;
	while((entry = readdir(d)) != NULL){
		printf("%s\n", entry->d_name);
	}
	closedir(d);
	return 0;
}
```
### 10. Write a C program to get the size of a file named "file.txt"?
```c
#include<stdio.h>
#include<sys/stat.h>

int main() {
	struct stat st;
	if(stat("9_list_files_directory.c", &st) == 0)
		printf("size: %ld bytes\n", st.st_size);
	else
		perror("stat");
	return 0;
}
```
### 11. Develop a C program to check if a directory named "Test" exists in the current
directory?
```c
#include<stdio.h>
#include<sys/stat.h>

int main() {
	struct stat st;
	if (stat("Test", &st) == 0)
		printf("Directory exists.\n");
	else
		printf("Directory does not exist.\n");
	return 0;
}
```
#include<stdio.h>
#include<sys/stat.h>

int main() {
	if(mkdir("../Backup",0755) == 0)
		printf("Backup directory created in parent directory.\n");
	else
		perror("mkdir");
	return 0;
}
#include<stdio.h>
#include<unistd.h>

int main() {
	FILE *fp = fopen("6_delete_file.c", "r");
	if (!fp) {
		perror("fopen");
		return 1;
	}
	int lines = 0;
	char c;
	while ((c = fgetc(fp)) != EOF) {
		if (c == '\n')
			lines++;
	}

	fclose(fp);
	printf("no of lines: %d\n", lines);
	return 0;
}
#include<stdio.h>
#include<dirent.h>
#include<unistd.h>
#include<string.h>

int main() {
	DIR *d = opendir("Temp");
	if(!d){
		perror("opendir");
		return 1;
	}

	struct dirent *entry;
	char filepath[512];

	while((entry = readdir(d)) != NULL ) {
		if(entry->d_type == DT_REG) {
			snprintf(filepath, sizeof(filepath), "Temp/%s", entry->d_name);
			if (unlink(filepath) == 0)
				printf("Deleted: %s\n", filepath);
			else 
				perror("unlink");
		}
	}
	closedir(d);
	return 0;
}


#include <stdio.h>
#include <sys/stat.h>
#include <time.h>

int main() {
    struct stat st;
    if (stat("copy.txt", &st) == 0) {
        printf("Last modified: %s", ctime(&st.st_mtime));
    } else {
        perror("stat");
    }
    return 0;
}

#include<stdio.h>
#include<string.h>

int main() {
	FILE *fp = fopen("sample.txt", "a");
	if (!fp){
		perror("fopen");
		return 1;
	}
	fputs("Goodbye!\n", fp);
	printf("appended\n");
	fclose(fp);
	return 0;
}
#include <unistd.h>
#include <stdio.h>
#include <limits.h>

int main() {
    char cwd[PATH_MAX];
    if (getcwd(cwd, sizeof(cwd)))
        printf("CWD: %s\n", cwd);
    else
        perror("getcwd");
    return 0;
}

#include<stdio.h>
#include<sys/stat.h>

int main() {
	if (chmod("copy.txt", 0444) == 0)
		printf("changed to read-only\n");
	else perror("chmod");
	return 0;
}
#include<stdio.h>
#include<unistd.h>
#include<pwd.h>

int main() {
	struct passwd *pw = getpwnam("user1");
	if(!pw) {
		perror("getpwnam");
		return 1;
		}
	if (chown("permision.txt", pw->pw_uid, -1) == 0)
		printf("ownership changed\n");
	else
		perror("chown");
	
	return 0;
}


#include<stdio.h>
#include<sys/stat.h>
#include<time.h>

int main() {
	struct stat st;
	if (stat("sample.txt", &st )==0)
		printf("Last modified on: %s ", ctime(&st.st_mtime));
	else
		perror("stat");
	return 0;
}
#include<stdio.h>

int main() {
	char tmpname[] = "/tmp/tempXXXXXX";
	int fd = mkstemp(tmpname);
	if (fd < 0 ) {
		perror("mkstemp");
		return 1;
	}
	printf("Temporary file: %s\n",tmpname);
	write(fd, "some temp data\n", 15);
	close(fd);
	return 0;
}
#include<stdio.h>
#include<sys/stat.h>
int main() {
	struct stat st;

	if ( stat("/home/rajuc",&st) == 0){
		if(S_ISREG(st.st_mode))
			printf("its a file\n");
		else if(S_ISDIR(st.st_mode))
			printf("its a directory\n");
		else
			printf("other type\n");
		
	}
	else
		perror("stat");
	return 0;
}
#include <stdio.h>
#include <unistd.h>

int main() {
	if (link("source.txt", "hardlink.txt") == 0)
		printf("Hard link created\n");
	else
		perror("link");
	return 0;
}
#include <stdio.h>

int main() {
	FILE *f = fopen("day.csv", "r");
	if (!f) {
		perror("fopen");
		return 1;
	}
	char line[1024];
	while (fgets(line, sizeof(line), f))
		printf("%s",line);
	fclose(f);
	return 0;
}

#include<stdio.h>
#include<unistd.h>
#include<limits.h>
int main() {
	char cwd[PATH_MAX];
	if (getcwd(cwd, sizeof(cwd)))
		printf("CWD: %s\n",  cwd);
	else
		perror("getcwd");
	return 0;
}
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

long dir_size(const char *path) {
    DIR *d = opendir(".");
    struct dirent *e;
    struct stat st;
    char full[PATH_MAX];
    long total = 0;
    while ((e = readdir(d))) {
        if (!strcmp(e->d_name, ".") || !strcmp(e->d_name, "..")) continue;
        snprintf(full, sizeof(full), "%s/%s", path, e->d_name);
        if (stat(full, &st) == 0) {
            if (S_ISREG(st.st_mode)) total += st.st_size;
            else if (S_ISDIR(st.st_mode)) total += dir_size(full);
        }
    }
    closedir(d);
    return total;
}

int main() {
    printf("Size: %ld bytes\n", dir_size("Documents"));
    return 0;
}

#include <stdio.h>
int main() {
	FILE *src = fopen("/home/rajuc", "r");
	FILE *dst = fopen("destination.txt", "w");
	if (!src) {
		perror("Failed to open source file");
		return 1;
		}
	if (!dst) {
		fclose(src);
		perror("Failed to open destination file");
		return 1;
		}


	char c;
	while ((c = fgetc(src)) != EOF)
    	fputc(c, dst);

	fclose(src);
	fclose(dst);
	printf("File copied successfully.\n");
	return 0;


}

#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

int count_files(const char *path) {
    DIR *d = opendir(".");
    struct dirent *e;
    int c = 0;
    while ((e = readdir(d))) {
        if (e->d_type == DT_REG) c++;
    }
    closedir(d);
    return c;
}

int main() {
    printf("Files: %d\n", count_files("Images"));
    return 0;
}

#include<stdio.h>
#include<string.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<sys/types.h>
#include<unistd.h>


int main() 
{ 	int fd;
	char * myfifo = "/tmp/myfifo";

	mkfifo(myfifo,0666);

	char arr1[80], arr2[80];
	while (1) 
	{
		fd = open(myfifo, O_WRONLY);

		fgets(arr2, 80, stdin);

		write(fd, arr2, strlen(arr2)+1);
		close(fd);

		fd = open(myfifo, O_RDONLY);

		read(fd, arr1, sizeof(arr1));

		printf("User2: %s\n", arr1);
		close(fd);
	}
	return 0;
}

#include <stdio.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
    int fd1;

    // FIFO file path
    char * myfifo = "/tmp/myfifo";

    // Creating the named file(FIFO)
    // mkfifo(<pathname>, <permission>)
    mkfifo(myfifo, 0666);

    char str1[80], str2[80];
    while (1)
    {
        // First open in read only and read
        fd1 = open(myfifo,O_RDONLY);
        read(fd1, str1, 80);

        // Print the read string and close
        printf("User1: %s\n", str1);
        close(fd1);

        // Now open in write mode and write
        // string taken from user.
        fd1 = open(myfifo,O_WRONLY);
        fgets(str2, 80, stdin);
        write(fd1, str2, strlen(str2)+1);
        close(fd1);
    }
    return 0;
}
#include <unistd.h>
#include <stdio.h>
int main() {
    if (truncate("file.txt", 100) == 0)
        printf("Truncated.\n");
    else perror("truncate");
    return 0;
}

#include <stdio.h>
#include <string.h>

int main() {
    FILE *fp = fopen("day.csv", "r");
    char line[1024];
    char keyword[] = "1st";
    int line_num = 1;

    if (!fp) {
        perror("fopen");
        return 1;
    }

    while (fgets(line, sizeof(line), fp)) {
        if (strstr(line, keyword)) {
            printf("Found at line %d: %s", line_num, line);
        }
        line_num++;
    }

    fclose(fp);
    return 0;
}


