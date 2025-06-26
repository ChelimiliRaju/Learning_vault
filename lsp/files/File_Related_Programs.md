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
### 11. Develop a C program to check if a directory named "Test" exists in the current directory?
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
### 12. Implement a C program to create a new directory named "Backup" in the parent directory?
```c
#include<stdio.h>
#include<sys/stat.h>

int main() {
	if(mkdir("../Backup",0755) == 0)
		printf("Backup directory created in parent directory.\n");
	else
		perror("mkdir");
	return 0;
}
```
### 13. Write a C program to recursively list all files and directories in a given directory?

```c
#include <stdio.h>
#include <dirent.h>     // For opendir(), readdir()
#include <string.h>     // For strcmp()
#include <sys/stat.h>   // For stat()
#include <stdlib.h>     // For exit()

// Recursive function to list files and folders
void listFiles(const char *path) {
    DIR *dir;
    struct dirent *entry;
    struct stat info;
    char fullPath[1000];

    // Open the directory
    dir = opendir(path);
    if (dir == NULL) {
        printf("Cannot open directory: %s\n", path);
        return;
    }

    // Read all entries in the directory
    while ((entry = readdir(dir)) != NULL) {

        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Create full path (e.g., path/filename)
        sprintf(fullPath, "%s/%s", path, entry->d_name);
        printf("%s\n", fullPath);  // Print the file or folder name

        // Get information about the file/folder
        stat(fullPath, &info);

        // If it's a directory, call this function again
        if (S_ISDIR(info.st_mode)) {
            listFiles(fullPath);  // Recursive call
        }
    }

    closedir(dir);  // Close the directory when done
}

int main() {
    char path[100];

    // Ask user to enter a folder path
    printf("Enter directory path: ");
    scanf("%s", path);

    listFiles(path);  // Start listing

    return 0;
}
```
### 14. Develop a C program to delete all files in a directory named "Temp"?

```c
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
```
### 15. Implement a C program to count the number of lines in a file named "data.txt"
```c
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
```
### 16. Write a C program to append "Goodbye!" to the end of an existing file named"message.txt"?
```c
#include <stdio.h>

int main() {
    FILE *fp;

    // Open the file in append mode ("a")
    fp = fopen("message.txt", "a");

    // Check if file opened successfully
    if (fp == NULL) {
        printf("Error: Could not open file.\n");
        return 1;
    }

    // Append the text
    fprintf(fp, "Goodbye!\n");

    // Close the file
    fclose(fp);

    printf("Text appended successfully.\n");
    return 0;
}

```
### 17. Implement a C program to change the permissions of a file named "file.txt" to readonly?
```c
#include<stdio.h>
#include<sys/stat.h>

int main() {
	if (chmod("copy.txt", 0444) == 0)
		printf("changed to read-only\n");
	else perror("chmod");
	return 0;
}
```
### 18. Write a C program to change the ownership of a file named "file.txt" to the user "user1"?
```c
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
```
### 19. Develop a C program to get the last modified timestamp of a file named "file.txt"?
```c
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
```

### 20. Implement a C program to create a temporary file and write some data to it?
```c
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
```


### 21. Write a C program to check if a given path refers to a file or a directory?
```c

#include<stdio.h>
#include<sys/stat.h>
#include<string.h>
int main() {
        struct stat st;
        char path[100];
        printf("enter path or file name");
        fgets(path, sizeof(path),stdin);
        path[strcspn(path, "\n")] = 0;
        if ( stat(path,&st) == 0){
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
```
### 22. Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?
```c
#include <stdio.h>
#include <unistd.h>

int main() {
	if (link("source.txt", "hardlink.txt") == 0)
		printf("Hard link created\n");
	else
		perror("link");
	return 0;
}
```
### 23. Implement a C program to read and display the contents of a CSV file named "data.csv"?
```c
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
```
### 24. Write a C program to get the absolute path of the current working directory?
```c
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
```

### 25. Develop a C program to get the size of a directory named "Documents"?
```c
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
```
### 26. Recursively copy all files/directories from one directory to another
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <dirent.h>
#include <unistd.h>

void copyFile(const char *src, const char *dest) {
    FILE *f1 = fopen(src, "rb");
    FILE *f2 = fopen(dest, "wb");
    char ch;
    while ((ch = fgetc(f1)) != EOF) fputc(ch, f2);
    fclose(f1); fclose(f2);
}

