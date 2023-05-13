# os
slip no.1

Q.1 ) Write a C Menu driven Program to implement following functionality
a) Accept Available
b) Display Allocation, Max
c) Display the contents of need matrix
d) Display Available

ANS:-

#include<stdio.h>
int nop,nor,A[10][10],M[10][10],Av[10],N[10][10],finish[10];
void acceptdata(int x[10][10])
{
 int i,j; 
 for(i=0;i<nop;i++)
 {
printf("P%d\n",i);
 for(j=0;j<nor;j++)
{
 printf("%c: ",65+j);
 scanf("%d",&x[i][j]);
}
 }
}
void acceptav()
{
 int i;
 for(i=0;i<nor;i++)
 {
 printf("%c: ",65+i);
 scanf("%d",&Av[i]);
 }
}
void calcneed()
{
 int i,j;
 for(i=0;i<nop;i++)
 for(j=0;j<nor;j++)
 N[i][j]=M[i][j]-A[i][j];
}
void displaydata()
{
int i,j;
printf("\n\tAllocation \t\tMax\t\tNeed\n\t");
for(i=0;i<3;i++)
{
for(j=0;j<nor;j++)
printf("%4c",65+j);
printf("\t");
}
for(i=0;i<nop;i++)
{
printf("\nP%d\t",i);
for(j=0;j<nor;j++)
printf("%4d",A[i][j]);
printf("\t");
for(j=0;j<nor;j++)
printf("%4d",M[i][j]);
printf("\t");
for(j=0;j<nor;j++)
printf("%4d",N[i][j]);
}
printf("\navailable");
for(i=0;i<nor;i++)
printf("%4d",Av[i]);
}
int checkneed(int pno)
{
int i;
for(i=0;i<nor;i++)
if(N[pno][i]>Av[i])
return 0;
return 1;
}
main()
{
 printf("\nEnter No of Processes: ");
 scanf("%d",&nop);
 printf("\nEnter No. of Resources: ");
 scanf("%d",&nor);
 printf("\nEnter Allocation Matrix: ");
 acceptdata(A);
 printf("\nEnter Max Matrix: ");
 acceptdata(M);
 printf("\nEnter Availability:");
 acceptav();
 calcneed();
 displaydata();
}


Q.2 Write a simulation program for disk scheduling using FCFS algorithm. Accept
total number of disk blocks, disk request string, and current head position from the
user. Display the list of request in the order in which it is served. Also display the
total number of head moments.
 55, 58, 39, 18, 90, 160, 150, 38, 184
 Start Head Position: 50

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
main()
{
int i,j,k;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
for(i=0;i<nor;i++)
{
printf("\n%d-%d",start,ReqString[i]);
 if(ReqString[i]>=start)
 {
thm+=ReqString[i]-start;
start=ReqString[i];
 }
 else
 {
 thm+=start-ReqString[i];
 start=ReqString[i];
 }
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.2

Q.1 Write a program to simulate Linked file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned below and implement each option.
• Show Bit Vector
• Create New File
• Show Directory
• Exit

ANS:-
#include <stdio.h>
#include <stdlib.h>
int main(){
 int f[50], p, i, st, len, j, c, k, a;
 for (i = 0; i < 50; i++)
 f[i] = 0;
 printf("Enter how many blocks already allocated: ");
 scanf("%d", &p);
 printf("Enter blocks already allocated: ");
 for (i = 0; i < p; i++) {
 scanf("%d", &a);
 f[a] = 1;
 }
x:
 printf("Enter starting block: ");
 scanf("%d",&st);
 printf("Enter length of the file: ");
 scanf("%d",&len);
 k = len;
 if (f[st] == 0)
 {
 for (j = st; j < (st + k); j++){
 if (f[j] == 0){
 f[j] = 1;
 printf("%d-------->%d\n", j, f[j]);
 }
 else{
 printf("%d Block is already allocated \n", j);
 k++;
 }
 }
 }
 else
 printf("%d starting block is already allocated \n", st);
 printf("Do you want to enter more file(Yes - 1/No - 0)");
 scanf("%d", &c);
 if (c == 1)
 goto x;
 else
 exit(0);
 return 0;
}

Q.2 Write an MPI program to calculate sum of randomly generated 1000 numbers
(stored in array) on a cluster 

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 == 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of even numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}

*******************************************************************************************
slip no.3
Q.1 Write a C program to simulate Banker’s algorithm for the purpose of deadlock
avoidance. Consider the following snapshot of system, A, B, C and D is the
resource type.
Process Allocation 	Max      Available
        A B C D 	  A B C D 	   A B C D
P0 	  0 0 1 2 	  0 0 1 2      1 5 2 0
P1 1 0 0 0 1 7 5 0
P2 1 3 5 4 2 3 5 6
P3 0 6 3 2 0 6 5 2
P4 0 0 1 4 0 6 5 6
a)Calculate and display the content of need matrix?
b)Is the system in safe state? If display the safe sequence

ANS:-
#include<stdio.h>
int main()
{
    int n,m,i,j,k;
    n=5; // Number of processes
    m=4; // Number of resources
    int alloc[5][4] = { { 0, 0, 1, 2 }, // P0 // Allocation Matrix
                        { 1, 0, 0, 0 }, // P1
                        { 1, 3, 5, 4 }, // P2
                        { 0, 6, 3, 2 }, // P3
                        { 0, 0, 1, 4 } }; // P4

    int max[5][4] = { { 0, 0, 1, 2 }, // P0 // MAX Matrix
                      { 1, 7, 5, 0 }, // P1
                      { 2, 3, 5, 6 }, // P2
                      { 0, 6, 5, 2 }, // P3
                      { 0, 6, 5, 6 } }; // P4

    int avail[4] = {1 ,5 ,2 ,0}; // Available Resources

    int f[n],ans[n],ind=0;
    for (k=0;k<n;k++) {
        f[k] = 0;
    }
    int need[n][m];
    for (i=0;i<n;i++) {
        for (j=0;j<m;j++)
            need[i][j] = max[i][j] - alloc[i][j];
    }
    int y=0;
    for (k=0;k<5;k++)
    {
        for (i=0;i<n;i++)
        {
            if (f[i]==0)
            {

                int flag=0;
                for (j=0;j<m;j++)
                {
                    if (need[i][j]>avail[j]){
                        flag=1;
                         break;
                    }
                }

                if (flag==0)
                {
                    ans[ind++]=i;
                    for (y=0;y<m;y++)
                        avail[y]+=alloc[i][y];
                    f[i]=1;
                }
            }
        }
    }

    printf("Need Matrix:\n");
    for (i = 0 ; i < n ; i++)
    {
        for (j = 0 ; j < m ; j++)
        {
            printf("%d ",need[i][j]);
        }
        printf("\n");
    }

    printf("System is in safe state.\nSafe sequence is: ");
    for (i=0;i<n-1;i++)
        printf("P%d -> ",ans[i]);
    printf("P%d",ans[n-1]);

    return(0);
}


Q.2 Write an MPI program to calculate sum and average of randomly generated 1000
numbers (stored in array) on a cluster

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 != 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of odd numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}


*******************************************************************************************
slip no.4
Q.1 Implement the Menu driven Banker's algorithm for accepting Allocation, Max
fromuser.
a)Accept Available
b)Display Allocation, Max
c)Find Need and display It,
d)Display Available
Consider the system with 3 resources types A,B, and C with 7,2,6 instances respectively.
Consider the following snapshot:
Process Allocation Request
A B C A B C
P0 0 1 0 0 0 0
P1 4 0 0 5 2 2
P2 5 0 4 1 0 4
P3 4 3 3 4 4 4
P4 2 2 4 6 5 5

ANS:-
#include<stdio.h>
#include<stdlib.h>

int n,m,i,j,k;
int alloc[10][10], max[10][10], avail[10], need[10][10];

void input()
{
    printf("Enter the number of processes: ");
    scanf("%d",&n);
    printf("Enter the number of resource types: ");
    scanf("%d",&m);

    printf("Enter the Allocation Matrix:\n");
    for(i=0;i<n;i++)
    {
        for(j=0;j<m;j++)
        {
            scanf("%d",&alloc[i][j]);
        }
    }

    printf("Enter the Max Matrix:\n");
    for(i=0;i<n;i++)
    {
        for(j=0;j<m;j++)
        {
            scanf("%d",&max[i][j]);
        }
    }
}

