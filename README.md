# LinkedList
Implementation of Queue and Stack using arrays and singly linked lists to manage service requests.
## STudent Information
Name:Tasmim Jannat
ID:26002159
Course:BSC in CSE
## Experiment Title
LinkedList
## Objective
•	To implement Stack and Queue using Arrays and Singly Linked Lists.
•	To perform ADD, SERVE, and REOPEN operations.
•	To understand how Stack and Queue work.
•	To handle empty Stack and Queue conditions.
•	To compare the results of Array and Linked List implementations.
## Problem Analysis
In this problem, an organization manages service requests using a Queue and a Stack. Each request has a unique integer ID. The Queue follows FIFO (First In, First Out), which means the request that comes first will be served first. The Stack follows LIFO (Last In, First Out), which means the most recently processed request will be reopened first.We need to implement both data structures using Arrays and Linked Lists and make sure that both implementations give the same result.
## Solution Approach
First, we take the initial request IDs from the user and insert them into both Array Queue and Linked Queue. Then we perform the given operations:
•	ADD X: Insert a new request at the end of both Queues.
•	SERVE: Remove the first request from the Queue and push it into both Stacks.
•	REOPEN: Pop the top request from both Stacks and insert it at the end of both Queues.
After completing all operations, we display the remaining Queue elements and Stack elements for both implementations. If an operation cannot be performed, we display an appropriate message.
## Algorithm
Step 1: Start the program.
Step 2: Take N and Q as input.
Step 3: Take N request IDs and insert them into both Array Queue and Linked Queue.
Step 4: Repeat the following operations Q times:
If the operation is ADD then insert the new ID into both Queues. If the operation is SERVE then check whether the Queue is empty. If not then remove the front element and push it into both Stacks. Otherwise, print "SERVE: Not Possible" in output. If the operation is REOPEN then check whether the Stack is empty. If not then pop the top element and insert it into both Queues. Otherwise, print "REOPEN: Not Possible" in the output.
Step 5: Display both Queues from front to rear.
Step 6: Display both Stacks from top to bottom.
Step 7: Stop the program.
## Implementation
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


int arrayQueue[100];
int aqFront=0,aqRear=-1;
void addArrayQueue(int x)
{
     if (aqRear==99)
    {
        printf("Array Queue Overflow\n");
        return;
    }
    arrayQueue[++aqRear]=x;
}
int serveArrayQueue()
{
    if (aqFront>aqRear)
    {
        printf("Array Queue Underflow\n");
        return -1;
    }
    return arrayQueue[aqFront++];
}


int arrayStack[100];
int asTop=-1;
void pushArrayStack(int x)
{
     if (asTop==99)
    {
        printf("Array Stack Overflow\n");
        return;
    }
    arrayStack[++asTop]=x;
}
int popArrayStack()
{
    if (asTop==-1)
    {
        printf("Array Stack Underflow\n");
        return -1;
    }
    return arrayStack[asTop--];
}


struct Node {
    int data;
    struct Node *next;
};
struct Node *lqFront=NULL;
struct Node *lqRear=NULL;
struct Node* createNode(int x)
{
    struct Node *newNode=malloc(sizeof(struct Node));
    newNode->data=x;
    newNode->next=NULL;
    return newNode;
}

void addLinkedQueue(int x) {
    struct Node *newNode=createNode(x);

    if (newNode==NULL)
    {
        printf("Linked Queue Overflow\n");
        return;
    }
    if (lqRear==NULL)
    {
        lqFront=lqRear=newNode;
    } else {
        lqRear->next=newNode;
        lqRear=newNode;
    }
}
int serveLinkedQueue() {
    if (lqFront == NULL)
    {
        printf("Linked Queue Underflow\n");
        return -1;
    }
    struct Node *temp=lqFront;
    int x=temp->data;
    lqFront=lqFront->next;
    if (lqFront==NULL)
    {
        lqRear=NULL;
    }
    free(temp);
    return x;
}


struct Node *lsTop=NULL;
void pushLinkedStack(int x)
{
    struct Node *newNode=createNode(x);
    newNode->next=lsTop;
    lsTop=newNode;
}
int popLinkedStack()
{
    if (lsTop == NULL)
    {
        printf("Linked Stack Underflow\n");
        return -1;
    }
    struct Node *temp = lsTop;
    int x = temp->data;
    lsTop = lsTop->next;
    free(temp);
    return x;
}