void copyDir(const char *src, const char *dest) {
    mkdir(dest, 0777);
    DIR *dir = opendir(src);
    struct dirent *entry;
    struct stat st;

    while ((entry = readdir(dir)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) continue;
        char srcPath[1024], destPath[1024];
        sprintf(srcPath, "%s/%s", src, entry->d_name);
        sprintf(destPath, "%s/%s", dest, entry->d_name);
        stat(srcPath, &st);
        if (S_ISDIR(st.st_mode)) copyDir(srcPath, destPath);
        else copyFile(srcPath, destPath);
    }
    closedir(dir);
}

int main() {
    copyDir("source_folder", "destination_folder");
    return 0;
}
```

### 27. Get number of files in "Images" directory
```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>

int main() {
    DIR *d = opendir("Images");
    struct dirent *entry;
    struct stat st;
    int count = 0;

    while ((entry = readdir(d)) != NULL) {
        char path[100];
        sprintf(path, "Images/%s", entry->d_name);
        stat(path, &st);
        if (S_ISREG(st.st_mode)) count++;
    }
    closedir(d);
    printf("Number of files: %d\n", count);
    return 0;
}
```

### 28. Create a FIFO named "myfifo"
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    mkfifo("myfifo", 0666);
    return 0;
}
```

### 29. Read data from FIFO "myfifo"
```c
#include <stdio.h>

int main() {
    char buffer[100];
    FILE *fp = fopen("myfifo", "r");
    fgets(buffer, sizeof(buffer), fp);
    printf("Read: %s\n", buffer);
    fclose(fp);
    return 0;
}
```

### 30. Truncate "file.txt" to specific length
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    truncate("file.txt", 10);
    return 0;
}
```

### 31. Search string in "data.txt" and show line numbers
```c
#include <stdio.h>
#include <string.h>

int main() {
    FILE *fp = fopen("data.txt", "r");
    char line[256];
    int lineNo = 1;
    while (fgets(line, sizeof(line), fp)) {
        if (strstr(line, "target"))
            printf("Found at line: %d\n", lineNo);
        lineNo++;
    }
    fclose(fp);
    return 0;
}
```

### 32. Get file type of a given path
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    stat("path.txt", &st);

    if (S_ISREG(st.st_mode)) printf("Regular file\n");
    else if (S_ISDIR(st.st_mode)) printf("Directory\n");
    else if (S_ISLNK(st.st_mode)) printf("Symbolic link\n");
    else printf("Other type\n");
    return 0;
}
```

### 33. Create empty file "empty.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("empty.txt", "w");
    fclose(fp);
    return 0;
}
```

### 34. Get permissions of "file.txt"
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    stat("file.txt", &st);
    printf("Permissions: %o\n", st.st_mode & 0777);
    return 0;
}
```

### 35. Recursively delete directory "Temp"
```c
#include <stdio.h>
#include <string.h>
#include <dirent.h>
#include <unistd.h>
#include <sys/stat.h>

void deleteDir(const char *path) {
    DIR *dir = opendir(path);
    struct dirent *entry;
    char full[1024];
    struct stat st;

    while ((entry = readdir(dir)) != NULL) {
        if (!strcmp(entry->d_name, ".") || !strcmp(entry->d_name, "..")) continue;
        sprintf(full, "%s/%s", path, entry->d_name);
        stat(full, &st);
        if (S_ISDIR(st.st_mode)) deleteDir(full);
        else remove(full);
    }
    closedir(dir);
    rmdir(path);
}

int main() {
    deleteDir("Temp");
    return 0;
}
```

### 36. Read and display first 10 lines of "log.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("log.txt", "r");
    char line[256];
    int count = 0;

    while (fgets(line, sizeof(line), fp) && count < 10) {
        printf("%s", line);
        count++;
    }
    fclose(fp);
    return 0;
}
```

### 37. Copy file in reverse order
```c
#include <stdio.h>

int main() {
    FILE *in = fopen("input.txt", "r");
    FILE *out = fopen("output.txt", "w");
    fseek(in, 0, SEEK_END);
    long size = ftell(in);
    for (long i = size - 1; i >= 0; i--) {
        fseek(in, i, SEEK_SET);
        fputc(fgetc(in), out);
    }
    fclose(in);
    fclose(out);
    return 0;
}
```

### 38. Create new directory with current date format
```c
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>