void findNeed()
{
    for(i=0;i<n;i++)
    {
        for(j=0;j<m;j++)
        {
            need[i][j] = max[i][j] - alloc[i][j];
        }
    }
}

void display(int x[][10])
{
    for(i=0;i<n;i++)
    {
        for(j=0;j<m;j++)
        {
            printf("%d ",x[i][j]);
        }
        printf("\n");
    }
}

int main()
{
    int ch;
    while(1)
    {
        printf("\nMenu:\n1. Input\n2. Display Allocation Matrix\n3. Display Max Matrix\n4. Find Need Matrix\n5. Display Need Matrix\n6. Exit\nEnter your choice: ");
        scanf("%d",&ch);
        switch(ch)
        {
            case 1: input();
                    break;
            case 2: display(alloc);
                    break;
            case 3: display(max);
                    break;
            case 4: findNeed();
                    break;
            case 5: findNeed();
                    display(need);
                    break;
            case 6: exit(0);
                    break;
            default: printf("Invalid choice!\n");
        }
    }
}


Q 2.)Write a simulation program for disk scheduling using SCAN algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total
number of head moments.
86, 147, 91, 170, 95, 130, 102, 70
Starting Head position= 125
Direction: Left

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
start=0;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,nob-1);
thm+=(nob-1)-start;
start=nob-1;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}



*******************************************************************************************
slip no.5

Q.1 Consider a system with ‘m’ processes and ‘n’ resource types. Accept number of
instances for every resource type. For each process accept the allocation and maximum
requirement matrices. Write a program to display the contents of need matrix and to
check if the given request of a process can be granted immediately or not

ANS:-
#include<stdio.h>
int nop,nor,Rprocess,A[10][10],M[10][10],Av[10],N[10][10],R[10],finish[10];
void acceptdata(int x[10][10])
{
int i,j; 
for(i=0;i<nop;i++)
{
printf("P%d\n",i);
for(j=0;j<nor;j++)
{
printf("%c: ",65+j);
scanf("%d",&x[i][j]);
}
}
}
void acceptav()
{
int i;
for(i=0;i<nor;i++)
{
printf("%c: ",65+i);
scanf("%d",&Av[i]);
}
}
void acceptrequest()
{
int i;
printf("\nEnter the Process for which request has arrived :P");
scanf("%d",&Rprocess); 
printf("\nEnter the request for process: ");
for(i=0;i<nor;i++)
{
printf("%c: ",65+i);
scanf("%d",&R[i]);
}
}
void calcneed()
{
int i,j;
for(i=0;i<nop;i++)
for(j=0;j<nor;j++)
N[i][j]=M[i][j]-A[i][j];
}
void displaydata()
{
int i,j;
printf("\n\tAllocation \t\tMax\t\tNeed\n\t");
for(i=0;i<3;i++)
{
for(j=0;j<nor;j++)
printf("%4c",65+j);
printf("\t");
}
for(i=0;i<nop;i++)
{
printf("\nP%d\t",i);
for(j=0;j<nor;j++)
printf("%4d",A[i][j]);
printf("\t");
for(j=0;j<nor;j++)
printf("%4d",M[i][j]);
printf("\t");
for(j=0;j<nor;j++)
printf("%4d",N[i][j]);
}
printf("\navailable");
for(i=0;i<nor;i++)
printf("%4d",Av[i]);
}
int checkneed(int pno)
{
int i;
for(i=0;i<nor;i++)
if(N[pno][i]>Av[i])
return 0;
return 1;
}
void resourcerequest()
{
int i;
for(i=0;i<nor;i++)
{
if(R[i]>N[Rprocess][i])
break; 
}
if(i==nor)
{
for(i=0;i<nor;i++)
{ 
if(R[i]>Av[i])
break;
}
}
if(i==nor)
{ 
printf("\nRequest<=Need \n Request<=Available \n Both 
Condition are true");
printf("\nThen system Pretends to fulfill request , then 
modify resourse allocation state");
for(i=0;i<nor;i++)
{
Av[i]=Av[i]-R[i];
A[Rprocess][i]=A[Rprocess][i]+R[i];
N[Rprocess][i]=N[Rprocess][i]-R[i];
}
displaydata();
}
else
{
printf("\nRequest<=Need \n Request<=Available \n Condition is 
not true");
printf("\nSo request cannot be satisfied!");
}
}
main()
{
printf("\nEnter No of Processes: ");
scanf("%d",&nop);
printf("\nEnter No. of Resources: ");
scanf("%d",&nor);
printf("\nEnter Allocation Matrix: ");
acceptdata(A);
printf("\nEnter Max Matrix: ");
acceptdata(M);
printf("\nEnter Availability:");
acceptav();
calcneed();
displaydata();
acceptrequest();
resourcerequest(); 
banker();
}

Q.2 Write an MPI program to find the max number from randomly generated 1000 numbers
(stored in array) on a cluster (Hint: Use MPI_Reduce)

ANS:-

#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_max = 0;
    int global_max = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] > local_max) {
            local_max = numbers[i];
        }
    }

    MPI_Reduce(&local_max, &global_max, 1, MPI_INT, MPI_MAX, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Max: %d\n", global_max);
    }

    MPI_Finalize();
    return 0;
}



*******************************************************************************************
slip no.6

Q.1 Write a program to simulate Linked file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned below and implement each option.
• Show Bit Vector
• Create New File
• Show Directory
• Exit

ANS:-
#include <stdio.h>
#include <stdlib.h>
void contiguous(int files[])
{
 int flag = 0, startBlock, len, j, k, ch;
 char fname[20]; 
 printf("\nEnter Name of File: ");
 scanf("%s",&fname);
 printf("Enter the starting block of the files: ");
 scanf("%d", &startBlock);
 printf("Enter the length of the files: ");
 scanf("%d", &len);
 for (j = startBlock; j < (startBlock + len); j++)
 {
 if (files[j] == 0)
 flag++;
 }
 if (len == flag)
 {
 for (k = startBlock; k < (startBlock + len); k++)
 {
 if (files[k] == 0)
 {
 files[k] = 1;
 printf("%d\t%d\n", k, files[k]);
 }
 }
 if (k != (startBlock + len - 1))
 printf("The file is allocated to the disk\n");
 }
 else
 printf("The file is not allocated to the disk\n");
 printf("Do you want to enter more files?\n");
 printf("Press 1 for YES, 0 for NO: ");
 scanf("%d", &ch);
 if (ch == 1)
 contiguous(files);
 else
 exit(0);
 return;
}
main()
{
 int i,files[50];
 for (i = 0; i < 50; i++)
 files[i] = 0;
 printf("Files Allocated are :\n");
 contiguous(files);
 return 0;
}


Q.2 Write a simulation program for disk scheduling using C-SCAN algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments..
80, 150, 60,135, 40, 35, 170
Starting Head Position: 70
Direction: Right

ANS:-

#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);printf("\nEnter Direction: ");
scanf(" %c",&direction);
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
printf("\n%d-%d",nob-1,ReqString[spos+1]);
thm+=(nob-1)-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",nob-1,start);
thm+=(nob-1)-start;
printf("\n%d-0",ReqString[spos-1]);
thm+=ReqString[spos-1]-0;
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.7

Q.1 Consider the following snapshot of the system.
Proces
s
Allocation Max Available
A B C D A B C D A B C D
P0 2 0 0 1 4 2 1 2 3 3 2 1
P1 3 1 2 1 5 2 5 2
P2 2 1 0 3 2 3 1 6
P3 1 3 1 2 1 4 2 4
P4 1 4 3 2 3 6 6 5
Using Resource –Request algorithm to Check whether the current system is in safe state
or not

ANS:-

??????

Q.2 Write a simulation program for disk scheduling using SCAN algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
82, 170, 43, 140, 24, 16, 190
 Starting Head Position: 50
 Direction: Right

ANS:-

