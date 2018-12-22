# C Functions and Structures with Pointers

- [1. Pass Arguments by Reference to a Function](#1-pass-arguments-by-reference-to-a-function)
- [2. Display Data Stored in Nested Structures](#2-display-data-stored-in-nested-structures)
- [3. Build a Structure Using a Function](#3-build-a-structure-using-a-function)
- [4. Modify the Data in a Structure by Passing It to a Function](#4-modify-the-data-in-a-structure-by-passing-it-to-a-function)
- [5. Modify the Data in a Structure by Passing a Pointer-to-Structure to a Function](#5-modify-the-data-in-a-structure-by-passing-a-pointer-to-structure-to-a-function)
- [6. Store and Retrieve Data Using an Array of Structures](#6-store-and-retrieve-data-using-an-array-of-structures)
- [7. Store and Retrieve Data Using an Array of Structures in Interactive Mode](#7-store-and-retrieve-data-using-an-array-of-structures-in-interactive-mode)
- [8. Invoke a Function Using a Pointer-to-Function](#8-invoke-a-function-using-a-pointer-to-function)
- [9. Implement a Text-Based Menu System](#9-implement-a-text-based-menu-system)

## 1. Pass Arguments by Reference to a Function


```c

#include <stdio.h>
void changeCreditCount(int *p1, int *p2);
main() {
  int intCC1 = 15, intCC2 = 20;
  printf("Computer-control is in main() function\n");
  printf("intCC1 = %d and intCC2 = %d\n", intCC1, intCC2);
  changeCreditCount(&intCC1, &intCC2);
  printf("Computer-control is back in main() function\n");
  printf("intCC1 = %d and intCC2 = %d\n", intCC1, intCC2);
  return (0);
}
void changeCreditCount(int *p1, int *p2) {
  printf("Computer-control is in changeCreditCount() function\n");
  printf("Initial values of *p1 and *p2: \n");
  printf("*p1 = %d and *p2 = %d\n", *p1, *p2);
  *p1 = *p1 * 4;
  *p2 = *p2 * 4;
  printf("Now values of *p1 and *p2 are changed\n");
  printf("*p1 = %d and *p2 = %d\n", *p1, *p2);
  return;
}
```

## 2. Display Data Stored in Nested Structures


```c

#include <stdio.h>
int main() {
  struct date {
    int month;
    int day;
    int year;
  };
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
    struct date joiningDate;
  };
  struct biodata *ptr1, sa1 = {"Dick", 1, 21, 70.6F, 10, 18, 2006};
  struct biodata *ptr2, sa2 = {"Robert", 2, 22, 75.8F, 8, 24, 2007};
  ptr1 = &sa1;
  ptr2 = &sa2;
  printf("Biodata of Secret Agent # 1: \n");
  printf("\tName: %s\n", (*ptr1).name);
  printf("\tRoll Number: %d\n", (*ptr1).rollno);
  printf("\tAge: %d years \n", (*ptr1).age);
  printf("\tWeight: %.1f kg\n", (*ptr1).weight);
  printf("\tJoining Date: %d/%d/%d\n\n", (*ptr1).joiningDate.month,
         (*ptr1).joiningDate.day, (*ptr1).joiningDate.year);
  printf("Biodata of Secret Agent # 2: \n");
  printf("\tName: %s\n", ptr2->name);
  printf("\tRoll Number: %d\n", ptr2->rollno);
  printf("\tAge: %d years \n", ptr2->age);
  printf("\tWeight: %.1f kg\n", ptr2->weight);
  printf("\tJoining Date: %d/%d/%d\n", ptr2->joiningDate.month,
         ptr2->joiningDate.day, ptr2->joiningDate.year);
  return (0);
}
```

## 3. Build a Structure Using a Function


```c

#include <stdio.h>
struct rectangle {
  int height;
  int width;
};
struct rectangle makeIt(int height, int width);
main() {
  struct rectangle rect1, rect2;
  rect1 = makeIt(20, 30);
  rect2 = makeIt(40, 80);
  printf("Dimensions of rect1: \n");
  printf("height: %d\n", rect1.height);
  printf("width: %d\n\n", rect1.width);
  printf("Dimensions of rect2: \n");
  printf("height: %d\n", rect2.height);
  printf("width: %d\n\n", rect2.width);
  return (0);
}
struct rectangle makeIt(int height, int width) {
  struct rectangle myRectangle;
  myRectangle.height = height;
  myRectangle.width = width;
  return myRectangle;
}
```

## 4. Modify the Data in a Structure by Passing It to a Function


```c

#include <stdio.h>
struct rectangle {
  int height;
  int width;
};
struct rectangle doubleIt(struct rectangle ourRect);
main() {
  struct rectangle rect1 = {10, 15}, rect2 = {25, 35};
  printf("Dimensions of rect1 before modification: \n");
  printf("height: %d\n", rect1.height);
  printf("width: %d\n\n", rect1.width);
  rect1 = doubleIt(rect1);
  printf("Dimensions of rect1 after modification: \n");
  printf("height: %d\n", rect1.height);
  printf("width: %d\n\n", rect1.width);
  printf("Dimensions of rect2 before modification: \n");
  printf("height: %d\n", rect2.height);
  printf("width: %d\n\n", rect2.width);
  rect2 = doubleIt(rect2);
  printf("Dimensions of rect2 after modification: \n");
  printf("height: %d\n", rect2.height);
  printf("width: %d\n\n", rect2.width);
  return (0);
}
struct rectangle doubleIt(struct rectangle ourRect) {
  ourRect.height = 2 * ourRect.height;
  ourRect.width = 2 * ourRect.width;
  return ourRect;
}
```

## 5. Modify the Data in a Structure by Passing a Pointer-to-Structure to a Function


```c

#include <stdio.h>
struct rectangle {
  int height;
  int width;
};
void doubleIt(struct rectangle *ptr);
main() {
  struct rectangle rect1 = {10, 15}, rect2 = {25, 35};
  printf("Dimensions of rect1 before modification: \n");
  printf("height: %d\n", rect1.height);
  printf("width: %d\n\n", rect1.width);
  doubleIt(&rect1);
  printf("Dimensions of rect1 after modification: \n");
  printf("height: %d\n", rect1.height);
  printf("width: %d\n\n", rect1.width);
  printf("Dimensions of rect2 before modification: \n");
  printf("height: %d\n", rect2.height);
  printf("width: %d\n\n", rect2.width);
  doubleIt(&rect2);
  printf("Dimensions of rect2 after modification: \n");
  printf("height: %d\n", rect2.height);
  printf("width: %d\n\n", rect2.width);
  return (0);
}
void doubleIt(struct rectangle *ptr) {
  ptr->height = 2 * ptr->height;
  ptr->width = 2 * ptr->width;
  return;
}
```

## 6. Store and Retrieve Data Using an Array of Structures


```c

#include <stdio.h>
main() {
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
  };
  struct biodata agents[2];
  strcpy(agents[0].name, "Dick");
  agents[0].rollno = 1;
  agents[0].age = 21;
  agents[0].weight = 70.6F;
  strcpy(agents[1].name, "Robert");
  agents[1].rollno = 2;
  agents[1].age = 22;
  agents[1].weight = 75.8F;
  printf("Biodata of Secret Agent # 1: \n");
  printf("\tName: %s\n", agents[0].name);
  printf("\tRoll Number: %d\n", agents[0].rollno);
  printf("\tAge: %d years \n", agents[0].age);
  printf("\tWeight: %.1f kg\n\n", agents[0].weight);
  printf("Biodata of Secret Agent # 2: \n");
  printf("\tName: %s\n", agents[1].name);
  printf("\tRoll Number: %d\n", agents[1].rollno);
  printf("\tAge: %d years \n", agents[1].age);
  printf("\tWeight: %.1f kg\n", agents[1].weight);
  return (0);
}
```

## 7. Store and Retrieve Data Using an Array of Structures in Interactive Mode


```c

#include <stdio.h>
main() {
  int i;
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
  };
  struct biodata agents[5];
  for (i = 0; i < 5; i++) {
    printf("\nEnter Biodata of Secret Agent # %d: \n", i + 1);
    printf("Name: ");
    scanf("%s", &agents[i].name);
    printf("Roll Number: ");
    scanf("%d", &agents[i].rollno);
    printf("Age: ");
    scanf("%d", &agents[i].age);
    printf("Weight: ");
    scanf("%f", &agents[i].weight);
  }
  printf("\nNow data entered by you will ");
  printf("be displayed on the screen.\n\n");
  for (i = 0; i < 5; i++) {
    printf("Biodata of Secret Agent # %d: \n", i + 1);
    printf("\tName: %s\n", agents[i].name);
    printf("\tRoll Number: %d\n", agents[i].rollno);
    printf("\tAge: %d years \n", agents[i].age);
    printf("\tWeight: %.1f kg\n\n", agents[i].weight);
  }
  return (0);
}
linkfloat() {
  float number = 10, *pointer;
  pointer = &number;
  number = *pointer;
  return (0);
}
```

## 8. Invoke a Function Using a Pointer-to-Function


```c

#include <stdio.h>
int sum(double n1, double n2);
int add(int m1, int m2);
main() {
  int r;
  int (*ptrFunc)();
  ptrFunc = sum;
  r = (*ptrFunc)(2.3, 4.5);
  printf("(int)(2.3 + 4.5) = %d\n", r);
  ptrFunc = add;
  r = (*ptrFunc)(10, 15);
  printf("10 + 15 = %d\n", r);
  return (0);
}
int sum(double j1, double j2) {
  int result;
  result = (int)(j1 + j2);
  return (result);
}
int add(int k1, int k2) { return (k1 + k2); }
```

## 9. Implement a Text-Based Menu System


```c

#include <stdio.h>
void cut(int intCut);
void copy(int intCopy);
void paste(int intPaste);
void delete (int intDelete);
main() {
  void (*funcPtr[4])(int) = {cut, copy, paste, delete};
  int intChoice;
  printf("\nEdit Menu: Enter your choices (0, 1, 2, or 3).\n");
  printf("Please do not enter any other number   except 0, 1, 2, or 3 to \n");
  printf("avoid abnormal termination of program.\n");
  printf("Enter 0 to activate menu-item Cut.\n");
  printf("Enter 1 to activate menu-item Copy.\n");
  printf("Enter 2 to activate menu-item Paste.\n");
  printf("Enter 3 to activate menu-item Delete.\n");
  scanf("%d", &intChoice);
  (*funcPtr[intChoice])(intChoice);
  printf("Thank you.\n");
  return (0);
}
void cut(int intCut) {
  printf("You entered %d.\n", intCut);
  printf("Menu-item Cut is activated.\n");
}
void copy(int intCopy) {
  printf("You entered %d.\n", intCopy);
  printf("Menu-item Copy is activated.\n");
}
void paste(int intPaste) {
  printf("You entered %d.\n", intPaste);
  printf("Menu-item Paste is activated.\n");
}
void delete (int intDelete)
{
  printf("You entered %d.\n", intDelete);
  printf("Menu-item Delete is activated.\n");
}
```


