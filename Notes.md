<link rel="stylesheet" href="notes.css">

# NOTES

## Contents

- [NOTES](#notes)
  - [Contents](#contents)
  - [UNIT - 1](#unit---1)
    - [Memory Allocation](#memory-allocation)
      - [Static Memory Allocation](#static-memory-allocation)
      - [Dynamic Memory Allocation](#dynamic-memory-allocation)
    - [Linked List](#linked-list)
      - [Singly Linked List](#singly-linked-list)
      - [Doubly Linked List](#doubly-linked-list)
      - [Circular Singly Linked List](#circular-singly-linked-list)
      - [Circular Doubly Linked List](#circular-doubly-linked-list)
      - [Multi List](#multi-list)
      - [Skip List](#skip-list)
    - [STACKS](#stacks)
      - [Array implementation](#array-implementation)
      - [LL implementation](#ll-implementation)
      - [Structure implementation](#structure-implementation)
    - [Application of Stack](#application-of-stack)
      - [Infix to postfix](#infix-to-postfix)
      - [Infix to prefix](#infix-to-prefix)
      - [Check for balanced parenthesis](#check-for-balanced-parenthesis)

---

## UNIT - 1

### Memory Allocation

#### Static Memory Allocation

- Exact size and type of memory must be known at compile time
- Exact size and type of memory must be known at compile time
- Memory is allocated in stack area

```c
int b;
int c[10] ;
```

- Disadvantages

  - Memory allocated can not be altered during run time as it
    is allocated during compile time
  - This may lead to under utilization or over utilization of
    memory
  - Memory can not be deleted explicitly only contents can
    be overwritten
  - Useful only when data size is fixed and known before
    processing

#### Dynamic Memory Allocation

- Dynamic memory allocation is used to obtain and
  release memory during program execution.
- It operates at a low-level
- Memory Management functions are used for allocating
  and deallocating memory during execution of program
- These functions are defined in “stdlib.h”
- Dynamic Memory Allocation Functions:
  - Allocate memory - malloc(), calloc(), and realloc()
  - Free memory - free()

```c
int *p;
if ((p = (int*)malloc(100 * sizeof(int))) == NULL){
  printf("out of memory\n");
  exit();
}
int *ptr;
ptr=(int*)calloc(100,sizeof(int));
```

### Linked List

#### Singly Linked List

```c
// Structure to represent a node
struct node {
    int data;
    struct node *link;
};
typedef struct node NODE;

// Structure to represent a linked list
struct list {
    NODE *head;
};
typedef struct list LIST;

```

![SLL](./images/SLL.png)

```c
// Initialize the list with an empty head
void init_list(LIST *ptr_list) {
    ptr_list->head = NULL;
}
```

- Insertion

```c
// Insert node at the head of the list
void insert_head(LIST *ptr_list, int data) {
    NODE *temp = (NODE *)malloc(sizeof(NODE));
    temp->data = data;
    temp->link = ptr_list->head;
    ptr_list->head = temp;
}

// Insert node at the end of the list
void insert_end(LIST *ptr_list, int data) {
    NODE *pres = ptr_list->head;
    NODE *temp = (NODE *)malloc(sizeof(NODE));
    temp->data = data;
    temp->link = NULL;

    if (pres == NULL) {
        ptr_list->head = temp;
    } else {
        while (pres->link != NULL)
            pres = pres->link;
        pres->link = temp;
    }
}

// Insert node at a specific position in the list
void insert_pos(LIST *ptr_list, int data, int pos) {
    NODE *pres = ptr_list->head, *temp;
    int i = 1;
    temp = (NODE *)malloc(sizeof(NODE));
    temp->data = data;
    temp->link = NULL;

    if (pos == 1) {
        temp->link = ptr_list->head;
        ptr_list->head = temp;
        return;
    }

    while (pres != NULL && i < pos - 1) {
        pres = pres->link;
        i++;
    }

    if (pres != NULL) {
        temp->link = pres->link;
        pres->link = temp;
    } else {
        printf("\nInvalid position.\n");
    }
}
```

- Display

```c
// Display the list
void display(LIST *ptr_list) {
    NODE *pres = ptr_list->head;
    if (pres == NULL) {
        printf("\nEmpty list.\n");
    } else {
        while (pres != NULL) {
            printf("%d --> ", pres->data);
            pres = pres->link;
        }
    }
}
```

- Deletion

```c
// Delete node by value
void delete_node(LIST *ptr_list, int data) {
    NODE *pres = ptr_list->head, *prev = NULL;

    while (pres != NULL && pres->data != data) {
        prev = pres;
        pres = pres->link;
    }

    if (pres != NULL) {
        if (prev == NULL) {
            ptr_list->head = pres->link;
        } else {
            prev->link = pres->link;
        }
        free(pres);
    } else {
        printf("Node not found..\n");
    }
}

// Delete node at a specific position
void delete_pos(LIST *ptr_list, int pos) {
    NODE *pres = ptr_list->head, *prev = NULL;
    int i = 1;

    while (pres != NULL && i < pos) {
        prev = pres;
        pres = pres->link;
        i++;
    }

    if (pres != NULL) {
        if (prev == NULL) {
            ptr_list->head = pres->link;
        } else {
            prev->link = pres->link;
        }
        free(pres);
    } else {
        printf("Invalid Position..\n");
    }
}

// Delete alternate nodes
void delete_alternate(LIST *ptr_list) {
    NODE *pres = ptr_list->head, *prev = NULL;

    while (pres != NULL) {
        if (prev == NULL) {
            ptr_list->head = pres->link;
        } else {
            prev->link = pres->link;
        }
        prev = pres->link;
        if (prev != NULL)
            pres = prev->link;
        else
            pres = NULL;
    }
}
```

#### Doubly Linked List

```c
struct node
{
    int key;       // Data (value) of the node
    struct node *next;  // Pointer to the next node
    struct node *prev;  // Pointer to the previous node
};

typedef struct node NODE;

struct dlist
{
    NODE *head;     // Pointer to the head node
                    // (first node of the list)
};

typedef struct dlist DLIST;
```

![DLL](./images/DLL.png)

```c
void init_list(DLIST *ptr_list)
{
    // Initialize the list with an empty head
    ptr_list->head = NULL;
}
```

- Insertion

```c
void insert_head(DLIST *ptr_list, int key)
{
    // Allocate memory for a new node
    NODE *temp = (NODE *)malloc(sizeof(NODE));
    temp->key = key;
    temp->next = temp->prev = NULL;

    if (ptr_list->head == NULL)
        // If the list is empty, make temp the head
        ptr_list->head = temp;
    else
    {
        temp->next = ptr_list->head;
         // Point new node to current head
        ptr_list->head->prev = temp;
        // Point current head's prev to new node
        ptr_list->head = temp;  // Update the head to the new node
    }
}
void insert_tail(DLIST *ptr_list, int key)
{
    NODE *temp, *pres;
    // Allocate memory for a new node
    temp = (NODE *)malloc(sizeof(NODE));
    temp->key = key;
    temp->next = temp->prev = NULL;

    if (ptr_list->head == NULL)
      // If the list is empty, make temp the head
      ptr_list->head = temp;
    else
    {
      pres = ptr_list->head;
      // Traverse to the last node
      while (pres->next != NULL)
          pres = pres->next;

      // Make the last node point to the new node
      pres->next = temp;
      // Make the new node's prev point to the last node
      temp->prev = pres;
    }
}
```

- Display

```c
void display(DLIST *ptr_list)
{
    NODE *pres = ptr_list->head;  // Start from the head

    if (pres == NULL)
        printf("\nEmpty List..\n");
    else
    {
        while (pres != NULL)
        {
            printf("%d<->", pres->key);  // Print the key
            pres = pres->next;  // Move to the next node
        }
    }
    printf("\n");
}
```

- Deletion

```c
void delete_first(DLIST *ptr_list)
{
  NODE *pres = ptr_list->head;

  if (pres->next == NULL)
    // Only one node, set head to NULL
    ptr_list->head = NULL;
  else
  {
    // Make the second node's prev NULL
    pres->next->prev = NULL;
    // Update the head to the second node
    ptr_list->head = pres->next;
  }
  free(pres);  // Free the memory of the deleted node
}
void delete_last(DLIST *ptr_list)
{
    NODE *pres = ptr_list->head;

    if (pres->next == NULL)  // Only one node
        ptr_list->head = NULL;
    else
    {
        while (pres->next != NULL)  // Traverse to the last node
            pres = pres->next;

        // Make the second-to-last node's next NULL
        pres->prev->next = NULL;
    }
    free(pres);  // Free the memory of the last node
}
```

> If You want **explaination** for these code maybe stop cs engg and become prompt engg and <span class="highlight">**ASK GPT**</span>

#### Circular Singly Linked List

```c
struct node
{
    int data;
    struct node *link;
};

typedef struct node NODE;

struct list
{
    NODE *last;
};

typedef struct list CLIST;
```

![CLL](./images/CSLL.png)

```c
void init_list(CLIST *ptr_list)
{
    ptr_list->last = NULL;
}
```

- Insertion

```c
void insert_front(CLIST *ptr_list, int data)
{
    NODE *temp, *p;

    // Create a new node
    temp = (NODE*)malloc(sizeof(NODE));
    temp->data = data;
    temp->link = temp;  // Links to itself (circular)

    p = ptr_list->last;  // Get the address of the last node

    if(p == NULL)  // First node (empty list)
        ptr_list->last = temp;
    else
    {
        temp->link = p->link;
        p->link = temp;
    }
}
void insert_end(CLIST *ptr_list, int data)
{
    NODE *temp, *p;

    // Create a new node
    temp = (NODE*)malloc(sizeof(NODE));
    temp->data = data;
    temp->link = temp;  // Links to itself (circular)

    p = ptr_list->last;  // Get the address of the last node

    if(p == NULL)  // Empty list
        ptr_list->last = temp;  // First node
    else
    {
        temp->link = p->link;  // Link after the last node
        p->link = temp;
        ptr_list->last = temp;  // Update the last pointer
    }
}
```

- Display

```c
void display(CLIST* ptr_list)
{
    NODE *pres, *end;

    if(ptr_list->last == NULL)
        printf("Empty List\n");
    else
    {
        end = ptr_list->last;  // Copy last node's address
        pres = end->link;  // Point to the first node

        while(pres != end)
        {
            printf("%d ->", pres->data);
            pres = pres->link;
        }
        printf("%d ", pres->data);  // Print the last node
    }
}
```

- Deletion

```c
void delete_node(CLIST* ptr_list, int data)
{
    NODE *end, *pres, *prev;
    end = ptr_list->last;  // Copy address of the last node
    pres = end->link;  // Point to the first node
    prev = end;

    // Find the node with the given data
    while((pres->data != data) && (pres != end))
    {
        prev = pres;
        pres = pres->link;
    }

    if(pres->data == data)
    {
        // If only one node
        if(pres->link == pres)
            ptr_list->last = NULL;
        else
        {
            // Link the previous node to the next node
            prev->link = pres->link;
            if(pres == end)  // If deleting the last node
                ptr_list->last = prev;  // Update the last pointer
        }
        free(pres);
    }
    else
        printf("Data not found..\n");
}
```

#### Circular Doubly Linked List

```c
struct node {
    int data;
    struct node *next;
    struct node *prev;
};

typedef struct node NODE;

struct list {
    NODE *head;
};
```

![CDLL](./images/CDLL.png)

```c
void init_list(CDLL *ptr_list) {
    ptr_list->head = NULL;
}
```

- Insertion

```c
void insert_front(CDLL *ptr_list, int data) {
    NODE *temp;

    // Create new node
    temp = (NODE *)malloc(sizeof(NODE));
    temp->data = data;

    if (ptr_list->head == NULL) {  // If the list is empty
        temp->next = temp;  // Points to itself
        temp->prev = temp;  // Points to itself
        ptr_list->head = temp;  // Head points to the new node
    } else {
        // Next node points to current head
        temp->next = ptr_list->head;
        // Previous node points to current last
        temp->prev = ptr_list->head->prev;
        // Last node's next points to the new node
        ptr_list->head->prev->next = temp;
        // Head's previous points to the new node
        ptr_list->head->prev = temp;
        // Make new node as head
        ptr_list->head = temp;
    }
}
void insert_end(CDLL *ptr_list, int data) {
    NODE *temp;

    // Create new node
    temp = (NODE *)malloc(sizeof(NODE));
    temp->data = data;

    if (ptr_list->head == NULL) {  // If the list is empty
        temp->next = temp;  // Points to itself
        temp->prev = temp;  // Points to itself
        ptr_list->head = temp;  // Head points to the new node
    } else {
        // Next points to the head
        temp->next = ptr_list->head;
        // Previous points to the current last node
        temp->prev = ptr_list->head->prev;
        // Last node's next points to the new node
        ptr_list->head->prev->next = temp;
        // Head's previous points to the new node
        ptr_list->head->prev = temp;
    }
}
```

- Display

```c
void display(CDLL *ptr_list) {
    NODE *pres;

    if (ptr_list->head == NULL) {
        printf("Empty List\n");
    } else {
        pres = ptr_list->head;

        // Traverse the list in forward direction
        do {
            printf("%d <-> ", pres->data);
            pres = pres->next;
        } while (pres != ptr_list->head);
        // Stop when we circle back to head

        printf("(Head)\n");
    }
}
```

- Deletion

```c
void delete_node(CDLL *ptr_list, int data) {
    NODE *temp, *pres;

    if (ptr_list->head == NULL) {
        printf("List is empty\n");
        return;
    }

    pres = ptr_list->head;

    // Traverse the list to find the node
    do {
        if (pres->data == data) {
            temp = pres;

            if (pres->next == pres) {  // If there's only one node
              ptr_list->head = NULL;  // List becomes empty
            } else {
              // Previous node's next points to the next node
              pres->prev->next = pres->next;
              // Next node's previous points to the previous node
              pres->next->prev = pres->prev;
              if (pres == ptr_list->head) {
                // If head is to be deleted, update head
                 ptr_list->head = pres->next;
              }
            }

            free(temp);
            printf("Node with data %d deleted\n", data);
            return;
        }
        pres = pres->next;
    } while (pres != ptr_list->head);
    // Stop when we circle back to head
    printf("Data not found\n");
}
```

#### Multi List

- OK So multi list(Sparse list) is basically a 2D List of a matrix
- The main point being we just dont include 0s' in the list at all
- There are two Nodes for this list
  ![Nodes](./images/NodesML.png)

  $$
  \begin{bmatrix}
  2 & 0 & 0 &0 \\
  4 & 0 & 0 & 3 \\
  0 & 0 & 0 & 0 \\
  8 & 0 & 0 & 1 \\
  0 & 0 & 6 & 0
  \end{bmatrix}
  $$

  ![Sparce Matrix](./images/SMML.png)

- You can imagine what functions we have with these , no implementation so less work for me

#### Skip List

- This is basically a list where we keep multiple copies with some missing elements who are attached to each other

![Skip List](./images/SkpL.png)

- Skip Lists support O(log n)

  - Insertion
  - Deletion
  - Search

- Ok SO lets say we wanna go to 60 how do we go
- Some basic rule would look like
  - x = y: we return element(after(p))
  - x > y: we “scan forward”
  - x < y: we “drop down”
    ![Traversal](./images/SkpLTrverse60.png)
- For Insertion We use a random function to tell us inside how many rows should the element be in
- For deletion we delete all the appearing node of that data and if there are more than one empty row then we delete the extras

### STACKS

- Definition: A stack is a linear data structure that follows the Last In, First Out (LIFO) principle, meaning the last element added is the first to be removed.

- Basic Operations:
  - Push: Adds an element to the top of the stack.
  - Pop: Removes the top element from the stack.
  - isEmpty: Checks if the stack is empty.

![Stack](./images/STACK.png)

- Implementation:

  - Can be implemented using arrays or linked lists.
  - In an array-based stack, a fixed size is predefined.
  - In a linked-list-based stack, dynamic memory allocation is used.

- Applications:

  - Expression evaluation: Infix to postfix conversion, postfix evaluation.
  - Backtracking: Undo operations in text editors, solving mazes.
  - Function call management: Recursion uses stack memory.
  - Browser navigation: Forward and backward navigation.

- Complexity:

  - Push, Pop, and Peek operations are performed in O(1) time.

- Limitations:

  - Fixed size in array-based stacks can lead to stack overflow.
  - Linked-list-based stacks require additional memory for pointers.

#### Array implementation

> There is no need of structure in array implementation COS we use a <span class="highlight">**FUCKING ARRAY**</span>

- Push Function

```c
int push(int *s, int *top, int size, int ele)
{
    if (*top == size - 1) // Check for stack overflow
    {
        printf("STACK IS OVERFLOWING\n");
        return 0;
    }
    *top = *top + 1; // Increment top
    s[*top] = ele;   // Insert element at the new top position
    return 1;        // Return success
}
```

- Pop Function

```c
int pop(int *s, int *top, int size)
{
    if (*top == -1) // Check for stack underflow
    {
        printf("Stack is underflow\n");
        return 0;
    }
    int ele = s[*top]; // Store the top element to return later
    *top = *top - 1;   // Decrement top
    return ele;        // Return the popped element
}
```

- Display Function

```c
void display(int *s, int top, int size)
{
    if (top == -1) // Check if the stack is empty
    {
        printf("Stack is empty\n");
    }
    else
    {
        printf("Stack elements are:\n");
        // Traverse from top to bottom
        for (int i = top; i >= 0; i--)
        {
            printf("%d ", s[i]);
        }
        printf("\n");
    }
}
```

> If Any of these Functions Confuse you <span class="highlight">_YOU ARE COOKED_</span>

#### LL implementation

- FOR Linked list implementation guess what we use <span class=highlight>FUCKING LINKED LIST</span>
<figure>
<figcaption>
IKR FUCKING MIND BOGGLING
</figcaption>
</figure>

```c
// Node structure for each stack element
typedef struct node
{
    int data;           // Data part of the node
    struct node *link;  // Pointer to the next node
} NODE;

// Stack structure with a pointer to the top node
typedef struct stack
{
    NODE *top;          // Pointer to the top of the stack
} STACK;
```

Anyways here's the code for the functions,you might but `WIZ` Where is the main function , To that i say **_fuck you_** make your own

- INIT

```c
// Initialize the stack by setting the top to NULL
void init(STACK *ptr)
{
    ptr->top = NULL;
}
```

> If you are wondering why do all this ,why extra init function?<br/>
> WELL <br/>

- What if you have more than one stack or you don'y know how many you have yet
  <br/>- Thats why we do this

- PUSH

```c
// Push an element onto the stack
void push(STACK *ptr, int ele)
{
    // Create and populate a new node
    NODE *temp = (NODE *)malloc(sizeof(NODE));
    temp->data = ele;   // Set the data part
    temp->link = NULL;  // Initialize the link to NULL

    // Insert the new node at the top of the stack
    temp->link = ptr->top; // Point new node to the current top
    ptr->top = temp;       // Update top to the new node
}
```

- POP

```c
// Pop the top element from the stack
int pop(STACK *ptr)
{
    NODE *p;
    int data;

    p = ptr->top;       // Point to the current top
    if (p == NULL)      // Check for empty stack
    {
        printf("Empty stack\n");
        return 0;
    }

    // Retrieve and remove the top element
    data = p->data;         // Get the data from the top node
    ptr->top = p->link;     // Update top to the next node
    free(p);                // Free the memory of the removed node
    return data;            // Return the popped data
}
```

- DISPLAY

```c
// Display all elements in the stack
void display(STACK *ptr)
{
    NODE *p = ptr->top;  // Start from the top of the stack

    if (p == NULL)       // Check if the stack is empty
    {
        printf("Empty stack\n");
    }
    else
    {
        // Traverse the stack and print elements
        while (p != NULL)
        {
            // Print current node's data
            printf("%d --> ", p->data);
            p = p->link;    // Move to the next node
        }
        printf("NULL\n");              // End of stack
    }
}
```

```bash
Output :
10 --> 11 --> NULL
```

#### Structure implementation

- OK , Remember how we made array implementation
- But what if we need more than one and can't hardcode the number of stacks
- That's when we use Structure implementation

```c
// Define a structure for stack
typedef struct stack
{
  int *s;     // Pointer to the array holding stack elements
  int top;    // Index of the top element
  int size;   // Maximum size of the stack
} STACK;
```

- INIT

```c
void init(STACK *stk, int size)
{
    stk->size = size;                 // Set the size of the stack
    // Allocate memory array
    stk->s = (int *)malloc(size * sizeof(int));
    if (stk->s == NULL)
    {
        printf("Memory allocation failed!\n");
        exit(1); // Exit if allocation fails
    }
    stk->top = -1; // Initialize the stack as empty
}
```

- IS EMPTY

```c
int isEmpty(STACK *stk) {//returns true or false
    return stk->top == -1;
}
```

- Push

```c
// Push an element onto the stack
int push(STACK *p, int x)
{
  // Check for stack overflow
  if (p->top == p->size - 1)
  {
    printf("Stack overflow..\n");
    return 0;
  }

  // Increment the top and add the new element
  p->top++;            // Move the top pointer
  p->s[p->top] = x;    // Store the element at the top
  return 1;            // Indicate successful operation
}
```

- Pop

```c
// Pop the top element from the stack
int pop(STACK *p)
{
  int x;

  // Check for stack underflow
  if (p->top == -1)
  {
    printf("Stack underflow..\n");
    return 0;
  }

  // Retrieve and remove the top element
  x = p->s[p->top];    // Get the element at the top
  p->top--;            // Move the top pointer down
  return x;            // Return the popped element
}
```

- Display

```c
// Display all elements in the stack
void display(STACK p)
{
  int i;

  // Check if the stack is empty
  if (p.top == -1)
    printf("\nStack empty..\n");
  else
  {
    // Traverse and print all elements from top to bottom
    for (i = p.top; i >= 0; i--)
      printf("%d  ", p.s[i]);
    printf("\n");
  }
}
```

### Application of Stack

- Expressions consist of operands and operators. They can be categorized into three types:

  - Infix Notation
  <pre>
      Format: <op> <opr> <op>
      Example: A + B
  </pre>
  - Prefix Notation
    <pre>
      Format: <opr> <op> <op>
      Example: +AB
  </pre>
  - Postfix Notation
    <pre>
        Format: <op> <op> <opr>
        Example: AB+
    </pre>

#### Infix to postfix

- ALGORITHM FOR INFIX to POSTFIX:
  - Input is the token of the infix expression - char array
    1.  Create an empty stack called opstack for keeping operators.
    2.  Create an empty array for the output.
    3.  Scan the input array from L-R
        - If the token is an operand, add it to the output array.
        - If the token is a left paranthesis (, push it on to the stack.
        - IF the token is a right paranthesis ) , pop the stack until the
          corresponding left paranthesis is found. Add all the operators to the
          end of the output array.
        - If the token is an operator`*,/,+,(,-` push it on to stack.
          However, first remove any operator already on the stack that has higher
          or equal precedence and add them to the output array.
    4.  When the input is completely processed, check the stack for any leftover
        operators. Pop them to the end of the output array.
- Here the code for implementation

```c
int main()
{
    char infix[10], postfix[10];
    printf("\nEnter valid Infix Expression\n");
    scanf("%s", infix);
    convert_postfix(infix, postfix);
    printf("\nThe postfix equivalent=%s\n", postfix);
}
```

- Stack precendence

```c
int stack_prec(char ch)
{
    switch (ch)
    {
        case '+':
        case '-':
            // Lower precedence for addition and subtraction
            return 2;
        case '*':
        case '/':
            // Higher precedence for multiplication and division
            return 4;
        case '(':
            // Opening parenthesis has the lowest precedence
            return 0;
        case '#':
            // Marker for the bottom of the stack
            return -1;
        default:
            // Default for operands and unknown characters
            return 6;
    }
}
```

- Input Precendence

```c
int input_prec(char ch)
{
    switch (ch)
    {
        case '+':
        case '-':
            // Lowest precedence for addition and subtraction
            return 1;
        case '*':
        case '/':
            // Higher precedence for multiplication and division
            return 3;
        case '(':
            // Highest precedence for opening parenthesis
            return 7;
        case ')':
            // Low precedence for closing parenthesis
            return 0;
        default:
            // Default for operands (numbers, variables)
            return 5;
    }
}
```

- Conversion code

```c
void convert_postfix(char *infix, char *postfix)
{
    int i, j;
    char ch;
    char s[10]; // stack
    int top = -1;
    i = 0;
    j = 0;
    push(s, &top, '#'); // push # to stack as initial marker

    while (infix[i] != '\0')
    {
        ch = infix[i];

        /* Pop stack until a lower precedence operator
        or open parenthesis is found */
        while (stack_prec(s[top]) > input_prec(ch))
            postfix[j++] = pop(s, &top);

        /* Push the current operator
        if it has higher precedence than the stack top */
        if (input_prec(ch) > stack_prec(s[top]))
            push(s, &top, ch);
        else
            pop(s, &top); // Pop the stack for parenthesis

        i++;
    }

    // Pop remaining operators in the stack to the output
    while (s[top] != '#')
        postfix[j++] = pop(s, &top);

    postfix[j] = '\0'; // Null-terminate the postfix string
}
```

- ALOGORITHM FOR EVALUATING A POSTFIX EXPRESSION:

  1. Create a stack called input.
  2. Scan the input from 0 to len-1: L - R
  3. if token is an operand.
     push it on to the input stack.
  4. Otherwise, if the token is an operator
     pop the first two elements on the stack and perform the operation
     based on the operator.
     push the result back onto the stack.
  5. return the top most element -> result of evaluating the postfix exp.

```c
int main() {
    char postfix[100];
    printf("\nEnter the postfix expression\n");
    scanf("%s", postfix);

    int result = postfix_eval(postfix);
    printf("\nThe result = %d\n", result);

    return 0;
}
```

```c
int isoper(char ch) {
    //checks if it is a operator and returns true or false
    if ((ch == '+') || (ch == '-') || (ch == '*') || (ch == '/'))
        return 1;
    return 0;
}
```

- Eval Function

```c
int postfix_eval(char* postfix) {
  int isoper(char);
  int i = 0, result, op1, op2, a;
  char ch;
  stack st; // Initialize a stack
  init_stk(&st);
  // Process each character in the postfix expression
  while (postfix[i] != '\0') {
     ch = postfix[i];
       if (isoper(ch))
      {                 // If the character is an operator
         op1 = pop(&st); // Pop two operands
         op2 = pop(&st);
            // Perform the operation and push the result back
        switch (ch) {
           case '+': result = op2 + op1; push(&st, result); break;
           case '-': result = op2 - op1; push(&st, result); break;
           case '*': result = op2 * op1; push(&st, result); break;
           case '/': result = op2 / op1; push(&st, result); break;
        }
      }
      else { // If the character is an operand
          printf("%c = ", ch);
          scanf("%d", &a); // Read the value of the operand
          push(&st, a);
      }
      i++;
  }
  return pop(&st); // Return the final result
}
```

#### Infix to prefix

- Algorithm: Infix to Prefix Conversion
  - Step 1: Input the Infix Expression
    - Read the infix expression (e.g., `A+B\*(C-D)`).
  - Step 2: Reverse the Infix Expression
    - Reverse the input expression and swap parentheses:
      - Replace `(` with `)` and vice versa.
      - Example: `A+B*(C-D)` → Reversed → `(D-C)*B+A`.
  - Step 3: Convert Reversed Infix to Postfix
    - Details of Infix to Postfix Conversion
      - Initialize a stack:
        - Push a sentinel value `#` onto the stack to signify the bottom of the stack.
      - Scan the reversed infix expression from left to right:
        - For each operand `A, B, C, ...`:
          - Append it to the postfix expression.
        - For each operator `+, -, *, /, ...`:
          - Pop operators from the stack to the postfix expression while the operator's precedence on the stack is greater or equal to the incoming operator's precedence.
          - Push the incoming operator onto the stack.
        - For parentheses:
          - Push `(` onto the stack when encountered.
          - Pop operators from the stack to the postfix expression until `)` is found, then discard the `)`.
        - Pop remaining operators from the stack:
          - Append them to the postfix expression until the sentinel `#` is reached.
        - Result:
          - The output is the postfix equivalent of the reversed infix expression.
  - Step 4: Reverse the Postfix Expression
    - Reverse the postfix expression to get the prefix equivalent.
      - Example: Postfix → `DC-B*A+` → Reversed → `+A*B-CD`.
  - Step 5: Output the Prefix Expression
    - Display the resulting prefix expression.
- Example Walkthrough

  - Input:
    - Infix Expression: `A+B*(C-D)`

---

- Reverse and Adjust Parentheses:

  1. Infix: `A+B*(C-D)`
  2. Reversed: `(D-C)*B+A`

- Convert to Postfix:

  1. Reversed Infix: `(D-C)*B+A`

  - Postfix Conversion:
    - Scan `(` → Push onto stack.
    - Scan `D` → Append to postfix: `D`.
    - Scan `-` → Push onto stack.
    - Scan `C` → Append to postfix: `D C`.
    - Scan `)` → Pop operators to postfix until `(` is encountered: `D C -`.
    - Scan `*` → Push onto stack.
    - Scan `B` → Append to postfix: `D C - B`.
    - Scan `+` → Push onto stack (lower precedence than `*`).
    - Scan `A` → Append to postfix: `D C - B * A`.
    - Scan `\0`→ Pop The rest of the Stack
  - Resulting Postfix:` D C - B * A +`

- Reverse the Postfix:

  - Postfix: `D C - B * A +`
  - Reversed: `+ A * B - C D`

- Output the Prefix Expression:
  - Prefix: `+A\*B-CD`.

```c
void reverse_string(char *a, char *b) {
    int i = strlen(a) - 1, j = 0;

    // Loop through the string backward
    while (i >= 0) {
        if (a[i] == '(')
            b[j++] = ')';
        else if (a[i] == ')')
            b[j++] = '(';
        else
            b[j++] = a[i]; // Copy other characters
        i--;
    }

    // Null-terminate the reversed string
    b[j] = '\0';
}
```

```c
int main()
{
  char infix[100],prefix[100], reverse[100],postfix[100];

  // Get infix expression input from the user
  printf("\nEnter valid Infix Expression\n");
  scanf("%s",infix);

  /* Reverse the infix expression and
  store it in the 'reverse' array     */
  reverse_string(infix, reverse);
  printf("Reversed = %s\n", reverse);

  // Convert the reversed infix expression to a postfix expression
  convert_postfix(reverse, postfix);
  printf("\npostfix equivalent = %s",postfix);

  // Reverse the postfix expression to get the prefix expression
  reverse_string(postfix, prefix);
  printf("\nThe prefix equivalent=%s\n",prefix);
}
```

- Precedence Function

```c
int input_prec(char ch)
{
    switch(ch)
    {
        case '+':
        case '-': return 2;
        // Lower precedence for '+' and '-' compared to postfix
        case '*':
        case '/': return 4;
        // Higher precedence for '*' and '/' compared to postfix
        case '(': return 7;
        // '(' has the highest precedence when it is the input
        case ')': return 0;
        // ')' has the lowest precedence when it is the input
        default: return 5;
        // For operands (like variables), precedence is constant
    }
}
/* Function to return the stack precedence of the operator */
int stack_prec(char ch)
{
    switch(ch)
    {
        case '+':
        case '-': return 1;
        // Stack precedence is lower for '+' and '-'
        case '*':
        case '/': return 3;
        // Stack precedence is lower for '*' and '/'
        case '(': return 0;
        // '(' has the lowest precedence when it is on the stack
        case '#': return -1;
        // Sentinel value '#' has the lowest precedence
        default: return 6;
        // For operands (like variables), precedence is constant
    }
}
```

- Convert To Postfix

```c
/* Function to convert infix to postfix expression */
void convert_postfix(char *infix, char *postfix)
{
    char s[100]; // Stack to hold operators and parentheses
    int top = -1, i, j;
    i = 0;
    char ch;
    j = 0;

    // Push '#' onto the stack as a sentinel value
    push(s, &top, '#');

    // Loop through the infix expression from left to right
    while(infix[i] != '\0')
    {
        ch = infix[i];

        /* Pop from the stack
        if input precedence is lower than stack precedence*/
        while(input_prec(ch) < stack_prec(peep(s, top)))
            postfix[j++] = pop(s, &top);

        /* Push to the stack
        if input precedence is higher than stack precedence*/
        if(input_prec(ch) > stack_prec(peep(s, top)))
            push(s, &top, ch);
        else
// If the precedence is equal, this happens when ')' matches '('
            pop(s, &top);
        i++;
    }

    /* Pop remaining operators from the stack
     and store in postfix expression*/
    while(peep(s, top) != '#')
        postfix[j++] = pop(s, &top);

    // Null-terminate the postfix string
    postfix[j] = '\0';
}
```

#### Check for balanced parenthesis

- Some helpers:

```c
// Function to return the top element of the stack
char topmost(stack* S) {
    if (!isEmpty(S)) {
        return S->items[S->top];
    }
    return '\0';
}
/* Function to check whether two characters
are opening and closing of the same type    */
int arePair(char opening, char closing) {
    if (opening == '(' && closing == ')') return 1;
    else if (opening == '{' && closing == '}') return 1;
    else if (opening == '[' && closing == ']') return 1;
    return 0;
}
```

- Checker

```c
// Function to check whether the parentheses are balanced
int check_balance(char* exp) {
    stack S;
    initStack(&S);

    for (int i = 0; i < strlen(exp); i++) {
        // Push opening brackets onto the stack
        if (exp[i] == '(' || exp[i] == '{' || exp[i] == '[') {
            push(&S, exp[i]);
        }
        /* For closing brackets,
        check if they match the top of the stack*/
        else if (exp[i] == ')' || exp[i] == '}' || exp[i] == ']')
        {
            if (isEmpty(&S) || !arePair(topmost(&S), exp[i])) {
                return 0; // Not balanced
            } else {
                pop(&S); // Match found, pop the stack
            }
        }
    }

    // If stack is empty, all parentheses were matched
    return isEmpty(&S);
}
```