#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
start=0;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,nob-1);
thm+=(nob-1)-start;
start=nob-1;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.8

Q.1 Write a program to simulate Contiguous file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned above and implement each option.
• Show Bit Vector
• Create New File
• Show Directory
• Exit

ANS:-
#include <stdio.h>
#include <stdlib.h>
void contiguous(int files[])
{
 int flag = 0, startBlock, len, j, k, ch;
 char fname[20]; 
 printf("\nEnter Name of File: ");
 scanf("%s",&fname);
 printf("Enter the starting block of the files: ");
 scanf("%d", &startBlock);
 printf("Enter the length of the files: ");
 scanf("%d", &len);
 for (j = startBlock; j < (startBlock + len); j++)
 {
 if (files[j] == 0)
 flag++;
 }
 if (len == flag)
 {
 for (k = startBlock; k < (startBlock + len); k++)
 {
 if (files[k] == 0)
 {
 files[k] = 1;
 printf("%d\t%d\n", k, files[k]);
 }
 }
 if (k != (startBlock + len - 1))
 printf("The file is allocated to the disk\n");
 }
 else
 printf("The file is not allocated to the disk\n");
 printf("Do you want to enter more files?\n");
 printf("Press 1 for YES, 0 for NO: ");
 scanf("%d", &ch);
 if (ch == 1)
 contiguous(files);
 else
 exit(0);
 return;
}
main()
{
 int i,files[50];
 for (i = 0; i < 50; i++)
 files[i] = 0;
 printf("Files Allocated are :\n");
 contiguous(files);
 return 0;
}


Q.2) Write a simulation program for disk scheduling using SSTF algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
 186, 89, 44, 70, 102, 22, 51, 124
 Start Head Position: 70

ANS:-

#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
void sort(int RS[])
{
int i,j,temp;
for(i=1;i<nor;i++)
{
for(j=0;j<nor-i;j++)
{
if(RS[j]>RS[j+1])
{
temp=RS[j];
RS[j]=RS[j+1];
RS[j+1]=temp;
}
}
}
}
void display(int RS[])
{
int i;
for(i=0;i<nor;i++)
printf(" %d",RS[i]);
}
int searchstartblock(int RS[])
{
int i;
for(i=0;i<nor;i++)
{
if(RS[i]==start)
return i;
}
return 1;
}
main()
{
int
i,j,k,ans,finish=0,leftpos,startpos,rightpos,leftdis,rightdis,initpos,flag=0;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
ans=searchstartblock(ReqString);
if(ans==1)
ReqString[nor++]=start;
sort(ReqString);
printf("\nRefString After Sorting: ");
display(ReqString);
startpos=searchstartblock(ReqString);
initpos=startpos;
leftpos=initpos-1;
rightpos=initpos+1;
while(1)
{
leftdis=ReqString[startpos]-ReqString[leftpos];
rightdis=ReqString[rightpos]-ReqString[startpos];
if(leftdis<rightdis)
{
printf("\n%d-%d",ReqString[startpos],ReqString[leftpos]);thm+=ReqString[startpos]-ReqString[leftpos];
startpos=leftpos;
leftpos--;
if(leftpos==-1)
{
break;
}

}
else if(leftdis>rightdis)
{
printf("\n%d-%d",ReqString[rightpos],ReqString[startpos]);
thm+=ReqString[rightpos]-ReqString[startpos];
startpos=rightpos;
rightpos++;
if(rightpos==nor)
{
break;
}
}
}
 printf("\nEnd of while loop");
while(leftpos>=0)
{
printf("\n%d-%d",ReqString[startpos],ReqString[leftpos]);thm+=ReqString[startpos]-ReqString[leftpos];
startpos=leftpos;
leftpos--;
}
while(rightpos<nor)
{
printf("\n%d-%d",ReqString[rightpos],ReqString[startpos]);thm+=ReqString[rightpos]-ReqString[startpos];
startpos=rightpos;
rightpos++;
}
printf("\nTotal Head Movement: %d",thm);
}



*******************************************************************************************
slip no.9

Q.1.Consider the following snapshot of system, A, B, C, D is the resource type.
Proces
s
Allocation Max Available
A B C D A B C D A B C D
P0 0 0 1 2 0 0 1 2 1 5 2 0
P1 1 0 0 0 1 7 5 0
P2 1 3 5 4 2 3 5 6
P3 0 6 3 2 0 6 5 2
P4 0 0 1 4 0 6 5 6
Using Resource –Request algorithm to Check whether the current system is in safe state
or not

ANS:-

??????

Q.2 Write a simulation program for disk scheduling using LOOK algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments. 
 176, 79, 34, 60, 92, 11, 41, 114
 Starting Head Position: 65
Direction: Left

ANS:-

#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
 main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,min);
thm+=start-min;
start=min;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,max);
thm+=max-start;
start=max;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}




*******************************************************************************************
slip no.10

Q.1 Write an MPI program to calculate sum and average of randomly generated 1000
numbers (stored in array) on a cluster

ANS:-

#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    double global_avg = 0.0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        local_sum += numbers[i];
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        global_avg = (double)global_sum / N;
        printf("Sum: %d\n", global_sum);
        printf("Average: %f\n", global_avg);
    }

    MPI_Finalize();
    return 0;
}

Q.2 Write a simulation program for disk scheduling using C-SCAN algorithm. Accept
total number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total
number of head moments.
 33, 99, 142, 52, 197, 79, 46, 65
Start Head Position: 72
Direction: Left

ANS:-

#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);printf("\nEnter Direction: ");
scanf(" %c",&direction);
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
printf("\n%d-%d",nob-1,ReqString[spos+1]);
thm+=(nob-1)-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",nob-1,start);
thm+=(nob-1)-start;
printf("\n%d-0",ReqString[spos-1]);
thm+=ReqString[spos-1]-0;
}
printf("\nTotal Head Movement: %d",thm);
}



*******************************************************************************************
slip no.11

Q.1 Write a C program to simulate Banker’s algorithm for the purpose of
deadlock avoidance. the following snapshot of system, A, B, C and D are the
resource type.
Proces
s
Allocation Max Available
A B C A B C A B C
P0 0 1 0 0 0 0 0 0 0
P1 2 0 0 2 0 2
P2 3 0 3 0 0 0
P3 2 1 1 1 0 0
P4 0 0 2 0 0 2
Implement the following Menu.
a) Accept Available
b) Display Allocation, Max
c) Display the contents of need matrix
d) Display Available 