int main() {
    char name[50];
    time_t t = time(NULL);
    strftime(name, sizeof(name), "%Y-%m-%d", localtime(&t));
    mkdir(name, 0777);
    return 0;
}
```

### 39. Read and display contents of binary file "binary.bin"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("binary.bin", "rb");
    char byte;
    while (fread(&byte, 1, 1, fp))
        printf("%02X ", (unsigned char)byte);
    fclose(fp);
    return 0;
}
```

### 40. Get size of largest file in a directory
```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>

int main() {
    DIR *dir = opendir(".");
    struct dirent *entry;
    struct stat st;
    long max = 0;

    while ((entry = readdir(dir)) != NULL) {
        stat(entry->d_name, &st);
        if (S_ISREG(st.st_mode) && st.st_size > max)
            max = st.st_size;
    }
    closedir(dir);
    printf("Largest file size: %ld bytes\n", max);
    return 0;
}
```

### 41. Check if "data.txt" is readable
```c
#include <unistd.h>
#include <stdio.h>

int main() {
    if (access("data.txt", R_OK) == 0)
        printf("data.txt is readable.\n");
    else
        printf("Not readable.\n");
    return 0;
}
```

### 42. Move all ".log" files to "Logs" folder
```c
#include <stdio.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>

int main() {
    mkdir("Logs", 0777);
    DIR *d = opendir(".");
    struct dirent *entry;
    char old[100], new[100];

    while ((entry = readdir(d)) != NULL) {
        if (strstr(entry->d_name, ".log")) {
            sprintf(old, "%s", entry->d_name);
            sprintf(new, "Logs/%s", entry->d_name);
            rename(old, new);
        }
    }
    closedir(d);
    return 0;
}
```

### 43. Check if "config.ini" is writable
```c
#include <unistd.h>
#include <stdio.h>

int main() {
    if (access("config.ini", W_OK) == 0)
        printf("Writable.\n");
    else
        printf("Not writable.\n");
    return 0;
}
```

### 44. Read and run shell commands from "instructions.txt"
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp = fopen("instructions.txt", "r");
    char line[100];
    while (fgets(line, sizeof(line), fp))
        system(line);
    fclose(fp);
    return 0;
}
```

### 45. Get number of hard links to "file.txt"
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    stat("file.txt", &st);
    printf("Hard links: %ld\n", (long)st.st_nlink);
    return 0;
}
```

### 46. Copy all .txt files in a directory into "combined.txt"
```c
#include <stdio.h>
#include <dirent.h>
#include <string.h>

int main() {
    DIR *d;
    struct dirent *entry;
    FILE *in, *out;
    char buffer[1024];

    d = opendir(".");
    out = fopen("combined.txt", "w");

    while ((entry = readdir(d)) != NULL) {
        if (strstr(entry->d_name, ".txt") && strcmp(entry->d_name, "combined.txt") != 0) {
            in = fopen(entry->d_name, "r");
            while (fgets(buffer, sizeof(buffer), in))
                fputs(buffer, out);
            fclose(in);
        }
    }

    fclose(out);
    closedir(d);
    return 0;
}
```

### 47. Recursively calculate total size of files in directory
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <dirent.h>

long getSize(const char *path) {
    struct stat st;
    long total = 0;
    DIR *dir = opendir(path);
    struct dirent *entry;

    if (!dir) return 0;

    while ((entry = readdir(dir)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        char fullpath[1024];
        sprintf(fullpath, "%s/%s", path, entry->d_name);
        stat(fullpath, &st);

        if (S_ISDIR(st.st_mode))
            total += getSize(fullpath);
        else
            total += st.st_size;
    }
    closedir(dir);
    return total;
}

int main() {
    printf("Total size: %ld bytes\n", getSize("."));
    return 0;
}
```

### 48. Get number of bytes in file "data.bin"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("data.bin", "rb");
    if (!fp) {
        perror("Error");
        return 1;
    }
    fseek(fp, 0, SEEK_END);
    long size = ftell(fp);
    fclose(fp);
    printf("Size: %ld bytes\n", size);
    return 0;
}
```

### 49. Create new directory with timestamp format
```c
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>

int main() {
    time_t t = time(NULL);
    struct tm *tm = localtime(&t);
    char dirname[100];
    strftime(dirname, sizeof(dirname), "%Y-%m-%d-%H-%M-%S", tm);
    mkdir(dirname, 0777);
    printf("Created directory: %s\n", dirname);
    return 0;
}
```

### 50. Create directory "Documents" in current directory
```c
#include <sys/stat.h>

int main() {
    mkdir("Documents", 0777);
    return 0;
}
```

### 51. Check if "file.txt" exists
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("file.txt", "r");
    if (fp) {
        printf("file.txt exists.\n");
        fclose(fp);
    } else {
        printf("file.txt does not exist.\n");
    }
    return 0;
}
```

### 52. Open and display contents of "data.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("data.txt", "r");
    char ch;
    if (!fp) {
        printf("Can't open file.\n");
        return 1;
    }
    while ((ch = fgetc(fp)) != EOF)
        putchar(ch);
    fclose(fp);
    return 0;
}
```

