# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
````
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h> 
#include<sys/stat.h> 
#include<string.h> 
#include<fcntl.h> 
#include<unistd.h>
#include<sys/wait.h>
void server(int,int); 
void client(int,int); 
int main() 
{ 
int p1[2],p2[2],pid, *waits; 
pipe(p1); 
pipe(p2); 
pid=fork(); 
if(pid==0) { 
close(p1[1]); 
close(p2[0]); 
server(p1[0],p2[1]); return 0;
 } 
close(p1[0]); 
close(p2[1]); 
client(p1[1],p2[0]); 
wait(waits); 
return 0; 
} 

void server(int rfd,int wfd) 
{ 
int i,j,n; 
char fname[2000]; 
char buff[2000];
n=read(rfd,fname,2000);
fname[n]='\0';
int fd=open(fname,O_RDONLY);
sleep(10); 
if(fd<0) 
write(wfd,"can't open",9); 
else 
n=read(fd,buff,2000); 
write(wfd,buff,n); 
}
void client(int wfd,int rfd) {
int i,j,n; char fname[2000];
char buff[2000];
printf("ENTER THE FILE NAME :");
scanf("%s",fname);
printf("CLIENT SENDING THE REQUEST .... PLEASE WAIT\n");
sleep(10);
write(wfd,fname,2000);
n=read(rfd,buff,2000);
buff[n]='\0';
printf("THE RESULTS OF CLIENTS ARE ...... \n"); write(1,buff,n);
}
`````


## OUTPUT
https://private-user-images.githubusercontent.com/118889454/316734981-e1f8e702-443f-4afa-a311-76cacd67169b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTIyMjc2MTgsIm5iZiI6MTcxMjIyNzMxOCwicGF0aCI6Ii8xMTg4ODk0NTQvMzE2NzM0OTgxLWUxZjhlNzAyLTQ0M2YtNGFmYS1hMzExLTc2Y2FjZDY3MTY5Yi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNDA0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDQwNFQxMDQxNThaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zODI2OTE4YjVhOGE3NDUzODU4NjVjYTJjZDJmNGE4ZTVlODY1YmJhNGIxNzVhZDg2ZDA1OTZlM2FjYmIyYzFlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.D0BCG2X4YaC_8VQ2Q8P_nI3S68o7z88ullwnGfrNlQs

## C Program that illustrate communication between two process using named pipes using Linux API system calls
`````
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
int main(){
int res = mkfifo("/tmp/my_fifo", 0777);
if (res == 0) printf("FIFO created\n");
exit(EXIT_SUCCESS);
}
`````





## OUTPUT
https://private-user-images.githubusercontent.com/138850116/316583885-8b754427-2304-4cf5-9298-59730f6fb15e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTIyMjc2MTgsIm5iZiI6MTcxMjIyNzMxOCwicGF0aCI6Ii8xMzg4NTAxMTYvMzE2NTgzODg1LThiNzU0NDI3LTIzMDQtNGNmNS05Mjk4LTU5NzMwZjZmYjE1ZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNDA0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDQwNFQxMDQxNThaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02ZGNmN2Q5YmY3YjJiZjI1ODY2MGNhYjU0ZjAwMzRjZWZmMzI5YmMyMzFlYTZmYzhlMWQ5NjFiMmE1N2QxYTNhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.VUe4_BlExuZMKAl2Z4ecZrAxz0KwOWLqfzju0JVpxdQ

# RESULT:
The program is executed successfully.