ANS:-
#include<stdio.h>
int nop,nor,A[10][10],M[10][10],Av[10],N[10][10],finish[10];
void acceptdata(int x[10][10])
{
 int i,j; 
 for(i=0;i<nop;i++)
 {
printf("P%d\n",i);
 for(j=0;j<nor;j++)
{
 printf("%c: ",65+j);
 scanf("%d",&x[i][j]);
}
 }
}
void acceptav()
{
 int i;
 for(i=0;i<nor;i++)
 {
 printf("%c: ",65+i);
 scanf("%d",&Av[i]);
 }
}
void calcneed()
{
 int i,j;
 for(i=0;i<nop;i++)
 for(j=0;j<nor;j++)
 N[i][j]=M[i][j]-A[i][j];
}
void displaydata()
{
int i,j;
printf("\n\tAllocation \t\tMax\t\tNeed\n\t");
for(i=0;i<3;i++)
{
for(j=0;j<nor;j++)
printf("%4c",65+j);
printf("\t");
}
for(i=0;i<nop;i++)
{
printf("\nP%d\t",i);
for(j=0;j<nor;j++)
printf("%4d",A[i][j]);
printf("\t");
for(j=0;j<nor;j++)
printf("%4d",M[i][j]);
printf("\t");
for(j=0;j<nor;j++)
printf("%4d",N[i][j]);
}
printf("\navailable");
for(i=0;i<nor;i++)
printf("%4d",Av[i]);
}
int checkneed(int pno)
{
int i;
for(i=0;i<nor;i++)
if(N[pno][i]>Av[i])
return 0;
return 1;
}
void banker()
{
int p=0,j=0,k=0,flag=0,safe[10];
while(flag<2)
{
if(!finish[p])
{
printf("\n\nNeed of process P%d (,",p);
for(j=0;j<nor;j++)
printf("%d,",N[p][j]);
if(checkneed(p))
{
printf(") <= available (");
for(j=0;j<nor;j++)
printf("%d,",Av[j]);
printf(")");
printf("\nNeed is Satsified, So process P%d can be 
granted requiered resources.\n After P%d finishes, it will realease all 
the resources.",p,p);
for(j=0;j<nor;j++)
Av[j]=Av[j]+A[p][j];
printf("New Availble=");
for(j=0;j<nor;j++)
printf("%d ",Av[j]); 
finish[p]=1;
safe[k++]=p;
}
else
{
printf(") > available (");
for(j=0;j<nor;j++)
printf("%d,",Av[j]);
printf(")");
printf("\nNeed is not Satsified, So process P%d 
cannot be granted required resources.\n process P%d has to wait.",p,p);
}
}
if((p+1)%nop==0)
flag++;
p=(p+1)%nop;
}//while
 if(k==nop)
{
 printf("\nSystem is in safe state...");
 printf("\nSafe Sequence: ");
 for(j=0;j<k;j++)
 printf("P%d->",safe[j]);
 }
 else
printf("\nSystem is not in safe state....");
}
main()
{
 printf("\nEnter No of Processes: ");
 scanf("%d",&nop);
 printf("\nEnter No. of Resources: ");
 scanf("%d",&nor);
 printf("\nEnter Allocation Matrix: ");
 acceptdata(A);
 printf("\nEnter Max Matrix: ");
 acceptdata(M);
 printf("\nEnter Availability:");
 acceptav();
 calcneed();
 displaydata();
 banker();
}

Q.2 Write an MPI program to find the min number from randomly generated 1000 numbers
(stored in array) on a cluster (Hint: Use MPI_Reduce) 

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_min = 100;
    int global_min = 100;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] < local_min) {
            local_min = numbers[i];
        }
    }

    MPI_Reduce(&local_min, &global_min, 1, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Min: %d\n", global_min);
    }

    MPI_Finalize();
    return 0;
}


*******************************************************************************************
slip no.12

Q.1 Write an MPI program to calculate sum and average randomly generated 1000
numbers (stored in array) on a cluster. 

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 == 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of even numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}


Q.2 Write a simulation program for disk scheduling using C-LOOK algorithm. Accept
total number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
23, 89, 132, 42, 187, 69, 36, 55
Start Head Position: 40
Direction: Right

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,max,min,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,ReqString[0]);
thm+=start-ReqString[0];
printf("\n%d-%d",max,ReqString[spos+1]);
thm+=max-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",max,start);
thm+=max-start;
printf("\n%d-%d",ReqString[spos-1],min);
thm+=ReqString[spos-1]-min;
}
printf("\nTotal Head Movement: %d",thm);
}



*******************************************************************************************
slip no.13

Q.1 Write a C program to simulate Banker’s algorithm for the purpose of deadlock
avoidance.The following snapshot of system, A, B, C and D are the resource type.
Proces
s
Allocation Max Available
A B C A B C A B C
P0 0 1 0 0 0 0 0 0 0
P1 2 0 0 2 0 2
P2 3 0 3 0 0 0
P3 2 1 1 1 0 0
P4 0 0 2 0 0 2
a) Calculate and display the content of need matrix?
b) Is the system in safe state? If display the safe sequence. 

ANS:-
#include<stdio.h>
int main()
{
    int n,m,i,j,k;
    n=5; // Number of processes
    m=4; // Number of resources
// CHANGE THE MATRIX ACORDINGLY
    int alloc[5][4] = { { 0, 0, 1, 2 }, // P0 // Allocation Matrix
                        { 1, 0, 0, 0 }, // P1
                        { 1, 3, 5, 4 }, // P2
                        { 0, 6, 3, 2 }, // P3
                        { 0, 0, 1, 4 } }; // P4

    int max[5][4] = { { 0, 0, 1, 2 }, // P0 // MAX Matrix
                      { 1, 7, 5, 0 }, // P1
                      { 2, 3, 5, 6 }, // P2
                      { 0, 6, 5, 2 }, // P3
                      { 0, 6, 5, 6 } }; // P4

    int avail[4] = {1 ,5 ,2 ,0}; // Available Resources

    int f[n],ans[n],ind=0;
    for (k=0;k<n;k++) {
        f[k] = 0;
    }
    int need[n][m];
    for (i=0;i<n;i++) {
        for (j=0;j<m;j++)
            need[i][j] = max[i][j] - alloc[i][j];
    }
    int y=0;
    for (k=0;k<5;k++)
    {
        for (i=0;i<n;i++)
        {
            if (f[i]==0)
            {

                int flag=0;
                for (j=0;j<m;j++)
                {
                    if (need[i][j]>avail[j]){
                        flag=1;
                         break;
                    }
                }

                if (flag==0)
                {
                    ans[ind++]=i;
                    for (y=0;y<m;y++)
                        avail[y]+=alloc[i][y];
                    f[i]=1;
                }
            }
        }
    }

    printf("Need Matrix:\n");
    for (i = 0 ; i < n ; i++)
    {
        for (j = 0 ; j < m ; j++)
        {
            printf("%d ",need[i][j]);
        }
        printf("\n");
    }

    printf("System is in safe state.\nSafe sequence is: ");
    for (i=0;i<n-1;i++)
        printf("P%d -> ",ans[i]);
    printf("P%d",ans[n-1]);

    return(0);
}

Q.2 Write a simulation program for disk scheduling using SCAN algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
176, 79, 34, 60, 92, 11, 41, 114
Starting Head Position: 65
Direction: Left

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
start=0;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,nob-1);
thm+=(nob-1)-start;
start=nob-1;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}



*******************************************************************************************slip no.14

Q.1 Write a program to simulate Sequential (Contiguous) file allocation method.
Assume disk with n number of blocks. Give value of n as input. Randomly mark some
block as allocated and accordingly maintain the list of free blocks Write menu driver
program with menu options as mentioned below and implement each option.
• Show Bit Vector
• Show Directory
• Delete File
• Exit

ANS:-
 #include <stdio.h>
#include <stdlib.h>
void contiguous(int files[])
{
 int flag = 0, startBlock, len, j, k, ch;
 char fname[20]; 
 printf("\nEnter Name of File: ");
 scanf("%s",&fname);
 printf("Enter the starting block of the files: ");
 scanf("%d", &startBlock);
 printf("Enter the length of the files: ");
 scanf("%d", &len);
 for (j = startBlock; j < (startBlock + len); j++)
 {
 if (files[j] == 0)
 flag++;
 }
 if (len == flag)
 {
 for (k = startBlock; k < (startBlock + len); k++)
 {
 if (files[k] == 0)
 {
 files[k] = 1;
 printf("%d\t%d\n", k, files[k]);
 }
 }
 if (k != (startBlock + len - 1))
 printf("The file is allocated to the disk\n");
 }
 else
 printf("The file is not allocated to the disk\n");
 printf("Do you want to enter more files?\n");
 printf("Press 1 for YES, 0 for NO: ");
 scanf("%d", &ch);
 if (ch == 1)
 contiguous(files);
 else
 exit(0);
 return;
}
main()
{
 int i,files[50];
 for (i = 0; i < 50; i++)
 files[i] = 0;
 printf("Files Allocated are :\n");
 contiguous(files);
 return 0;
}

