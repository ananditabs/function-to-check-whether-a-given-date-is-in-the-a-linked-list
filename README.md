# function-to-check-whether-a-given-date-is-in-the-a-linked-list
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
 
#define NEW(x) (x*)malloc(sizeof(x))
 // defining datastructure date, schedule 
typedef struct DATE
{int day;
int month;
}date;
 
typedef struct NODE
{
                date *dp;
                struct NODE *next;
                struct NODE *prev;
}node;
typedef struct SCHEDULE
{   long num;
                node *earliest;
                node *latest;
}schedule;
 
 // creation of node and schedule
node *make_node(int d,int m)
{node *temp;
temp=NEW(node);
temp->dp=NEW(date);
if(temp!=NULL)
{temp->dp->day=d;
temp->dp->month=m;
temp->next=NULL;
temp->prev=NULL;
}
return temp;}
 
schedule* make_schedule(void)
{schedule *temp;
temp=NEW(schedule);
if(temp!=NULL)
{temp->num=0;
temp->earliest=NULL;
temp->latest=NULL;
}
return temp;
}
 
//traversing the linked list
void display_list(schedule *sch)
{node *temp;
if(sch!=NULL)
{temp=sch->earliest;
do
{
printf("%3d",temp->dp->month);
printf("%3d\n ",temp->dp->day);
temp=temp->next;
}while(temp!=NULL);
}
printf("\n");
}
//search for the date input by the user 
int search(schedule *sch,int d,int m)
{node *temp;
int count=0;
if(sch!=NULL)
{temp=sch->earliest;
do
{if(temp->dp->day==d && temp->dp->month==m )
                {printf("this date is in the list\n");
                count++;
				exit (0);
                }
 
else       
                temp=temp->next;
}while(temp!=NULL);
}
if(count==0)
{printf("the data does not exist in the list");}
printf("\n");

return 0;}
 // insert at head 
int insert_at_head(schedule *sch,int d, int m)
{node *temp;
temp=make_node(d,m);
 
if(temp==NULL)return -1;// fail, could not create a new node
if (sch==NULL)
{sch=make_schedule();
if (sch==NULL)return -1;// fail, could not create root
}
sch->num++;
temp->next=NULL;
temp->prev=NULL;
sch->earliest=temp;
sch->latest=temp;//i dont know where we are initialising the next and previos pointers of the earliest and latest
printf("head node created\n");
return 0;
}
 
//inserting at tail
int insert_at_tail(schedule *sch,int d, int m)
{node *temp;
temp=make_node(d,m);
if(temp==NULL)return -1;
if(sch==NULL)
{sch=make_schedule();
if(sch==NULL)return -1;
}
sch->num++;
if(sch->num==1)//if the list was empty previously
sch->earliest=sch->latest=temp;
else
{sch->latest->next=temp;
temp->prev=sch->latest;
sch->latest=temp;
}
return 0;
}
 
// now a combination of searching and inserting. Insertion is done only when the date being passed in the function is smaller than the list of dates in schedule 
int insert_at_position(schedule *sch, int d,int m)
{node *temp,*this;
temp=make_node(d,m);
if(temp==NULL)return -1;// could not create node
if(sch==NULL)
{sch=make_schedule();
if(sch==NULL)return-1;//could not create root
}
this=sch->earliest;
//inserting at position
if(this->dp->month>temp->dp->month)
{//printf("1");
                temp->next=this;
                temp->prev=NULL;
                sch->earliest=temp;
                this->prev=temp;
                this->next=NULL;
                sch->num++;
   return 0;
}               
else if(this->dp->month==temp->dp->month && (this->dp->day>=temp->dp->day))
   {          
                printf("2");
                temp->next=this;
                temp->prev=NULL;
                sch->earliest=temp;
                this->prev=temp;
                sch->num++;
                return(0);
   }
else if(sch->num==1)// if list is empty, then the current node is assigned to latest in the list 
  {
                temp->next=NULL;
                temp->prev=this;
                sch->latest=temp;
                sch->num++;
                this->next=temp;
                this->prev=NULL;
                return(0);
}
else
{            
                while(this->next!=NULL)//till the last node, keep checking each node 
                 {
                                this=this->next;
                         if(this->next==NULL)// if its the last node then assign the latest node to the node being inserted 
                            {
                                                                temp->prev=this;
                                                                                temp->next=NULL;
                                                                                sch->latest=temp;
                                                                                this->next=temp;
                                                                                sch->num++;
                                                                                return(0);
                                        }
                         else if((this->dp->month >temp->dp->month ||(this->dp->month == temp->dp->month && this->dp->day >= temp->dp->day )))// if the date passsed in the function is less than the current date , then insert in the required position 
                                                                {
                                                                                temp->next=this;
                                                                                temp->prev=this->prev;
                                                                                this->prev->next=temp;
                                                                                this->prev=temp;
                                                                                sch->num++;
                                                                                return(0);
                                                                }
                    }
                }
 
return(0);}
 
int main(int argc, char * argv[])
{schedule *sch;
int da, mo;
int *d,*m;
int i;
int a;
node *n;
FILE *fp;
sch=make_schedule();
fp=fopen(argv[1],"r");
while(fscanf(fp,"%d %d",&mo,&da)!=EOF)
{
                printf("%d %d\n",mo,da);
                if(sch->num!=0)
                {
                                insert_at_position(sch,da,mo);
                }
                else if(sch->num==0)
                                {insert_at_head(sch,da,mo);
                                }
}
int ds,ms;
display_list(sch);
printf("enter the date to be searched (mm:dd)");
scanf("%d %d",&ms,&ds);
search(sch,ds,ms);
fclose(fp);
}
 
 
 
 