### 53. Write "Hello, World!" to "output.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("output.txt", "w");
    fputs("Hello, World!", fp);
    fclose(fp);
    return 0;
}
```

### 54. Delete "delete_me.txt"
```c
#include <stdio.h>

int main() {
    if (remove("delete_me.txt") == 0)
        printf("File deleted.\n");
    else
        perror("Error deleting file");
    return 0;
}
```

### 55. Rename file
```c
#include <stdio.h>

int main() {
    if (rename("old_name.txt", "new_name.txt") == 0)
        printf("Renamed successfully.\n");
    else
        perror("Rename failed");
    return 0;
}
```

### 56. Copy file contents
```c
#include <stdio.h>

int main() {
    FILE *src = fopen("source.txt", "r");
    FILE *dest = fopen("copy.txt", "w");
    char ch;
    if (!src || !dest) return 1;
    while ((ch = fgetc(src)) != EOF)
        fputc(ch, dest);
    fclose(src);
    fclose(dest);
    return 0;
}
```

### 57. Move file to "Backup" folder
```c
#include <stdio.h>

int main() {
    if (rename("file.txt", "Backup/file.txt") == 0)
        printf("Moved successfully.\n");
    else
        perror("Move failed");
    return 0;
}
```

### 58. List files and folders in current directory
```c
#include <stdio.h>
#include <dirent.h>

int main() {
    DIR *dir = opendir(".");
    struct dirent *entry;
    while ((entry = readdir(dir)) != NULL)
        printf("%s\n", entry->d_name);
    closedir(dir);
    return 0;
}
```

### 59. Get size of "data.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("data.txt", "rb");
    fseek(fp, 0, SEEK_END);
    long size = ftell(fp);
    fclose(fp);
    printf("Size: %ld bytes\n", size);
    return 0;
}
```

### 60. Create "Pictures" directory in parent directory
```c
#include <sys/stat.h>

int main() {
    mkdir("../Pictures", 0777);
    return 0;
}
### 61. Count number of lines in "log.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("log.txt", "r");
    char ch;
    int lines = 0;
    while ((ch = fgetc(fp)) != EOF)
        if (ch == '\n') lines++;
    fclose(fp);
    printf("Lines: %d\n", lines);
    return 0;
}
```

### 62. Append "Goodbye!" to end of "message.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("message.txt", "a");
    fprintf(fp, "Goodbye!\n");
    fclose(fp);
    return 0;
}
```

### 63. Create symbolic link "link.txt" to "target.txt"
```c
#include <unistd.h>

int main() {
    symlink("target.txt", "link.txt");
    return 0;
}
```

### 64. Change permissions of "file.txt" to read-only
```c
#include <sys/stat.h>

int main() {
    chmod("file.txt", 0444);
    return 0;
}
```

### 65. Change ownership of "file.txt" to user "user1"
```c
#include <unistd.h>
#include <pwd.h>

int main() {
    struct passwd *pw = getpwnam("user1");
    chown("file.txt", pw->pw_uid, -1);
    return 0;
}
```

### 66. Get last modified time of "file.txt"
```c
#include <stdio.h>
#include <sys/stat.h>
#include <time.h>

int main() {
    struct stat st;
    stat("file.txt", &st);
    printf("Last modified: %s", ctime(&st.st_mtime));
    return 0;
}
```

### 67. Create temporary file and write data
```c
#include <stdio.h>

int main() {
    FILE *fp = tmpfile();
    fprintf(fp, "Temporary data\n");
    fclose(fp);
    return 0;
}
```

### 68. Get size of "image.jpg"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("image.jpg", "rb");
    fseek(fp, 0, SEEK_END);
    printf("Size: %ld bytes\n", ftell(fp));
    fclose(fp);
    return 0;
}
```

### 69. Write multiple lines to "notes.txt"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("notes.txt", "w");
    fprintf(fp, "Line 1\nLine 2\nLine 3\n");
    fclose(fp);
    return 0;
}
```

### 70. Count words in "essay.txt"
```c
#include <stdio.h>
#include <ctype.h>