void displayArrayQueue()
{
    printf("Array Queue:");
    if (aqFront > aqRear){
        printf(" Empty");
    }
    else{
        for(int i=aqFront; i<=aqRear; i++){
            printf(" %d",arrayQueue[i]);
        }
    }
    printf("\n");
}
void displayArrayStack()
{
    printf("Array Stack:");
    if (asTop==-1){
        printf(" Empty");
    }
    else{
        for(int i=asTop; i>=0; i--){
            printf(" %d", arrayStack[i]);
        }
    }
    printf("\n");
}
void displayLinkedQueue()
{
    printf("Linked Queue:");
    if(lqFront==NULL){
        printf(" Empty");
    }
    else{
        struct Node *temp=lqFront;

        while (temp!=NULL){
            printf(" %d", temp->data);
            temp=temp->next;
        }
    }
    printf("\n");
}
void displayLinkedStack()
{
    printf("Linked Stack:");
    if (lsTop==NULL){
        printf(" Empty");
    }
    else{
        struct Node *temp=lsTop;
        while (temp!=NULL){
            printf(" %d", temp->data);
            temp=temp->next;
        }
    }
    printf("\n");
}


int main(){
    int N,Q,x;
    char operation[20];
    scanf("%d %d",&N,&Q);

    for (int i=0; i<N; i++)
    {
        scanf("%d",&x);
        addArrayQueue(x);
        addLinkedQueue(x);
    }
    int serveError = 0, reopenError = 0;
    for (int i=0; i<Q; i++)
    {
        scanf("%s",operation);
        if (strcmp(operation,"ADD")==0)
        {
            scanf("%d",&x);
            addArrayQueue(x);
            addLinkedQueue(x);
        }
        else if (strcmp(operation,"SERVE")==0) {
            if (aqFront>aqRear)
            {
                serveError++;
            }
            else{
                x=serveArrayQueue();
                serveLinkedQueue();
                pushArrayStack(x);
                pushLinkedStack(x);
            }
        }

        else if (strcmp(operation, "REOPEN") == 0) {
            if (asTop == -1)
            {
                reopenError++;
            }
            else {
                x = popArrayStack();
                popLinkedStack();
                addArrayQueue(x);
                addLinkedQueue(x);
            }
        }
    }

    for (int i = 0; i < serveError; i++)
    {
        printf("SERVE: Not Possible\n");
    }

    for (int i = 0; i < reopenError; i++)
    {
        printf("REOPEN: Not Possible\n");
    }

    displayArrayQueue();
    displayLinkedQueue();
    displayArrayStack();
    displayLinkedStack();

    return 0;
}
## Result
Case#1Input:
4 8
11 22 33 44
SERVE
ADD 55
SERVE
SERVE
REOPEN
ADD 66
SERVE
REOPEN
Case#1Output:
Array Queue: 55 33 66 44
Linked Queue: 55 33 66 44
Array Stack: 22 11
Linked Stack: 22 11
Case#2Input:
2 6
7 9
SERVE
SERVE
SERVE
REOPEN
REOPEN
REOPEN
Case#2Output:
SERVE: Not Possible
REOPEN: Not Possible
Array Queue: 9 7
Linked Queue: 9 7
Array Stack: Empty
Linked Stack: Empty
## Complexity Analysis
The ADD, SERVE and REOPEN operations take a constant amount of time because each operation requires only a few steps. Displaying the Queue and Stack takes more time because the program needs to visit every element. The space required depends on the number of elements stored in the Queue and Stack. Overall, both Array and Linked List implementations perform the main operations efficiently.
## Discussion
The program successfully performed all the required operations using both Arrays and Singly Linked Lists. Both implementations produced the same results for the Queue and Stack. The program also handled empty Queue and Stack conditions by showing the appropriate error messages without changing the records. This helped me understand how Stack and Queue work and also how they can be implemented using different methods.
## Conclusion
In conclusion, I successfully implemented Stack and Queue using Arrays and Singly Linked Lists in C. I learned how to perform ADD, SERVE and REOPEN operations. Also how to handle empty conditions. This helped me understand the working process of Stack and Queue. Also helped me to understand the difference in their implementation.