Q.2 Write a simulation program for disk scheduling using SSTF algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
55, 58, 39, 18, 90, 160, 150, 38, 184
Start Head Position: 50

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
void sort(int RS[])
{
int i,j,temp;
for(i=1;i<nor;i++)
{
for(j=0;j<nor-i;j++)
{
if(RS[j]>RS[j+1])
{
temp=RS[j];
RS[j]=RS[j+1];
RS[j+1]=temp;
}
}
}
}
void display(int RS[])
{
int i;
for(i=0;i<nor;i++)
printf(" %d",RS[i]);
}
int searchstartblock(int RS[])
{
int i;
for(i=0;i<nor;i++)
{
if(RS[i]==start)
return i;
}
return 1;
}
main()
{
int
i,j,k,ans,finish=0,leftpos,startpos,rightpos,leftdis,rightdis,initpos,flag=0;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
ans=searchstartblock(ReqString);
if(ans==1)
ReqString[nor++]=start;
sort(ReqString);
printf("\nRefString After Sorting: ");
display(ReqString);
startpos=searchstartblock(ReqString);
initpos=startpos;
leftpos=initpos-1;
rightpos=initpos+1;
while(1)
{
leftdis=ReqString[startpos]-ReqString[leftpos];
rightdis=ReqString[rightpos]-ReqString[startpos];
if(leftdis<rightdis)
{
printf("\n%d-%d",ReqString[startpos],ReqString[leftpos]);thm+=ReqString[startpos]-ReqString[leftpos];
startpos=leftpos;
leftpos--;
if(leftpos==-1)
{
break;
}

}
else if(leftdis>rightdis)
{
printf("\n%d-%d",ReqString[rightpos],ReqString[startpos]);
thm+=ReqString[rightpos]-ReqString[startpos];
startpos=rightpos;
rightpos++;
if(rightpos==nor)
{
break;
}
}
}
 printf("\nEnd of while loop");
while(leftpos>=0)
{
printf("\n%d-%d",ReqString[startpos],ReqString[leftpos]);thm+=ReqString[startpos]-ReqString[leftpos];
startpos=leftpos;
leftpos--;
}
while(rightpos<nor)
{
printf("\n%d-%d",ReqString[rightpos],ReqString[startpos]);thm+=ReqString[rightpos]-ReqString[startpos];
startpos=rightpos;
rightpos++;
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.15

Q.1 Write a program to simulate Linked file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned below and implement each option.
• Show Bit Vector
• Create New File
• Show Directory
• Exit

ANS:-
#include <stdio.h>
#include <stdlib.h>
int main(){
 int f[50], p, i, st, len, j, c, k, a;
 for (i = 0; i < 50; i++)
 f[i] = 0;
 printf("Enter how many blocks already allocated: ");
 scanf("%d", &p);
 printf("Enter blocks already allocated: ");
 for (i = 0; i < p; i++) {
 scanf("%d", &a);
 f[a] = 1;
 }
x:
 printf("Enter starting block: ");
 scanf("%d",&st);
 printf("Enter length of the file: ");
 scanf("%d",&len);
 k = len;
 if (f[st] == 0)
 {
 for (j = st; j < (st + k); j++){
 if (f[j] == 0){
 f[j] = 1;
 printf("%d-------->%d\n", j, f[j]);
 }
 else{
 printf("%d Block is already allocated \n", j);
 k++;
 }
 }
 }
 else
 printf("%d starting block is already allocated \n", st);
 printf("Do you want to enter more file(Yes - 1/No - 0)");
 scanf("%d", &c);
 if (c == 1)
 goto x;
 else
 exit(0);
 return 0;
}

Q.2 Write a simulation program for disk scheduling using C-SCAN algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.. 
80, 150, 60,135, 40, 35, 170
Starting Head Position: 70
Direction: Right

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);printf("\nEnter Direction: ");
scanf(" %c",&direction);
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
printf("\n%d-%d",nob-1,ReqString[spos+1]);
thm+=(nob-1)-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",nob-1,start);
thm+=(nob-1)-start;
printf("\n%d-0",ReqString[spos-1]);
thm+=ReqString[spos-1]-0;
}
printf("\nTotal Head Movement: %d",thm);
}



*******************************************************************************************
slip no.16

Q.1 Write a program to simulate Sequential (Contiguous) file allocation method.
Assume disk with n number of blocks. Give value of n as input. Randomly mark some
block as allocated and accordingly maintain the list of free blocks Write menu driver
program with menu options as mentioned below and implement each option
• Show Bit Vector
• Create New File
• Show Directory
• Exit 

ANS:-

#include <stdio.h>
#include <stdlib.h>
void contiguous(int files[])
{
 int flag = 0, startBlock, len, j, k, ch;
 char fname[20]; 
 printf("\nEnter Name of File: ");
 scanf("%s",&fname);
 printf("Enter the starting block of the files: ");
 scanf("%d", &startBlock);
 printf("Enter the length of the files: ");
 scanf("%d", &len);
 for (j = startBlock; j < (startBlock + len); j++)
 {
 if (files[j] == 0)
 flag++;
 }
 if (len == flag)
 {
 for (k = startBlock; k < (startBlock + len); k++)
 {
 if (files[k] == 0)
 {
 files[k] = 1;
 printf("%d\t%d\n", k, files[k]);
 }
 }
 if (k != (startBlock + len - 1))
 printf("The file is allocated to the disk\n");
 }
 else
 printf("The file is not allocated to the disk\n");
 printf("Do you want to enter more files?\n");
 printf("Press 1 for YES, 0 for NO: ");
 scanf("%d", &ch);
 if (ch == 1)
 contiguous(files);
 else
 exit(0);
 return;
}
main()
{
 int i,files[50];
 for (i = 0; i < 50; i++)
 files[i] = 0;
 printf("Files Allocated are :\n");
 contiguous(files);
 return 0;
}

Q.2 Write an MPI program to find the min number from randomly generated 1000 numbers
(stored in array) on a cluster (Hint: Use MPI_Reduce)

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_min = 100;
    int global_min = 100;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] < local_min) {
            local_min = numbers[i];
        }
    }

    MPI_Reduce(&local_min, &global_min, 1, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Min: %d\n", global_min);
    }

    MPI_Finalize();
    return 0;
}


*******************************************************************************************
slip no.17

Q.1 Write a program to simulate Indexed file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned above and implement each option.
• Show Bit Vector
• Show Directory
• Delete Already File
• Exit

ANS:-
#include<stdio.h>
#include<stdlib.h>
main()
{
int f[50], index[50],i, n, st, len, j, c, k, ind,count=0;
for(i=0;i<50;i++)
f[i]=0;
x:printf("Enter the index block: ");
 scanf("%d",&ind);
 if(f[ind]!=1)
 {
 printf("Enter no of blocks needed for the index %d on the disk : \n", ind);
 scanf("%d",&n);
 }
 else
 {
 printf("%d index is already allocated \n",ind);
 goto x;
 }
y: count=0;
 printf("Enter blocks no for index %d on the disk : \n", ind);
 for(i=0;i<n;i++)
 {
 scanf("%d", &index[i]);
 if(f[index[i]]==0)
 count++;
 }
 if(count==n)
 {
 for(j=0;j<n;j++)
 f[index[j]]=1;
 printf("Allocated\n");
 printf("File Indexed\n");
 for(k=0;k<n;k++)
 printf("%d-------->%d : %d\n",ind,index[k],f[index[k]]);
 }
 else
 {
 printf("File in the index is already allocated \n");
 printf("Enter another file indexed");
 goto y;
 }
 printf("Do you want to enter more file(Yes - 1/No - 0)");
 scanf("%d", &c);
 if(c==1)
 goto x;
 else
 exit(0);
 getch();
}

Q.2 Write a simulation program for disk scheduling using LOOK algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
23, 89, 132, 42, 187, 69, 36, 55
Start Head Position: 40
 Direction: Left

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
 main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,min);
thm+=start-min;
start=min;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,max);
thm+=max-start;
start=max;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}




*******************************************************************************************
slip no.18

Q.1 Write a program to simulate Indexed file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned above and implement each option.
• Show Bit Vector
• Create New File
• Show Directory
• Delete File
• Exit