int main() {
    FILE *fp = fopen("essay.txt", "r");
    char ch;
    int words = 0, inWord = 0;
    while ((ch = fgetc(fp)) != EOF) {
        if (isspace(ch)) inWord = 0;
        else if (!inWord) { inWord = 1; words++; }
    }
    fclose(fp);
    printf("Words: %d\n", words);
    return 0;
}
```

### 71. Create symbolic link "shortcut.txt" to "target.txt"
```c
#include <unistd.h>

int main() {
    symlink("target.txt", "shortcut.txt");
    return 0;
}
```

### 72. Change "important.doc" permissions to read/write for owner only
```c
#include <sys/stat.h>

int main() {
    chmod("important.doc", 0600);
    return 0;
}
```

### 73. Get last access time of "data.txt"
```c
#include <stdio.h>
#include <sys/stat.h>
#include <time.h>

int main() {
    struct stat st;
    stat("data.txt", &st);
    printf("Last access: %s", ctime(&st.st_atime));
    return 0;
}
```

### 74. Read and display CSV file "data.csv"
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("data.csv", "r");
    char line[256];
    while (fgets(line, sizeof(line), fp))
        printf("%s", line);
    fclose(fp);
    return 0;
}
```

### 75. Truncate file "file.txt" to a length
```c
#include <unistd.h>

int main() {
    truncate("file.txt", 5);
    return 0;
}
```

### 76. Search word in "data.txt" and show line numbers
```c
#include <stdio.h>
#include <string.h>

int main() {
    FILE *fp = fopen("data.txt", "r");
    char line[256];
    int lineNo = 1;
    while (fgets(line, sizeof(line), fp)) {
        if (strstr(line, "word"))
            printf("Found at line %d\n", lineNo);
        lineNo++;
    }
    fclose(fp);
    return 0;
}
```

### 77. Get file type of given path
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    stat("path", &st);
    if (S_ISREG(st.st_mode)) printf("Regular\n");
    else if (S_ISDIR(st.st_mode)) printf("Directory\n");
    else if (S_ISLNK(st.st_mode)) printf("Symbolic link\n");
    else printf("Other\n");
    return 0;
}
```

### 78. Display binary file "binary.bin" in hex
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("binary.bin", "rb");
    unsigned char byte;
    while (fread(&byte, 1, 1, fp))
        printf("%02X ", byte);
    fclose(fp);
    return 0;
}
```

### 79. Move ".log" files to "Logs" directory
```c
#include <stdio.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>

int main() {
    mkdir("Logs", 0777);
    DIR *d = opendir(".");
    struct dirent *e;
    char old[100], new[100];
    while ((e = readdir(d)) != NULL) {
        if (strstr(e->d_name, ".log")) {
            sprintf(old, "%s", e->d_name);
            sprintf(new, "Logs/%s", e->d_name);
            rename(old, new);
        }
    }
    closedir(d);
    return 0;
}
```

### 80. Check if "config.ini" is writable
```c
#include <unistd.h>
#include <stdio.h>

int main() {
    if (access("config.ini", W_OK) == 0)
        printf("Writable\n");
    else
        printf("Not writable\n");
    return 0;
}
```

### 81. Run shell commands from "instructions.txt"
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp = fopen("instructions.txt", "r");
    char cmd[100];
    while (fgets(cmd, sizeof(cmd), fp))
        system(cmd);
    fclose(fp);
    return 0;
}
```

### 82. Read binary file and display in hex
```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("data.bin", "rb");
    unsigned char b;
    while (fread(&b, 1, 1, fp))
        printf("%02X ", b);
    fclose(fp);
    return 0;
}
```

### 83. Recursively copy all files/directories
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>

void copyFile(const char *src, const char *dest) {
    FILE *f1 = fopen(src, "rb");
    FILE *f2 = fopen(dest, "wb");
    char c;
    while ((c = fgetc(f1)) != EOF) fputc(c, f2);
    fclose(f1); fclose(f2);
}

void copyDir(const char *src, const char *dest) {
    mkdir(dest, 0777);
    DIR *dir = opendir(src);
    struct dirent *e;
    struct stat st;
    while ((e = readdir(dir))) {
        if (!strcmp(e->d_name, ".") || !strcmp(e->d_name, "..")) continue;
        char s[512], d[512];
        sprintf(s, "%s/%s", src, e->d_name);
        sprintf(d, "%s/%s", dest, e->d_name);
        stat(s, &st);
        if (S_ISDIR(st.st_mode)) copyDir(s, d);
        else copyFile(s, d);
    }
    closedir(dir);
}

int main() {
    copyDir("source", "destination");
    return 0;
}
```

