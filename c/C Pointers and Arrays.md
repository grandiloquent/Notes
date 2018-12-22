# C Pointers and Arrays

- [1. Retrieve Data from an Array with the int Type Data](#1-retrieve-data-from-an-array-with-the-int-type-data)
- [2. Retrieve Data from an Array Using the Array Name](#2-retrieve-data-from-an-array-using-the-array-name)
- [3. Retrieve Data from an Array with char and double Type Data](#3-retrieve-data-from-an-array-with-char-and-double-type-data)
- [4. Access the Out-of-Bounds Array Elements](#4-access-the-out-of-bounds-array-elements)
- [5. Store Strings](#5-store-strings)
- [6. Store Strings Without Initialization](#6-store-strings-without-initialization)
- [7. Store Strings in an Interactive Session](#7-store-strings-in-an-interactive-session)
- [8. Retrieve the Addresses of Elements in a Two-Dimensional Array](#8-retrieve-the-addresses-of-elements-in-a-two-dimensional-array)
- [9. Retrieve the Base Addresses of Rows in a Two-Dimensional Array](#9-retrieve-the-base-addresses-of-rows-in-a-two-dimensional-array)
- [10. Retrieve Data from a Two-Dimensional Array](#10-retrieve-data-from-a-two-dimensional-array)
- [11. Retrieve Data from a Two-Dimensional Array Using an Array Name](#11-retrieve-data-from-a-two-dimensional-array-using-an-array-name)
- [12. Retrieve Data from an Array Using an Array of Pointers](#12-retrieve-data-from-an-array-using-an-array-of-pointers)
- [13. Swap Strings Physically](#13-swap-strings-physically)
- [14. Swap Strings Logically](#14-swap-strings-logically)
- [15. Store Strings Interactively](#15-store-strings-interactively)
- [16. Pass Arguments to a Program from the Command Line](#16-pass-arguments-to-a-program-from-the-command-line)
- [17. Retrieve Stored Strings Using a Pointer to a Pointer](#17-retrieve-stored-strings-using-a-pointer-to-a-pointer)

## 1. Retrieve Data from an Array with the int Type Data


```c

#include <stdio.h>

main()
{
 int marks [] = {72, 56, 50, 80, 92};
 int i, *ptr;
 ptr = &marks[0];
 for (i = 0; i < 5; i++)
  printf("Element no %d, value: %d\n", i+1, *(ptr+i)) ;
  printf("Value of array name marks is: %u\n", marks);
 return(0);
}
```

## 2. Retrieve Data from an Array Using the Array Name


```c

#include <stdio.h>
main() {
  int marks[] = {72, 56, 50, 80, 92};
  int i;
  printf("Values stored in array elements.\n");
  for (i = 0; i < 5; i++)
    printf("Element no. %d, value: %d\n", i + 1, *(marks + i));
  printf("\nValue of array-name marks: %u\n\n", marks);
  printf("Addresses of array elements:\n");
  for (i = 0; i < 5; i++)
    printf("Address of element marks[%d] : %u\n", i, &marks[i]);
  return (0);
}
```

## 3. Retrieve Data from an Array with char and double Type Data


```c

#include <stdio.h>
main() {
  char text[] = "Hello";
  double num[] = {2.4, 5.7, 9.1, 4.5, 8.2};
  int i;
  printf("Values stored in elements of array text.\n");
  for (i = 0; i < 5; i++)
    printf("Element no. %d, value: %c\n", i + 1, *(text + i));
  printf("\nValues stored in elements of array num.\n");
  for (i = 0; i < 5; i++)
    printf("Element no. %d, value: %.1f\n", i + 1, *(num + i));
  printf("\nValue of array-name text: %u\n\n", text);
  printf("Addresses of elements of array text:\n");
  for (i = 0; i < 5; i++)
    printf("Address of element text[%d] : %u\n", i, &text[i]);
  printf("\nValue of array-name num: %u\n\n", num);
  printf("Addresses of elements of array num:\n");
  for (i = 0; i < 5; i++)
    printf("Address of element num[%d] : %u\n", i, &num[i]);
  return (0);
}
```

## 4. Access the Out-of-Bounds Array Elements


```c

#include <stdio.h>
main() {
  int num[] = {12, 23, 45, 65, 27, 83, 32, 93, 62, 74, 41};
  int *ipt1, *ipt2;
  ipt1 = &num[5];
  ipt2 = &num[6];
  printf("Current value of *ipt1: %d\n", *ipt1);
  printf("Current value of *ipt2: %d\n", *ipt2);
  ipt1 = ipt1 - 2;
  ipt2 = ipt2 + 3;
  printf("Now current value of *ipt1: %d\n", *ipt1);
  printf("Now current value of *ipt2: %d\n", *ipt2);
  ipt1 = ipt1 - 15;
  ipt2 = ipt2 + 22;
  printf("Now current value of *ipt1: %d\n", *ipt1);
  printf("Now current value of *ipt2: %d\n", *ipt2);
  return (0);
}
```

## 5. Store Strings


```c

#include <stdio.h>
main() {
  int i;
  char name[] = "Shirish";
  char *pname = "Shirish";
  printf("name: %s\n", name);
  printf("pname: %s\n", pname);
  strcpy(name, "Dick");
  pname = "Dick";
  printf("name: %s\n", name);
  printf("pname: %s\n", pname);
  printf("name (all eight bytes):  ");
  for (i = 0; i < 8; i++) {
    printf("%c ", name[i]);
  }
  printf("\npname (all eight bytes):  ");
  for (i = 0; i < 8; i++) {
    printf("%c ", *(pname + i));
  }
  return (0);
}
```

## 6. Store Strings Without Initialization


```c

#include <stdio.h>
main() {
  int i;
  char name[8];
  char *pname;
  strcpy(name, "Shirish");
  pname = "Shirish";
  printf("\nname: %s\n", name);
  printf("pname: %s\n", pname);
  strcpy(name, "Dick");
  pname = "Dick";
  printf("name: %s\n", name);
  printf("pname: %s\n", pname);
  printf("name (all eight bytes):  ");
  for (i = 0; i < 8; i++) {
    printf("%c ", name[i]);
  }
  printf("\npname (all eight bytes):  ");
  for (i = 0; i < 8; i++) {
    printf("%c ", *(pname + i));
  }
  return (0);
}
```

## 7. Store Strings in an Interactive Session


```c

#include <stdio.h>
main() {
  int i;
  char name[8];
  char *pname;
  printf("\nEnter name: ");
  scanf(" %[^\n]", name);
  printf("Enter name again: ");
  scanf(" %[^\n]", pname);
  printf("name: %s\n", name);
  printf("pname: %s\n", pname);
  return (0);
}
```

## 8. Retrieve the Addresses of Elements in a Two-Dimensional Array


```c

#include <stdio.h>
main() {
  int r, c;
  int num[3][2] = {{14, 457}, {24, 382}, {72, 624}};
  for (r = 0; r < 3; r++) {
    for (c = 0; c < 2; c++)
      printf("Address of num[%d][%d]: %u \n", r, c, &num[r][c]);
  }
  return (0);
}
```

## 9. Retrieve the Base Addresses of Rows in a Two-Dimensional Array


```c

#include <stdio.h>
main() {
  int r;
  int num[3][2] = {{14, 457}, {24, 382}, {72, 624}};
  for (r = 0; r < 3; r++)
    printf("Base address of row %d is: %u \n", r + 1, num[r]);
  return (0);
}
```
## 10. Retrieve Data from a Two-Dimensional Array


```c

#include <stdio.h>
main() {
  int num[3][2] = {{14, 457}, {24, 382}, {72, 624}};
  int(*ptrArray)[2];
  int row, col, *ptrInt;
  for (row = 0; row < 3; row++) {
    ptrArray = &num[row];
    ptrInt = (int *)ptrArray;
    for (col = 0; col < 2; col++)
      printf("%d    ", *(ptrInt + col));
    printf("\n");
  }
  return (0);
}
```

## 11. Retrieve Data from a Two-Dimensional Array Using an Array Name


```c

#include <stdio.h>
main() {
  int i, j, *p1, *p2, *p3;
  int t1[][4] = {{12, 14, 16, 18}, {22, 24, 26, 28}, {32, 34, 36, 38}};
  int t2[3][4] = {{13, 15, 17, 19}, {23, 25, 27, 29}, {33, 35, 37, 39}};
  int t3[3][4];
  printf("\nTable t3 is computed and displayed:\n\n");
  for (i = 0; i < 3; i++) {
    for (j = 0; j < 4; j++) {
      *(t3[i] + j) = *(t1[i] + j) + *(t2[i] + j);
      printf("%d  ", *(t3[i] + j));
    }
    printf("\n");
  }
  printf("\n\nTable t3 is computed and displayed again:\n\n");
  for (i = 0; i < 3; i++) {
    for (j = 0; j < 4; j++) {
      *(*(t3 + i) + j) = *(*(t1 + i) + j) + *(*(t2 + i) + j);
      printf("%d  ", t3[i][j]);
    }
    printf("\n");
  }
  p1 = (int *)t1;
  p2 = (int *)t2;
  p3 = (int *)t3;
  printf("\n\nTable t3 is computed and displayed again:\n\n");
  for (i = 0; i < 3; i++) {
    for (j = 0; j < 4; j++) {
      *(p3 + i * 4 + j) = *(p1 + i * 4 + j) + *(p2 + i * 4 + j);
      printf("%d  ", *(p3 + i * 4 + j));
    }
    printf("\n");
  }
  return (0);
}
```

## 12. Retrieve Data from an Array Using an Array of Pointers


```c

#include <stdio.h>
main() {
  int i, intArray[6];
  int *ptrArray[6];
  for (i = 0; i < 6; i++) {
    intArray[i] = (i + 2) * 100;
    ptrArray[i] = &intArray[i];
  }
  for (i = 0; i < 6; i++)
    printf("intArray[%d], Value: %d, Address: %u\n", i, *(ptrArray[i]),
           ptrArray[i]);
  return (0);
}
```

## 13. Swap Strings Physically


```c

#include <stdio.h>
main() {
  int i;
  char temp;
  char friends[5][10] = {"Kernighan", "Camarda", "Ford", "Nixon", "Wu"};
  printf("Strings before swapping:\n");
  for (i = 0; i < 5; i++)
    printf("Friend no. %d : %s\n", i + 1, friends[i]);
  for (i = 0; i < 10; i++) {
    temp = friends[0][i];
    friends[0][i] = friends[1][i];
    friends[1][i] = temp;
  }
  printf("\nStrings after swapping:\n");
  for (i = 0; i < 5; i++)
    printf("Friend no. %d : %s\n", i + 1, friends[i]);
  return (0);
}
```

## 14. Swap Strings Logically


```c

#include <stdio.h>
main() {
  int i;
  char *temporary;
  char *friends[5] = {"Kernighan", "Camarda", "Ford", "Nixon", "Wu"};
  printf("Strings before swapping:\n");
  for (i = 0; i < 5; i++)
    printf("Friend no. %d : %s\n", i + 1, friends[i]);
  temporary = friends[1];
  friends[1] = friends[0];
  friends[0] = temporary;
  printf("\nStrings after swapping:\n");
  for (i = 0; i < 5; i++)
    printf("Friend no. %d : %s\n", i + 1, friends[i]);
  return (0);
}
```

## 15. Store Strings Interactively


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
main() {
  char *friends[5], *ptr, name[30];
  int i, length;
  for (i = 0; i < 5; i++) {
    printf("Enter name of friend no. %d: ", i + 1);
    scanf(" %[^\n]", name);
    length = strlen(name);
    ptr = (char *)malloc(length + 1);
    strcpy(ptr, name);
    friends[i] = ptr;
  }
  printf("\n\nList of friends:\n");
  for (i = 0; i < 5; i++)
    printf("Friend no. %d : %s\n", i + 1, friends[i]);
  return (0);
}
```

## 16. Pass Arguments to a Program from the Command Line


```c

#include <stdio.h>
main(int argc, char *argv[]) {
  int i;
  printf("Few towns in %s district:\n", argv[1]);
  for (i = 2; i < argc; i++)
    printf("%s\n", argv[i]);
  return (0);
}
```

## 17. Retrieve Stored Strings Using a Pointer to a Pointer


```c

#include <stdio.h>
main() {
  int i, j;
  char ch;
  char cities[5][10] = {"Satara", "Sangli", "Karad", "Pune", "Mumbai"};
  char *ptr, **ptrPtr;
  ptrPtr = &ptr;
  for (i = 0; i < 5; i++) {
    ptr = (char *)cities[i];
    j = 0;
    do {
      ch = *(ptr + j);
      printf("%c", ch);
      j = j + 1;
    } while (ch != '\0');
    printf("\t\t");
    j = 0;
    do {
      ch = *(*ptrPtr + j);
      printf("%c", ch);
      j = j + 1;
    } while (ch != '\0');
    printf("\n");
  }
  return (0);
}
```