ANS:-
#include<stdio.h>
#include<stdlib.h>
main()
{
int f[50], index[50],i, n, st, len, j, c, k, ind,count=0;
for(i=0;i<50;i++)
f[i]=0;
x:printf("Enter the index block: ");
 scanf("%d",&ind);
 if(f[ind]!=1)
 {
 printf("Enter no of blocks needed for the index %d on the disk : \n", ind);
 scanf("%d",&n);
 }
 else
 {
 printf("%d index is already allocated \n",ind);
 goto x;
 }
y: count=0;
 printf("Enter blocks no for index %d on the disk : \n", ind);
 for(i=0;i<n;i++)
 {
 scanf("%d", &index[i]);
 if(f[index[i]]==0)
 count++;
 }
 if(count==n)
 {
 for(j=0;j<n;j++)
 f[index[j]]=1;
 printf("Allocated\n");
 printf("File Indexed\n");
 for(k=0;k<n;k++)
 printf("%d-------->%d : %d\n",ind,index[k],f[index[k]]);
 }
 else
 {
 printf("File in the index is already allocated \n");
 printf("Enter another file indexed");
 goto y;
 }
 printf("Do you want to enter more file(Yes - 1/No - 0)");
 scanf("%d", &c);
 if(c==1)
 goto x;
 else
 exit(0);
 getch();
}

Q.2 Write a simulation program for disk scheduling using SCAN algorithm. Accept
total number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total
number of head moments.
 33, 99, 142, 52, 197, 79, 46, 65
Start Head Position: 72
Direction: Right

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
start=0;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,nob-1);
thm+=(nob-1)-start;
start=nob-1;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}




*******************************************************************************************
slip no.19

Q.1 Write a C program to simulate Banker’s algorithm for the purpose of
deadlock avoidance.Consider the following snapshot of system, A, B, C and D
is the resource type.
Proces
s
Allocation Max Available
A B C D A B C D A B C D
P0 0 3 2 4 6 5 4 4 3 4 4 2
P1 1 2 0 1 4 4 4 4
P2 0 0 0 0 0 0 1 2
P3 3 3 2 2 3 9 3 4
P4 1 4 3 2 2 5 3 3
P5 2 4 1 4 4 6 3 4
a) Calculate and display the content of need matrix?
b) Is the system in safe state? If display the safe sequence

ANS:-

#include<stdio.h>
int main()
{
    int n,m,i,j,k;
    n=5; // Number of processes
    m=4; // Number of resources
// CAHNGE THE MATRIX *********************************
    int alloc[5][4] = { { 0, 0, 1, 2 }, // P0 // Allocation Matrix
                        { 1, 0, 0, 0 }, // P1
                        { 1, 3, 5, 4 }, // P2
                        { 0, 6, 3, 2 }, // P3
                        { 0, 0, 1, 4 } }; // P4

    int max[5][4] = { { 0, 0, 1, 2 }, // P0 // MAX Matrix
                      { 1, 7, 5, 0 }, // P1
                      { 2, 3, 5, 6 }, // P2
                      { 0, 6, 5, 2 }, // P3
                      { 0, 6, 5, 6 } }; // P4

    int avail[4] = {1 ,5 ,2 ,0}; // Available Resources

    int f[n],ans[n],ind=0;
    for (k=0;k<n;k++) {
        f[k] = 0;
    }
    int need[n][m];
    for (i=0;i<n;i++) {
        for (j=0;j<m;j++)
            need[i][j] = max[i][j] - alloc[i][j];
    }
    int y=0;
    for (k=0;k<5;k++)
    {
        for (i=0;i<n;i++)
        {
            if (f[i]==0)
            {

                int flag=0;
                for (j=0;j<m;j++)
                {
                    if (need[i][j]>avail[j]){
                        flag=1;
                         break;
                    }
                }

                if (flag==0)
                {
                    ans[ind++]=i;
                    for (y=0;y<m;y++)
                        avail[y]+=alloc[i][y];
                    f[i]=1;
                }
            }
        }
    }

    printf("Need Matrix:\n");
    for (i = 0 ; i < n ; i++)
    {
        for (j = 0 ; j < m ; j++)
        {
            printf("%d ",need[i][j]);
        }
        printf("\n");
    }

    printf("System is in safe state.\nSafe sequence is: ");
    for (i=0;i<n-1;i++)
        printf("P%d -> ",ans[i]);
    printf("P%d",ans[n-1]);

    return(0);
}

Q.2 Write a simulation program for disk scheduling using C-SCAN algorithm. Accept
total number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
23, 89, 132, 42, 187, 69, 36, 55
Start Head Position: 40
Direction: Left

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);printf("\nEnter Direction: ");
scanf(" %c",&direction);
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
printf("\n%d-%d",nob-1,ReqString[spos+1]);
thm+=(nob-1)-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",nob-1,start);
thm+=(nob-1)-start;
printf("\n%d-0",ReqString[spos-1]);
thm+=ReqString[spos-1]-0;
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.20

Q.1 Write a simulation program for disk scheduling using SCAN algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
33, 99, 142, 52, 197, 79, 46, 65
Start Head Position: 72
 Direction: User defined

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
start=0;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,nob-1);
thm+=(nob-1)-start;
start=nob-1;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}

Q.2 Write an MPI program to find the max number from randomly generated 1000 numbers
(stored in array) on a cluster (Hint: Use MPI_Reduce)

ANS:-

#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_max = 0;
    int global_max = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] > local_max) {
            local_max = numbers[i];
        }
    }

    MPI_Reduce(&local_max, &global_max, 1, MPI_INT, MPI_MAX, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Max: %d\n", global_max);
    }

    MPI_Finalize();
    return 0;
}


*******************************************************************************************
slip no.21

Q.1 Write a simulation program for disk scheduling using FCFS algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
 55, 58, 39, 18, 90, 160, 150, 38, 184
 Start Head Position: 50

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
main()
{
int i,j,k;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
for(i=0;i<nor;i++)
{
printf("\n%d-%d",start,ReqString[i]);
 if(ReqString[i]>=start)
 {
thm+=ReqString[i]-start;
start=ReqString[i];
 }
 else
 {
 thm+=start-ReqString[i];
 start=ReqString[i];
 }
}
printf("\nTotal Head Movement: %d",thm);
}

Q.2 Write an MPI program to calculate sum of all even randomly generated 1000
numbers (stored in array) on a cluster

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 == 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of even numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}


*******************************************************************************************
slip no.22

Q.1 Write an MPI program to calculate sum of all odd randomly generated 1000 numbers
(stored in array) on a cluster.

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 != 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of odd numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}


Q.2 Write a program to simulate Sequential (Contiguous) file allocation method.
Assume disk with n number of blocks. Give value of n as input. Randomly mark some
block as allocated and accordingly maintain the list of free blocks Write menu driver
program with menu options as mentioned below and implement each option
• Show Bit Vector
• Delete already created file
• Exit

ANS:-
#include <stdio.h>
#include <stdlib.h>
void contiguous(int files[])
{
 int flag = 0, startBlock, len, j, k, ch;
 char fname[20]; 
 printf("\nEnter Name of File: ");
 scanf("%s",&fname);
 printf("Enter the starting block of the files: ");
 scanf("%d", &startBlock);
 printf("Enter the length of the files: ");
 scanf("%d", &len);
 for (j = startBlock; j < (startBlock + len); j++)
 {
 if (files[j] == 0)
 flag++;
 }
 if (len == flag)
 {
 for (k = startBlock; k < (startBlock + len); k++)
 {
 if (files[k] == 0)
 {
 files[k] = 1;
 printf("%d\t%d\n", k, files[k]);
 }
 }
 if (k != (startBlock + len - 1))
 printf("The file is allocated to the disk\n");
 }
 else
 printf("The file is not allocated to the disk\n");
 printf("Do you want to enter more files?\n");
 printf("Press 1 for YES, 0 for NO: ");
 scanf("%d", &ch);
 if (ch == 1)
 contiguous(files);
 else
 exit(0);
 return;
}
main()
{
 int i,files[50];
 for (i = 0; i < 50; i++)
 files[i] = 0;
 printf("Files Allocated are :\n");
 contiguous(files);
 return 0;
}



*******************************************************************************************
slip no.23

Q.1 Consider a system with ‘m’ processes and ‘n’ resource types. Accept number of
instances for every resource type. For each process accept the allocation and maximum
requirement matrices. Write a program to display the contents of need matrix and to
check if the given request of a process can be granted immediately or not

ANS:-
??????

