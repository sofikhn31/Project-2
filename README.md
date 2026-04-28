# Project-2
Implementation of FCFS in C

#include<stdio.h>
int main()
{
int n,i,j;
printf("\nEnter no. of processes: ");
scanf("%d",&n);
int at[n],bt[n];
for(i=0;i<n;i++)
{
printf("\nEnter arrival and burst time of process T%d :",i+1);
scanf("%d%d",&at[i],&bt[i]);
}
int ct[n];
ct[0]=at[0]+bt[0];
for(j=1;j<n;j++)
{
if(ct[j-1]<at[j])
{
ct[j]=at[j]+bt[j];
}
else
{
ct[j]=ct[j-1]+bt[j];
}
}
printf("\nTotal time: %d",ct[n-1]);
for(i=0;i<n;i++)
{
printf("\nTAT for process %d is %d: ",i+1,ct[i]-at[i]);
}
printf("\nWT for process 1: 0");
int wt;
for(i=1;i<n;i++)
{
wt=at[i]-ct[i-1];
if(wt<=0)
{
printf("\nWT for process %d: %d",i+1,wt);
printf("\nCPU is idle between process %d and %d",i,i+1);
}
else
{
printf("\nWT for process %d: 0",i+1);
}
}
printf("\nNo. of context switch: %d\n",n-1);
}

------------------------------------------------------------------------------------------------

Implementation of SJF in C

#include<stdio.h>

void main()

{

int bt[20],p[20],wt[20],tat[20],i,j,n,total=0,pos,temp;

float avg_wt,avg_tat;

printf("Enter number of process:");

scanf("%d",&n);

printf("\nEnter Burst Time:\n");

for(i=0;i<n;i++)

{

printf("T%d:",i+1);

scanf("%d",&bt[i]);

p[i]=i+1; //contains process number

}

//sorting burst time in ascending order

for(i=0;i<n;i++)

{

pos=i;

for(j=i+1;j<n;j++)

{

if(bt[j]<bt[pos])

pos=j;

}

temp=bt[i];

bt[i]=bt[pos];

bt[pos]=temp;

temp=p[i];

p[i]=p[pos];

p[pos]=temp;

}

wt[0]=0; //waiting time for first process will be zero hi

//calculate waiting time

for(i=1;i<n;i++)

{

wt[i]=0;

for(j=0;j<i;j++)

wt[i]+=bt[j];

total+=wt[i];

}

avg_wt=(float)total/n; //average waiting time

total=0;

printf("\nProcess\t Burst Time \tWaiting Time\tTurnaround Time");

for(i=0;i<n;i++)

{

tat[i]=bt[i]+wt[i]; //calculate turnaround time

total+=tat[i];

printf("\np%d\t\t %d\t\t %d\t\t\t%d",p[i],bt[i],wt[i],tat[i]);

}

avg_tat=(float)total/n; //average turnaround time

printf("\n\nAverage Waiting Time=%f",avg_wt);

printf("\nAverage Turnaround Time=%f\n",avg_tat);

}

--------------------------------------------------------------------------------

Implementation of RR in C

#include<stdio.h>

int min(int a,int b){
  if(b<a) return b;
  return a;
}

void swap(int *a,int *b){
      int temp = *a;
      *a = *b;
      *b = temp;
}

int main()
{
 int a[1000],b[1000],x[1000],index[1000];
 int w[1000],t[1000],c[1000];
 int i,j,smallest,count=0,time,n;
 double avg=0,tt=0,end;

 printf("\nEnter the number of Processes: ");
 scanf("%d",&n);

 for(i=0;i<n;i++)
 {
   printf("\nEnter arrival time  ----  burst time %d : ",i+1);
   scanf("%d%d",&a[i],&b[i]);
   x[i]=b[i];
   index[i]=i+1;
 }

 int tq = 10;//Time Quant,
  for(int i = 0;i<n;i++){
        for(int j = i+1;j<n;j++){
          if(a[j]<a[j-1]){
               swap(&a[j],&a[j-1]);
               swap(&b[j],&b[j-1]);
               swap(&index[j],&index[j-1]);
            }
        } 
  }

   i = 0;
 for(time=0;count!=n;)
 {

   int j = 0;
   int k = i;
   while((a[i]>time || b[i]==0) && j<n) {
    i = (i+1)%n;
    j++;
  }

  
  if(j==n){
      time++;
      i=k;
      continue;
  }
  

  time += min(tq,b[i]);
  b[i] -= min(tq,b[i]);

  if(b[i]==0)
  {
   count++;
   end=time;
   c[i] = end;
   w[i] = end - a[i] - x[i];
   t[i] = end - a[i];
  
  }
  i = (i+1)%n;

 }
 printf("pid \t burst \t arrival \twaiting \tturnaround \tcompletion");
 for(i=0;i<n;i++)
 {
   printf("\n %d \t   %d \t %d\t\t%d   \t\t%d\t\t%d",index[i],x[i],a[i],w[i],t[i],c[i]);
   avg = avg + w[i];
   tt = tt + t[i];
 }
 printf("\n  %If   %If",avg,tt);
 printf("\n\nAverage waiting time = %lf\n",avg/n);
 printf("Average Turnaround time = %lf",tt/n);
}
---------------------------------------------------------------------------

Implementation of PS in C (Arrival time is assumed to be 0)

#include<stdio.h>

int main()
{
int a[1000],b[1000],x[1000],p[1000];
int w[1000],t[1000],c[1000];
int i,j,smallest,count=0,time,n;
double avg=0,tt=0,end;

printf("\nEnter the number of Processes: ");
scanf("%d",&n);

for(i=0;i<n;i++)
{
printf("\nEnter priority of process --- burst time %d : ",i+1);
scanf("%d%d",&p[i],&b[i]);

a[i] = 0;//Arrival time is 0
x[i]=b[i];
}

p[999]=99999;
a[999]=99999;

for(time=0;count!=n;)
{
smallest=999;
for(i=0;i<n;i++)
{
if(a[i]<=time && p[i]<p[smallest] && b[i]>0)
smallest=i;
else if(a[i]<=time && p[i]==p[smallest] && b[i]>0)
smallest = i;
}
if(smallest==999){
time++;
continue;
}
time += b[smallest];
b[smallest]==0;

if(b[smallest]==0)
{
count++;
end=time+1;
c[smallest] = end;
w[smallest] = end - a[smallest] - x[smallest];

{
printf("\n %d \t %d \t %d\t\t%d \t\t%d\t\t%d",i+1,x[i],a[i],w[i],t[i],c[i]);
avg = avg + w[i];
tt = tt + t[i];
}
printf("\n %If %If",avg,tt);
printf("\n\nAverage waiting time = %lf\n",avg/n);
printf("Average Turnaround time = %lf",tt/n);
}

--------------------------------------------------------------------------------------------

If you have any doubts, please ask in the comments section below.

t[smallest] = end - a[smallest];

}
}
printf("pid \t burst \t arrival \twaiting \tturnaround \tcompletion");
for(i=0;i<n;i++)
