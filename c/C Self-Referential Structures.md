


# C Self-Referential Structures

- [1. Generate Lists of Numbers in an Interactive Manner](#1-generate-lists-of-numbers-in-an-interactive-manner)
- [2. Create a Linked List Using Anonymous Variables](#2-create-a-linked-list-using-anonymous-variables)
- [3. Delete a Component from a Linked List](#3-delete-a-component-from-a-linked-list)
- [4. Insert a Component into a Linked List](#4-insert-a-component-into-a-linked-list)
- [5. Create a Linked List in an Interactive Session](#5-create-a-linked-list-in-an-interactive-session)
- [6. Process a Linear Linked List](#6-process-a-linear-linked-list)
- [7. Create a Linear Linked List with Forward and Backward Traversing](#7-create-a-linear-linked-list-with-forward-and-backward-traversing)

## 1. Generate Lists of Numbers in an Interactive Manner


```c

#include <stdio.h>
main() {
  int n, i, j;
  int *ptr, *list[10];
  printf("Enter an integer as size of list (1 <= n <= 20): ");
  scanf("%d", &n);
  for (i = 0; i < 10; i++) {
    list[i] = (int *)calloc(n, sizeof(int));
    for (j = 0; j < n; j++)
      *(list[i] + j) = i + j + 10;
  }
  printf("Displaying the values of items in list\n");
  for (i = 0; i < 10; i++) {
    printf("List[%d]: ", i);
    for (j = 0; j < n; j++) {
      printf("%d ", *(list[i] + j));
    }
    printf("\n");
  }
  return (0);
}
```

## 2. Create a Linked List Using Anonymous Variables


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
struct members {
  char name[20];
  struct members *next;
};
typedef struct members node;
void display(node *start);
main() {
  node *start;
  start = (node *)malloc(sizeof(node));
  strcpy(start->name, "lina");
  start->next = (node *)malloc(sizeof(node));
  strcpy(start->next->name, "mina");
  start->next->next = (node *)malloc(sizeof(node));
  strcpy(start->next->next->name, "bina");
  start->next->next->next = (node *)malloc(sizeof(node));
  strcpy(start->next->next->next->name, "tina");
  start->next->next->next->next = NULL;
  printf("Names of all the members:\n");
  display(start);
  return (0);
}
void display(node *start) {
  int flag = 1;
  do {
    printf("%s\n", start->name);
    if (start->next == NULL)
      flag = 0;
    start = start->next;
  } while (flag);
  return;
}
```

## 3. Delete a Component from a Linked List


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
struct members {
  char name[20];
  struct members *next;
};
typedef struct members node;
void display(node *start);
main() {
  node *start, *temp = NULL;
  start = (node *)malloc(sizeof(node));
  strcpy(start->name, "lina");
  start->next = (node *)malloc(sizeof(node));
  strcpy(start->next->name, "mina");
  start->next->next = (node *)malloc(sizeof(node));
  strcpy(start->next->next->name, "bina");
  start->next->next->next = (node *)malloc(sizeof(node));
  strcpy(start->next->next->next->name, "tina");
  start->next->next->next->next = NULL;
  printf("Names of all the members:\n");
  display(start);
  printf("\nDeleting first component - lina\n");
  temp = start->next;
  free(start);
  start = temp;
  temp = NULL;
  display(start);
  printf("\nDeleting non-first component - bina\n");
  temp = start->next->next;
  free(start->next);
  start->next = temp;
  temp = NULL;
  display(start);
  return (0);
}
void display(node *start) {
  int flag = 1;
  do {
    printf("%s\n", start->name);
    if (start->next == NULL)
      flag = 0;
    start = start->next;
  } while (flag);
  return;
}
```

## 4. Insert a Component into a Linked List


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct members {
  char name[20];
  struct members *next;
};

typedef struct members node;

void display(node *start);
void display(node *start)
{
  int flag = 1;

  do {
    printf("%s\n", start->name);
    if (start->next == NULL)
      flag = 0;
    start = start->next;
  } while (flag);

  return;
}
main()
{
  node *start, *temp;

  start = (node *)malloc(sizeof(node));
  strcpy(start->name, "lina");
  start->next = (node *)malloc(sizeof(node));
  strcpy(start->next->name, "mina");
  start->next->next = (node *)malloc(sizeof(node));
  strcpy(start->next->next->name, "bina");
  start->next->next->next = NULL;

  printf("Names of all the members:\n");
  display(start);

  printf("\nInserting sita at first position\n");
  temp = (node *)malloc(sizeof(node));
  strcpy(temp->name, "sita");
  temp->next = start;
  start = temp;
  display(start);

  printf("\nInserting tina between lina and mina\n");
  temp = (node *)malloc(sizeof(node));
  strcpy(temp->name, "tina");
  temp->next = start->next->next;
  start->next->next = temp;
  display(start);

  return (0);
}
```

## 5. Create a Linked List in an Interactive Session


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
struct members {
  char name[20];
  struct members *next;
};
typedef struct members node;
void display(node *start);
void create(node *start);
main() {
  node *start, *temp;
  start = (node *)malloc(sizeof(node));
  temp = start;
  create(start);
  start = temp;
  printf("\nNames of all the members:\n");
  display(start);
  return (0);
}
void display(node *start) {
  int flag = 1;
  do {
    printf("%s\n", start->name);
    if (start->next == NULL)
      flag = 0;
    start = start->next;
  } while (flag);
  return;
}
void create(node *start) {
  int flag = 1;
  char ch;
  printf("Enter name: ");
  do {
    scanf(" %[^\n]", start->name);
    printf("Any more name? (y/n): ");
    scanf(" %c", &ch);
    if (ch == 'n') {
      flag = 0;
      start->next = NULL;
    } else {
      start->next = (node *)malloc(sizeof(node));
      start = start->next;
      printf("Enter name: ");
    }
  } while (flag);
  return;
}
```

## 6. Process a Linear Linked List


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct members {
  char name[20];
  struct members *next;
};

typedef struct members node;

int menu(void);
void create(node *start);
void display(node *start);
node *insert(node *start);
node *delete (node *start);
node *location(node *start, char target[]);

main()
{
  node *start = NULL, *temp;
  int selection;

  do {
    selection = menu();
    switch (selection) {

    case 1:
      start = (node *)malloc(sizeof(node));
      temp = start;
      create(start);
      start = temp;
      display(start);
      continue;

    case 2:
      if (start == NULL) {
        printf("\nList is empty! Select the option 1.\n");
        continue;
      }
      start = insert(start);
      display(start);
      continue;

    case 3:
      if (start == NULL) {
        printf("\nList is empty! Select the option 1.\n");
        continue;
      }
      start = delete (start);
      display(start);
      continue;

    default:
      printf("\nEnd of session.\n");
    }
  } while (selection != 4);

  return (0);
}
int menu(void)
{
  int selection;
  do {
    printf("\nSelect the desired operation:\n");
    printf("Enter 1 to create a new linked list\n");
    printf("Enter 2 to insert a component in the list\n");
    printf("Enter 3 to delete a component from the list\n");
    printf("Enter 4 to end the session.\n");
    printf("\nNow enter a number(1, 2, 3, or 4): ");
    scanf("%d", &selection);
    if ((selection < 1) || (selection > 4))
      printf("Invalid Number! Please try again.\n");
  } while ((selection < 1) || (selection > 4));
  return (selection);
}

void create(node *start)
{
  int flag = 1;
  char ch;
  printf("Enter name: ");

  do {
    scanf(" %[^\n]", start->name);
    printf("Any more name?(y/n): ");
    scanf(" %c", &ch);
    if (ch == 'n') {
      flag = 0;
      start->next = NULL;
    }
    else {
      start->next = (node *)malloc(sizeof(node));
      start = start->next;
      printf("Enter name: ");
    }
  } while (flag);

  return;
}

void display(node *start)
{
  int flag = 1;
  if (start == NULL) {
    printf("\nList is empty! Select the option 1.\n");
    return;
  }
  printf("\nNames of all the members in the list:\n");

  do {
    printf("%s\n", start->name);
    if (start->next == NULL)
      flag = 0;
    start = start->next;
  } while (flag);

  return;
}

node *insert(node *start)
{
  int flag = 1;
  node *new, *before, *tmp;
  char newName[20];
  char target[20];

  printf("Enter name to be inserted: ");
  scanf(" %[^\n]", newName);
  printf("Before which name to place? Type \"last\" if last: ");
  scanf(" %[^\n]", target);

  if (strcmp(target, "last") == 0) {
    tmp = start;

    do {
      start = start->next;
      if (start->next == NULL) {
        new = (node *)malloc(sizeof(node));
        strcpy(new->name, newName);
        start->next = new;
        new->next = NULL;
        flag = 0;
      }
    } while (flag);

    start = tmp;
    return (start);
  }

  if (strcmp(start->name, target) == 0) {
    new = (node *)malloc(sizeof(node));
    strcpy(new->name, newName);
    new->next = start;
    start = new;
  }
  else {
    before = location(start, target);
    if (before == NULL)
      printf("\nInvalid entry! Please try again\n");
    else {
      new = (node *)malloc(sizeof(node));
      strcpy(new->name, newName);
      new->next = before->next;
      before->next = new;
    }
  }
  return (start);
}

node *delete (node *start)
{
  node *before, *tmp;
  char target[20];

  printf("\nEnter name to be deleted: ");
  scanf(" %[^\n]", target);

  if (strcmp(start->name, target) == 0)
    if (start->next == NULL) {
      free(start);
      start = NULL;
    }
    else
    {
      tmp = start->next;
      free(start);
      start = tmp;
    }
  else {
    before = location(start, target);
    if (before == NULL)
      printf("\nInvalid entry. Please try again.\n");
    else {
      tmp = before->next->next;
      free(before->next);
      before->next = tmp;
    }
  }
  return (start);
}

node *location(node *start, char target[])
{
  int flag = 1;
  if (strcmp(start->next->name, target) == 0)
    return (start);
  else if (start->next == NULL)
    return (NULL);
  else {

    do {
      start = start->next;
      if (strcmp(start->next->name, target) == 0)
        return (start);
      if (start->next == NULL) {
        flag = 0;
        printf("Invalid entry. Please try again.\n");
      }
    } while (flag);

  }
  return (NULL);
}
```

## 7. Create a Linear Linked List with Forward and Backward Traversing


```c

#include <stdio.h>
#include <string.h>
struct members {
  char name[20];
  struct members *forward, *backward;
};
typedef struct members node;
void showforward(node *start);
void showbackward(node *end);
main() {
  node m1, m2, m3, *start, *end;
  strcpy(m1.name, "lina");
  strcpy(m2.name, "mina");
  strcpy(m3.name, "bina");
  start = &m1;
  start->forward = &m2;
  start->forward->forward = &m3;
  start->forward->forward->forward = NULL;
  end = &m3;
  end->backward = &m2;
  end->backward->backward = &m1;
  end->backward->backward->backward = NULL;
  printf("Names of members (forward traversing):\n");
  showforward(start);
  printf("\nNames of members (backward traversing):\n");
  showbackward(end);
  return (0);
}
void showforward(node *start) {
  int flag = 1;
  do {
    printf("%s\n", start->name);
    if (start->forward == NULL)
      flag = 0;
    start = start->forward;
  } while (flag);
  return;
}
void showbackward(node *end) {
  int flag = 1;
  do {
    printf("%s\n", end->name);
    if (end->backward == NULL)
      flag = 0;
    end = end->backward;
  } while (flag);
  return;
}
```