Q.2 Write a simulation program for disk scheduling using SSTF algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
24, 90, 133, 43, 188, 70, 37, 55
Start Head Position: 58

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
void sort(int RS[])
{
int i,j,temp;
for(i=1;i<nor;i++)
{
for(j=0;j<nor-i;j++)
{
if(RS[j]>RS[j+1])
{
temp=RS[j];
RS[j]=RS[j+1];
RS[j+1]=temp;
}
}
}
}
void display(int RS[])
{
int i;
for(i=0;i<nor;i++)
printf(" %d",RS[i]);
}
int searchstartblock(int RS[])
{
int i;
for(i=0;i<nor;i++)
{
if(RS[i]==start)
return i;
}
return 1;
}
main()
{
int
i,j,k,ans,finish=0,leftpos,startpos,rightpos,leftdis,rightdis,initpos,flag=0;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
ans=searchstartblock(ReqString);
if(ans==1)
ReqString[nor++]=start;
sort(ReqString);
printf("\nRefString After Sorting: ");
display(ReqString);
startpos=searchstartblock(ReqString);
initpos=startpos;
leftpos=initpos-1;
rightpos=initpos+1;
while(1)
{
leftdis=ReqString[startpos]-ReqString[leftpos];
rightdis=ReqString[rightpos]-ReqString[startpos];
if(leftdis<rightdis)
{
printf("\n%d-%d",ReqString[startpos],ReqString[leftpos]);thm+=ReqString[startpos]-ReqString[leftpos];
startpos=leftpos;
leftpos--;
if(leftpos==-1)
{
break;
}

}
else if(leftdis>rightdis)
{
printf("\n%d-%d",ReqString[rightpos],ReqString[startpos]);
thm+=ReqString[rightpos]-ReqString[startpos];
startpos=rightpos;
rightpos++;
if(rightpos==nor)
{
break;
}
}
}
 printf("\nEnd of while loop");
while(leftpos>=0)
{
printf("\n%d-%d",ReqString[startpos],ReqString[leftpos]);thm+=ReqString[startpos]-ReqString[leftpos];
startpos=leftpos;
leftpos--;
}
while(rightpos<nor)
{
printf("\n%d-%d",ReqString[rightpos],ReqString[startpos]);thm+=ReqString[rightpos]-ReqString[startpos];
startpos=rightpos;
rightpos++;
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.24

Q.1 Write an MPI program to calculate sum of all odd randomly generated 1000 numbers
(stored in array) on a cluster.

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 != 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of odd numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}


Q.2 Write a C program to simulate Banker’s algorithm for the purpose of deadlock
avoidance.The following snapshot of system, A, B, C and D are the resource type.
Proces
s
Allocation Max Available
A B C A B C A B C
P0 0 1 0 0 0 0 0 0 0
P1 2 0 0 2 0 2
P2 3 0 3 0 0 0
P3 2 1 1 1 0 0
P4 0 0 2 0 0 2
a)Calculate and display the content of need matrix?
b)Is the system in safe state? If display the safe sequence.

ANS:-
#include<stdio.h>
int main()
{
    int n,m,i,j,k;
    n=5; // Number of processes
    m=4; // Number of resources
// CHANGE THE MATRIX
    int alloc[5][4] = { { 0, 0, 1, 2 }, // P0 // Allocation Matrix
                        { 1, 0, 0, 0 }, // P1
                        { 1, 3, 5, 4 }, // P2
                        { 0, 6, 3, 2 }, // P3
                        { 0, 0, 1, 4 } }; // P4

    int max[5][4] = { { 0, 0, 1, 2 }, // P0 // MAX Matrix
                      { 1, 7, 5, 0 }, // P1
                      { 2, 3, 5, 6 }, // P2
                      { 0, 6, 5, 2 }, // P3
                      { 0, 6, 5, 6 } }; // P4

    int avail[4] = {1 ,5 ,2 ,0}; // Available Resources

    int f[n],ans[n],ind=0;
    for (k=0;k<n;k++) {
        f[k] = 0;
    }
    int need[n][m];
    for (i=0;i<n;i++) {
        for (j=0;j<m;j++)
            need[i][j] = max[i][j] - alloc[i][j];
    }
    int y=0;
    for (k=0;k<5;k++)
    {
        for (i=0;i<n;i++)
        {
            if (f[i]==0)
            {

                int flag=0;
                for (j=0;j<m;j++)
                {
                    if (need[i][j]>avail[j]){
                        flag=1;
                         break;
                    }
                }

                if (flag==0)
                {
                    ans[ind++]=i;
                    for (y=0;y<m;y++)
                        avail[y]+=alloc[i][y];
                    f[i]=1;
                }
            }
        }
    }

    printf("Need Matrix:\n");
    for (i = 0 ; i < n ; i++)
    {
        for (j = 0 ; j < m ; j++)
        {
            printf("%d ",need[i][j]);
        }
        printf("\n");
    }

    printf("System is in safe state.\nSafe sequence is: ");
    for (i=0;i<n-1;i++)
        printf("P%d -> ",ans[i]);
    printf("P%d",ans[n-1]);

    return(0);
}




*******************************************************************************************
slip no.25

Q.1 Write a simulation program for disk scheduling using LOOK algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
86, 147, 91, 170, 95, 130, 102, 70
Starting Head position= 125
Direction: User Defined

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
 main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,min);
thm+=start-min;
start=min;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,max);
thm+=max-start;
start=max;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}

Q.2 Write a program to simulate Linked file allocation method. Assume disk with n
number of blocks. Give value of n as input. Randomly mark some block as allocated and
accordingly maintain the list of free blocks Write menu driver program with menu
options as mentioned below and implement each option.
• Show Bit Vector
• Create New File
• Show Directory
• Exit

ANS:-
#include <stdio.h>
#include <stdlib.h>
int main(){
 int f[50], p, i, st, len, j, c, k, a;
 for (i = 0; i < 50; i++)
 f[i] = 0;
 printf("Enter how many blocks already allocated: ");
 scanf("%d", &p);
 printf("Enter blocks already allocated: ");
 for (i = 0; i < p; i++) {
 scanf("%d", &a);
 f[a] = 1;
 }
x:
 printf("Enter starting block: ");
 scanf("%d",&st);
 printf("Enter length of the file: ");
 scanf("%d",&len);
 k = len;
 if (f[st] == 0)
 {
 for (j = st; j < (st + k); j++){
 if (f[j] == 0){
 f[j] = 1;
 printf("%d-------->%d\n", j, f[j]);
 }
 else{
 printf("%d Block is already allocated \n", j);
 k++;
 }
 }
 }
 else
 printf("%d starting block is already allocated \n", st);
 printf("Do you want to enter more file(Yes - 1/No - 0)");
 scanf("%d", &c);
 if (c == 1)
 goto x;
 else
 exit(0);
 return 0;
}



*******************************************************************************************
slip no.26

Q.1 Write a C program to simulate Banker’s algorithm for the purpose of
deadlock avoidance.Consider the following snapshot of system, A, B, C and D
is the resource type.
Proces
s
Allocation Max Available
A B C D A B C D A B C D
P0 0 0 1 2 0 0 1 2 1 5 2 0
P1 1 0 0 0 1 7 5 0
P2 1 3 5 4 2 3 5 6
P3 0 6 3 2 0 6 5 2
P4 0 0 1 4 0 6 5 6
a)Calculate and display the content of need matrix?
b)Is the system in safe state? If display the safe sequence.