### 84. Get file type of path
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    stat("path", &st);
    if (S_ISREG(st.st_mode)) printf("Regular\n");
    else if (S_ISDIR(st.st_mode)) printf("Directory\n");
    else if (S_ISLNK(st.st_mode)) printf("Link\n");
    else printf("Other\n");
    return 0;
}
```
# Linux File System and I/O – Q&A (85–123)

---

### **85. Types of files in Linux:**
- Regular file (`-`)
- Directory (`d`)
- Symbolic link (`l`)
- Character device (`c`)
- Block device (`b`)
- Socket (`s`)
- Named pipe / FIFO (`p`)

---

### **86. Accessing IPC named pipes:**
- Create: `mkfifo("myfifo", 0666);`
- Access with `open()`, `read()`, and `write()` system calls.

---

### **87. User-space to hardware I/O calls:**
- `read()`, `write()`, `ioctl()`.

---

### **88. Why are basic I/O calls called universal?**
- They work on all file types: regular files, devices, sockets, pipes, etc.

---

### **89. Inode object contains:**
- File type, permissions, owner UID/GID, timestamps, size, disk block addresses.

---

### **90. File info stored in:**
- The inode.

---

### **91. Kernel uses which object to represent a file?**
- The inode object.

---

### **92. Access inode info from user space?**
- Yes, using `stat()`, `fstat()`, or `lstat()`.

---

### **93. System calls to access file information:**
- `stat()`, `fstat()`, `lstat()`.

---

### **94. `ls` command uses:**
- `stat()` or `lstat()` internally to get file metadata.

---

### **95. What happens after kernel finds inode?**
- It creates a file object and maps it to the process's file descriptor table.

---

### **96. Contents of file object:**
- File offset, access flags, reference count, pointer to inode, operations pointer.

---

### **97. Object used to represent open files in kernel:**
- File object (struct file).

---

### **98. Primary data vs. others in file object:**
- Primary = file content,  
  Others = offset, access mode, flags, metadata.

---

### **99. Where is the file object pointer stored?**
- In the process’s File Descriptor Table.

---

### **100. When is FD table created and its size?**
- Created at process startup.  
  Default size is usually 1024 (configurable).

---

### **101. When is file object created?**
- When a file is opened using `open()` or `fopen()`.

---

### **102. What does `open()` return?**
- An integer file descriptor.

---

### **103. What does `open()` return on first call?**
- File descriptor `3`, since 0=stdin, 1=stdout, 2=stderr.

---

### **104. Why does `read()` need file object?**
- To get current offset, inode, and permissions.

---

### **105. System call to move file cursor:**
- `lseek(fd, offset, whence);`

---

### **106. Can multiple processes open same file?**
- Yes. They share the same inode but have separate file descriptors.

---

### **107. Object kernel uses for file:**
- Inode object.

---

### **108. Is there a file open limit?**
- Yes. Per-process limit defined by `ulimit -n`.

---

### **109. Standard I/O calls (aka Library I/O):**
- `fopen()`, `fread()`, `fwrite()`, `fclose()`

---

### **110. Basic I/O calls (aka System I/O):**
- `open()`, `read()`, `write()`, `close()`

---

### **111. Difference between basic and standard I/O:**

| Feature          | Basic I/O         | Standard I/O       |
|------------------|-------------------|---------------------|
| API              | `open()`, `read()`| `fopen()`, `fread()`|
| Buffering        | No                | Yes                 |
| Handle type      | int (fd)          | `FILE*`             |
| Speed            | Fast, low-level   | Slower, high-level  |

---

### **112. Other ways to access files:**
- Memory-mapped files using `mmap()`.

---

### **113. How does user access inode info?**
- With `stat()`, `fstat()`, `lstat()` system calls.

---

### **114. How to access kernel info from user space?**
- Using `/proc`, `/sys`, system calls, or kernel modules.

---

### **115. System calls to access file object:**
- `read()`, `write()`, `lseek()`, `dup()`, `close()`

---

### **116. System calls to access inode:**
- `stat()`, `open()`, `fstat()`

---

### **117. Print "Hello" using basic I/O:**
```c
#include <unistd.h>
int main() {
    write(1, "Hello\n", 6);
    return 0;
}




