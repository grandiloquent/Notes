# C Stacks and Queues

- [1. Implement a Stack as an Array](#1-implement-a-stack-as-an-array)
- [2. Implement a Stack as a Linked List](#2-implement-a-stack-as-a-linked-list)
- [3. Convert an Infix Expression to a Postfix Expression](#3-convert-an-infix-expression-to-a-postfix-expression)
- [4. Convert an Infix Expression to a Prefix Expression](#4-convert-an-infix-expression-to-a-prefix-expression)
- [5. Implement a Circular Queue as an Array](#5-implement-a-circular-queue-as-an-array)

## 1. Implement a Stack as an Array


```c

#include <stdio.h>
#include <stdlib.h>
#define STACKSIZE 8
int stack[STACKSIZE];
int intTop = 0;
int stackMenu(void);
void displayStack(void);
void popItem(void);
void pushItem(void);
void main() {
  int intChoice;
  do {
    intChoice = stackMenu();
    switch (intChoice) {
    case 1:
      pushItem();
      break;
    case 2:
      popItem();
      break;
    case 3:
      displayStack();
      break;
    case 4:
      exit(0);
    }
    fflush(stdin);
  } while (1);
}
int stackMenu() {
  int intChoice;
  printf("\n\n Enter 1 to Push an Element onto Stack. ");
  printf("\n Enter 2 to Pop an Element from Stack. ");
  printf("\n Enter 3 to Displays the Stack on the Screen.");
  printf("\n Enter 4 to Stop the Execution of Program.");
  printf("\n Enter your choice (0 <= N <= 4): ");
  scanf("%d", &intChoice);
  return intChoice;
}
void displayStack() {
  int j;
  if (intTop == 0) {
    printf("\n\nStack is Exhausted.");
    return;
  } else {
    printf("\n\nElements in stack:");
    for (j = intTop - 1; j > -1; j--)
      printf("\n%d", stack[j]);
  }
}
void popItem()
{
  if (intTop == 0) {
    printf("\n\nStack is Exhausted.");
    return;
  } else
    printf("\n\nPopped Element: %d ", stack[--intTop]);
}
void pushItem() {
  int intData;
  if (intTop == STACKSIZE) {
    printf("\n\nStack is Completely Filled.");
    return;
  } else {
    printf("\n\nEnter Element K (0 <= K <= 30000) : ");
    scanf("%d", &intData);
    stack[intTop] = intData;
    intTop = intTop + 1;
    printf("\n\nElement Pushed into the stack");
  }
}
```

## 2. Implement a Stack as a Linked List


```c

#include <stdio.h>
#include <stdlib.h>
struct intStack {
  int element;
  struct intStack *next;
};
typedef struct intStack node;
node *begin = NULL;
node *top = NULL;
node *getnode() {
  node *temporary;
  temporary = (node *)malloc(sizeof(node));
  printf("\nEnter Element (0 <= N <= 30000): ");
  scanf("%d", &temporary->element);
  temporary->next = NULL;
  return temporary;
}
void pushItem(node *newnode) {
  node *temporary;
  if (newnode == NULL) {
    printf("\nThe Stack is Completely Fillled");
    return;
  }
  if (begin == NULL) {
    begin = newnode;
    top = newnode;
  } else {
    temporary = begin;
    while (temporary->next != NULL)
      temporary = temporary->next;
    temporary->next = newnode;
    top = newnode;
  }
  printf("\nElement is pushed into the Stack");
}
void popItem() {
  node *temporary;
  if (top == NULL) {
    printf("\nStack is Exhausted");
    return;
  }
  temporary = begin;
  if (begin->next == NULL) {
    printf("\nPopped Element is: %d ", top->element);
    begin = NULL;
    free(top);
    top = NULL;
  } else {
    while (temporary->next != top) {
      temporary = temporary->next;
    }
    temporary->next = NULL;
    printf("\n Popped Element is: %d ", top->element);
    free(top);
    top = temporary;
  }
}
void displayStack() {
  node *temporary;
  if (top == NULL) {
    printf("\nStack is Exhausted ");
  }
  else {
    temporary = begin;
    printf("\nElements in the stack : ");
    printf("\nLeft-Most Element Represents Bottom  :  ");
    printf("Right-Most Element Represents Top \n\n");
    printf("%d", temporary->element);
    while (temporary != top) {
      temporary = temporary->next;
      printf("\t%d ", temporary->element);
    }
  }
}
int stackMenu() {
  int intChoice;
  printf("\n\nEnter 1 to Push an Element into Stack. ");
  printf("\nEnter 2 to Pop an Element from Stack. ");
  printf("\nEnter 3 to Displays the Stack on the Screen.");
  printf("\nEnter 4 to Stop the Execution of Program.");
  printf("\nEnter your choice (0 <= N <= 4): ");
  scanf("%d", &intChoice);
  return intChoice;
}
void main() {
  int intChoice;
  node *newnode;
  do {
    intChoice = stackMenu();
    switch (intChoice) {
    case 1:
      newnode = getnode();
      pushItem(newnode);
      break;
    case 2:
      popItem();
      break;
    case 3:
      displayStack();
      break;
    case 4:
      exit(0);
    }
    fflush(stdin);
  } while (1);
}
```

## 3. Convert an Infix Expression to a Postfix Expression


```c

#include <stdio.h>
#include <string.h>
char postfixExp[60];
char infixExp[60];
char operatorStack[60];
int i = 0, j = 0, intTop = 0;
int lowPriority(char opr, char oprStack) {
  int k, p1, p2;
  char oprList[] = {'+', '-', '*', '/', '%', '^', '('};
  int prioList[] = {0, 0, 1, 1, 2, 3, 4};
  if (oprStack == '(')
    return 0;
  for (k = 0; k < 6; k++) {
    if (opr == oprList[k])
      p1 = prioList[k];
  }
  for (k = 0; k < 6; k++) {
    if (oprStack == oprList[k])
      p2 = prioList[k];
  }
  if (p1 < p2)
    return 1;
  else
    return 0;
}
void pushOpr(char opr) {
  if (intTop == 0) {
    operatorStack[intTop] = opr;
    intTop++;
  } else {
    if (opr != '(') {
      while (lowPriority(opr, operatorStack[intTop - 1]) == 1 && intTop > 0) {
        postfixExp[j] = operatorStack[--intTop];
        j++;
      }
    }
    operatorStack[intTop] = opr;
    intTop++;
  }
}
void popOpr()
{
  while (operatorStack[--intTop] != '(') {
    postfixExp[j] = operatorStack[intTop];
    j++;
  }
}
void main() {
  char k;
  printf("\n Enter Infix Expression : ");
  gets(infixExp);
  while ((k = infixExp[i++]) != '\0') {
    switch (k) {
    case ' ':
      break;
    case '(':
    case '+':
    case '-':
    case '*':
    case '/':
    case '^':
    case '%':
      pushOpr(k);
      break;
    case ')':
      popOpr();
      break;
    default:
      postfixExp[j] = k;
      j++;
    }
  }
  while (intTop >= 0) {
    postfixExp[j] = operatorStack[--intTop];
    j++;
  }
  postfixExp[j] = '\0';
  printf("\n Infix Expression : %s ", infixExp);
  printf("\n Postfix Expression : %s ", postfixExp);
  printf("\n Thank you\n ");
}
```

## 4. Convert an Infix Expression to a Prefix Expression


```c

#include <stdio.h>
#include <string.h>
char prefixExp[60];
char infixExp[60];
char operatorStack[60];
int n = 0, intTop = 0;
void fillPre(char let) {
  int m;
  if (n == 0)
    prefixExp[0] = let;
  else {
    for (m = n + 1; m > 0; m--)
      prefixExp[m] = prefixExp[m - 1];
    prefixExp[0] = let;
  }
  n++;
}
int lowPriority(char opr, char oprStack) {
  int k, p1, p2;
  char oprList[] = {'+', '-', '*', '/', '%', '^', ')'};
  int prioList[] = {0, 0, 1, 1, 2, 3, 4};
  if (oprStack == ')')
    return 0;
  for (k = 0; k < 6; k++) {
    if (opr == oprList[k])
      p1 = prioList[k];
  }
  for (k = 0; k < 6; k++) {
    if (oprStack == oprList[k])
      p2 = prioList[k];
  }
  if (p1 < p2)
    return 1;
  else
    return 0;
}
void pushOpr(char opr) {
  if (intTop == 0) {
    operatorStack[intTop] = opr;
    intTop++;
  } else {
    if (opr != ')') {
      while (lowPriority(opr, operatorStack[intTop - 1]) == 1 && intTop > 0) {
        fillPre(operatorStack[--intTop]);
      }
    }
    operatorStack[intTop] = opr;
    intTop++;
  }
}
void popOpr() {
  while (operatorStack[--intTop] != ')')
    fillPre(operatorStack[intTop]);
}
void main() {
  char chrL;
  int length;
  printf("\n Enter Infix Expression : ");
  gets(infixExp);
  length = strlen(infixExp);
  while (length > 0) {
    chrL = infixExp[--length];
    switch (chrL) {
    case ' ':
      break;
    case ')':
    case '+':
    case '-':
    case '*':
    case '/':
    case '^':
    case '%':
      pushOpr(chrL);
      break;
    case '(':
      popOpr();
      break;
    default:
      fillPre(chrL);
    }
  }
  while (intTop > 0) {
    fillPre(operatorStack[--intTop]);
    n++;
  }
  prefixExp[n] = '\0';
  printf("\n Infix Expression : %s ", infixExp);
  printf("\n Prefix Expression : %s ", prefixExp);
  printf("\n Thank you\n");
}
```

## 5. Implement a Circular Queue as an Array


```c

#include <stdio.h>
#define SIZE 8
int circQue[SIZE];
int frontCell = 0;
int rearCell = 0;
int kount = 0;
void insertCircQue() {
  int num;
  if (kount == SIZE) {
    printf("\nCircular Queue is Full. Enter Any Choice Except 1.\n ");
  } else {
    printf("\nEnter data, i.e, a number N (0 <= N : 30000): ");
    scanf("%d", &num);
    circQue[rearCell] = num;
    rearCell = (rearCell + 1) % SIZE;
    kount++;
    printf("\nData Inserted in the Circular Queue. \n");
  }
}
void deleteCircQue() {
  if (kount == 0) {
    printf("\nCircular Queue is Exhausted!\n");
  } else {
    printf("\nElement Deleted from Cir Queue is %d \n", circQue[frontCell]);
    frontCell = (frontCell + 1) % SIZE;
    kount--;
  }
}
void displayCircQue() {
  int i, j;
  if (kount == 0) {
    printf("\nCircular Queue is Exhausted!\n ");
  } else {
    printf("\nElements in Circular Queue are given below: \n");
    j = kount;
    for (i = frontCell; j != 0; j--) {
      printf("%d   ", circQue[i]);
      i = (i + 1) % SIZE;
    }
    printf("\n");
  }
}
int displayMenu() {
  int choice;
  printf("\nEnter 1 to Insert Data.");
  printf("\nEnter 2 to Delete Data.");
  printf("\nEnter 3 to Display Data.");
  printf("\nEnter 4 to Quit the Program. ");
  printf("\nEnter Your Choice: ");
  scanf("%d", &choice);
  return choice;
}
void main() {
  int choice;
  do {
    choice = displayMenu();
    switch (choice) {
    case 1:
      insertCircQue();
      break;
    case 2:
      deleteCircQue();
      break;
    case 3:
      displayCircQue();
      break;
    case 4:
      exit(0);
    default:
      printf("\nInvalid Choice. Please enter again. \n ");
    }
  } while (1);
}
```