ANS:-
#include<stdio.h>
int main()
{
    int n,m,i,j,k;
    n=5; // Number of processes
    m=4; // Number of resources
// CHANGE THE MATRIX
    int alloc[5][4] = { { 0, 0, 1, 2 }, // P0 // Allocation Matrix
                        { 1, 0, 0, 0 }, // P1
                        { 1, 3, 5, 4 }, // P2
                        { 0, 6, 3, 2 }, // P3
                        { 0, 0, 1, 4 } }; // P4

    int max[5][4] = { { 0, 0, 1, 2 }, // P0 // MAX Matrix
                      { 1, 7, 5, 0 }, // P1
                      { 2, 3, 5, 6 }, // P2
                      { 0, 6, 5, 2 }, // P3
                      { 0, 6, 5, 6 } }; // P4

    int avail[4] = {1 ,5 ,2 ,0}; // Available Resources

    int f[n],ans[n],ind=0;
    for (k=0;k<n;k++) {
        f[k] = 0;
    }
    int need[n][m];
    for (i=0;i<n;i++) {
        for (j=0;j<m;j++)
            need[i][j] = max[i][j] - alloc[i][j];
    }
    int y=0;
    for (k=0;k<5;k++)
    {
        for (i=0;i<n;i++)
        {
            if (f[i]==0)
            {

                int flag=0;
                for (j=0;j<m;j++)
                {
                    if (need[i][j]>avail[j]){
                        flag=1;
                         break;
                    }
                }

                if (flag==0)
                {
                    ans[ind++]=i;
                    for (y=0;y<m;y++)
                        avail[y]+=alloc[i][y];
                    f[i]=1;
                }
            }
        }
    }

    printf("Need Matrix:\n");
    for (i = 0 ; i < n ; i++)
    {
        for (j = 0 ; j < m ; j++)
        {
            printf("%d ",need[i][j]);
        }
        printf("\n");
    }

    printf("System is in safe state.\nSafe sequence is: ");
    for (i=0;i<n-1;i++)
        printf("P%d -> ",ans[i]);
    printf("P%d",ans[n-1]);

    return(0);
}

Q.2 Write a simulation program for disk scheduling using FCFS algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user.
Display the list of request in the order in which it is served. Also display the total number
of head moments.
 56, 59, 40, 19, 91, 161, 151, 39, 185
 Start Head Position: 48

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
main()
{
int i,j,k;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
for(i=0;i<nor;i++)
{
printf("\n%d-%d",start,ReqString[i]);
 if(ReqString[i]>=start)
 {
thm+=ReqString[i]-start;
start=ReqString[i];
 }
 else
 {
 thm+=start-ReqString[i];
 start=ReqString[i];
 }
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.27

Q.1 Write a simulation program for disk scheduling using LOOK algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
 176, 79, 34, 60, 92, 11, 41, 114
 Starting Head Position: 65
Direction: Right

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
 main()
{
int i,j,k,max,min;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,min);
thm+=start-min;
start=min;
printf("\n%d-%d",max,start);
thm+=max-start;
}
else if(direction=='R')
{
printf("\n%d-%d",start,max);
thm+=max-start;
start=max;
printf("\n%d-%d",start,min);
thm+=start-min;
}
printf("\nTotal Head Movement: %d",thm);
}


Q. C-SCAN

#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);printf("\nEnter Direction: ");
scanf(" %c",&direction);
if(direction=='L')
{
printf("\n%d-0",start);
thm+=start-0;
printf("\n%d-%d",nob-1,ReqString[spos+1]);
thm+=(nob-1)-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",nob-1,start);
thm+=(nob-1)-start;
printf("\n%d-0",ReqString[spos-1]);
thm+=ReqString[spos-1]-0;
}
printf("\nTotal Head Movement: %d",thm);
}

Q.2 Write an MPI program to find the min number from randomly generated 1000 numbers
(stored in array) on a cluster (Hint: Use MPI_Reduce)

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_min = 100;
    int global_min = 100;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] < local_min) {
            local_min = numbers[i];
        }
    }

    MPI_Reduce(&local_min, &global_min, 1, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Min: %d\n", global_min);
    }

    MPI_Finalize();
    return 0;
}



*******************************************************************************************
slip no.28

Q.1 Write a simulation program for disk scheduling using C-LOOK algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
 56, 59, 40, 19, 91, 161, 151, 39, 185
 Start Head Position: 48
 Direction: User Defined

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,max,min,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,ReqString[0]);
thm+=start-ReqString[0];
printf("\n%d-%d",max,ReqString[spos+1]);
thm+=max-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",max,start);
thm+=max-start;
printf("\n%d-%d",ReqString[spos-1],min);
thm+=ReqString[spos-1]-min;
}
printf("\nTotal Head Movement: %d",thm);
}


Q.2 Write an MPI program to calculate sum of randomly generated 1000 numbers
(stored in array) on a cluster

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        local_sum += numbers[i];
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}

*******************************************************************************************
slip no.29

Q.1 Write an MPI program to calculate sum of all even randomly generated 1000 numbers
(stored in array) on a cluster. 

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_sum = 0;
    int global_sum = 0;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] % 2 == 0) {
            local_sum += numbers[i];
        }
    }

    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Sum of even numbers: %d\n", global_sum);
    }

    MPI_Finalize();
    return 0;
}


Q.2 Write a simulation program for disk scheduling using C-LOOK algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.. 
80, 150, 60,135, 40, 35, 170
Starting Head Position: 70
Direction: Right

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm,min[10],max[10];
char direction;
int getmin()
{
int i,j=0,min=999;
for(i=0;i<nor;i++)
if(ReqString[i]<=min)
min=ReqString[i];
return min;
}
int getmax()
{
int i,j=0,max=0;
for(i=0;i<nor;i++)
if(ReqString[i]>max)
max=ReqString[i];
return max;
}
void sort(int RS[])
{
 int i,j,temp;
 for(i=1;i<nor;i++)
 {
 for(j=0;j<nor-i;j++)
 {
 if(RS[j]>RS[j+1])
 {
 temp=RS[j];
 RS[j]=RS[j+1];
 RS[j+1]=temp;
 }
 }
 }
}
main()
{
int i,j,k,max,min,spos;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
 ReqString[nor++]=start;
 sort(ReqString);
 for(i=0;i<nor;i++)
 printf(" %d",ReqString[i]);
 for(spos=0;spos<=nor && ReqString[spos]!=start; spos++);
printf("\nEnter Direction: ");
scanf(" %c",&direction);
min=getmin();
max=getmax();
if(direction=='L')
{
printf("\n%d-%d",start,ReqString[0]);
thm+=start-ReqString[0];
printf("\n%d-%d",max,ReqString[spos+1]);
thm+=max-ReqString[spos+1];
}
else if(direction=='R')
{
printf("\n%d-%d",max,start);
thm+=max-start;
printf("\n%d-%d",ReqString[spos-1],min);
thm+=ReqString[spos-1]-min;
}
printf("\nTotal Head Movement: %d",thm);
}


*******************************************************************************************
slip no.30

Q.1 Write an MPI program to find the min number from randomly generated 1000 numbers
(stored in array) on a cluster (Hint: Use MPI_Reduce)

ANS:-
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 1000

int main(int argc, char** argv) {
    int rank, size;
    int local_min = 100;
    int global_min = 100;
    int numbers[N];
    int i;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (rank == 0) {
        srand(time(NULL));
        for (i = 0; i < N; i++) {
            numbers[i] = rand() % 100;
        }
    }

    MPI_Bcast(numbers, N, MPI_INT, 0, MPI_COMM_WORLD);

    for (i = rank; i < N; i += size) {
        if (numbers[i] < local_min) {
            local_min = numbers[i];
        }
    }

    MPI_Reduce(&local_min, &global_min, 1, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Min: %d\n", global_min);
    }

    MPI_Finalize();
    return 0;
}


Q.2 Write a simulation program for disk scheduling using FCFS algorithm. Accept total
number of disk blocks, disk request string, and current head position from the user. Display
the list of request in the order in which it is served. Also display the total number of head
moments.
 65, 95, 30, 91, 18, 116, 142, 44, 168
 Start Head Position: 52

ANS:-
#include<stdio.h>
int ReqString[20],nor,nob,start,thm;
main()
{
int i,j,k;
printf("\nEnter No.of Requests: ");
scanf("%d",&nor);
printf("\nEnter Requests:\n");
for(i=0;i<nor;i++)
{
printf("[%d]=",i);
scanf("%d",&ReqString[i]);
}
printf("\nEnter No.of Cylinders: ");
scanf("%d",&nob);
printf("\nEnter Start Block: ");
scanf("%d",&start);
for(i=0;i<nor;i++)
{
printf("\n%d-%d",start,ReqString[i]);
 if(ReqString[i]>=start)
 {
thm+=ReqString[i]-start;
start=ReqString[i];
 }
 else
 {
 thm+=start-ReqString[i];
 start=ReqString[i];
 }
}
printf("\nTotal Head Movement: %d",thm);
}

