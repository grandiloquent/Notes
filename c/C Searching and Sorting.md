

# C Searching and Sorting

- [1. Find a Data Element Using a Linear Search](#1-find-a-data-element-using-a-linear-search)
- [2. Find a Data Element Using a Binary Search](#2-find-a-data-element-using-a-binary-search)
- [3. Sort a Given List of Numbers Using a Bubble Sort](#3-sort-a-given-list-of-numbers-using-a-bubble-sort)
- [4. Sort a Given List of Numbers Using an Insertion Sort](#4-sort-a-given-list-of-numbers-using-an-insertion-sort)
- [5. Sort a Given List of Numbers Using a Selection Sort](#5-sort-a-given-list-of-numbers-using-a-selection-sort)
- [6. Sort a Given List of Numbers Using a Merge Sort](#6-sort-a-given-list-of-numbers-using-a-merge-sort)
- [7. Sort a Given List of Numbers Using a Shell Sort](#7-sort-a-given-list-of-numbers-using-a-shell-sort)
- [8. Sort a Given List of Numbers Using a Quick Sort](#8-sort-a-given-list-of-numbers-using-a-quick-sort)


## 1. Find a Data Element Using a Linear Search


```c

#include <stdio.h>
int intStorage[50];
int kount = 0;
int searchData(int intData) {
  int intCompare = 0;
  int intNum = -1;
  int i;
  for (i = 0; i < kount; i++) {
    intCompare++;
    if (intData == intStorage[i]) {
      intNum = i;
      break;
    }
  }
  printf("Total Number of Comparisons Made Are: %d", intCompare);
  return intNum;
}
void main() {
  int intPosition, intData, i;
  printf("Enter the number of data elements N (2 <= N <= 50): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  printf("Enter the Data Element D to be Searched (0 <= D <= 30000): ");
  scanf("%d", &intData);
  intPosition = searchData(intData);
  if (intPosition != -1) {
    printf("\nData Element Found at Position ");
    printf("or Location: %d\n", (intPosition + 1));
  } else
    printf("\nData Element Not Found.\n");
  printf("\nThank you.\n");
}
```

## 2. Find a Data Element Using a Binary Search


```c

#include <stdio.h>
int intStorage[50];
int kount = 0;
int searchData(int intData) {
  int intLowBound = 0;
  int intUpBound = kount - 1;
  int intMidPoint = -1;
  int intCompare = 0;
  int intNum = -1;
  while (intLowBound <= intUpBound) {
    intCompare++;
    intMidPoint = intLowBound + (intUpBound - intLowBound) / 2;
    if (intStorage[intMidPoint] == intData) {
      intNum = intMidPoint;
      break;
    } else {
      if (intStorage[intMidPoint] < intData) {
        intLowBound = intMidPoint + 1;
      } else {
        intUpBound = intMidPoint - 1;
      }
    }
  }
  printf("Total comparisons made: %d", intCompare);
  return intNum;
}
void main() {
  int intPosition, intData, i;
  printf("Enter the number of data elements N (2 <= N <= 50): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("in increasing order, \nseparated by white spaces: ");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  printf("Enter the Data Element D to be Searched (0 <= D <= 30000): ");
  scanf("%d", &intData);
  intPosition = searchData(intData);
  if (intPosition != -1) {
    printf("\nData Element Found at Position ");
    printf("or Location: %d", (intPosition + 1));
  } else
    printf("\nData Element not found.");
  printf("\nThank you.\n");
}
```

## 3. Sort a Given List of Numbers Using a Bubble Sort


```c

#include <stdio.h>
int intStorage[20];
int kount = 0;
void bubbleSort() {
  int intTemp;
  int i, j;
  int intSwap = 0;
  for (i = 0; i < kount - 1; i++) {
    intSwap = 0;
    for (j = 0; j < kount - 1 - i; j++) {
      if (intStorage[j] > intStorage[j + 1]) {
        intTemp = intStorage[j];
        intStorage[j] = intStorage[j + 1];
        intStorage[j + 1] = intTemp;
        intSwap = 1;
      }
    }
    if (!intSwap) {
      break;
    }
  }
}
void main() {
  int i;
  printf("Enter the number of items in the list, N (2 <= N <= 20): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  bubbleSort();
  printf("Sorted List: ");
  for (i = 0; i < kount; i++)
    printf("%d ", intStorage[i]);
  printf("\nThank you.\n");
}
```

## 4. Sort a Given List of Numbers Using an Insertion Sort


```c

#include <stdio.h>
int intStorage[20];
int kount = 0;
void insertionSort() {
  int intInsert;
  int intVacancy;
  int i;
  for (i = 1; i < kount; i++) {
    intInsert = intStorage[i];
    intVacancy = i;
    while (intVacancy > 0 && intStorage[intVacancy - 1] > intInsert) {
      intStorage[intVacancy] = intStorage[intVacancy - 1];
      intVacancy--;
    }
    if (intVacancy != i) {
      intStorage[intVacancy] = intInsert;
    }
  }
}
void main() {
  int i;
  printf("Enter the number of data elements N (2 <= N <= 20): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  insertionSort();
  printf("Sorted List: ");
  for (i = 0; i < kount; i++)
    printf("%d ", intStorage[i]);
  printf("\nThank you.\n");
}
```

## 5. Sort a Given List of Numbers Using a Selection Sort


```c

#include <stdio.h>
int intStorage[20];
int kount = 0;
void selectSort() {
  int i, j, k, intTemp, intMin;
  for (i = 0; i < kount - 1; i++) {
    intMin = intStorage[i];
    k = i;
    for (j = i + 1; j < kount; j++) {
      if (intMin > intStorage[j]) {
        intMin = intStorage[j];
        k = j;
      }
    }
    intTemp = intStorage[i];
    intStorage[i] = intStorage[k];
    intStorage[k] = intTemp;
  }
}
void main() {
  int i;
  printf("Enter the number of data elements N (2 <= N <= 20): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  selectSort();
  printf("Sorted List: ");
  for (i = 0; i < kount; i++)
    printf("%d ", intStorage[i]);
  printf("\nThank you.\n");
}
```

## 6. Sort a Given List of Numbers Using a Merge Sort


```c

#include <stdio.h>
int intStorage[20];
int kount = 0;
void merge(int m1, int n1, int m2, int n2);
void mergeSort(int m, int n) {
  int intMid;
  if (m < n) {
    intMid = (m + n) / 2;
    mergeSort(m, intMid);
    mergeSort(intMid + 1, n);
    merge(m, intMid, intMid + 1, n);
  }
}
void merge(int m1, int n1, int m2, int n2) {
  int tmpStorage[40];
  int m, n, k;
  m = m1;
  n = m2;
  k = 0;
  while (m <= n1 && n <= n2) {
    if (intStorage[m] < intStorage[n])
      tmpStorage[k++] = intStorage[m++];
    else
      tmpStorage[k++] = intStorage[n++];
  }
  while (m <= n1)
    tmpStorage[k++] = intStorage[m++];
  while (n <= n2)
    tmpStorage[k++] = intStorage[n++];
  for (m = m1, n = 0; m <= n2; m++, n++)
    intStorage[m] = tmpStorage[n];
}
void main() {
  int kount, i;
  printf("Enter the number of data elements N (2 <= N <= 20): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  mergeSort(0, kount - 1);
  printf("Sorted List: ");
  for (i = 0; i < kount; i++)
    printf("%d ", intStorage[i]);
  printf("\nThank you.\n");
}
```

## 7. Sort a Given List of Numbers Using a Shell Sort


```c

#include <stdio.h>
int intStorage[20];
int kount = 0;
void shellSort() {
  int in, out;
  int insert;
  int gap = 1;
  int elements = kount;
  int i = 0;
  while (gap <= elements / 3)
    gap = gap * 3 + 1;
  while (gap > 0) {
    for (out = gap; out < elements; out++) {
      insert = intStorage[out];
      in = out;
      while (in > gap - 1 && intStorage[in - gap] >= insert) {
        intStorage[in] = intStorage[in - gap];
        in -= gap;
      }
      intStorage[in] = insert;
    }
    gap = (gap - 1) / 3;
    i++;
  }
}
void main() {
  int i;
  printf("Enter the number of items in the list, N (2 <= N <= 20): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  shellSort();
  printf("Sorted List: ");
  for (i = 0; i < kount; i++)
    printf("%d ", intStorage[i]);
  printf("\nThank you.\n");
}
```

## 8. Sort a Given List of Numbers Using a Quick Sort


```c

#include <stdio.h>
int intStorage[20];
int kount = 0;
void swap(int n1, int n2) {
  int intTemp = intStorage[n1];
  intStorage[n1] = intStorage[n2];
  intStorage[n2] = intTemp;
}
int partition(int left, int right, int pivot) {
  int lPtr = left - 1;
  int rPtr = right;
  while (1) {
    while (intStorage[++lPtr] < pivot) {
    }
    while (rPtr > 0 && intStorage[--rPtr] > pivot) {
    }
    if (lPtr >= rPtr)
      break;
    else
      swap(lPtr, rPtr);
  }
  swap(lPtr, right);
  return lPtr;
}
void quickSort(int left, int right) {
  int pivot, partPt;
  if (right - left <= 0) {
    return;
  } else {
    pivot = intStorage[right];
    partPt = partition(left, right, pivot);
    quickSort(left, partPt - 1);
    quickSort(partPt + 1, right);
  }
}
void main() {
  int i;
  printf("Enter the number of items in the list, N (2 <= N <= 20): ");
  scanf("%d", &kount);
  printf("Enter the %d integers I (0 <= I <= 30000) ", kount);
  printf("separated by white spaces: \n");
  for (i = 0; i < kount; i++)
    scanf("%d", &intStorage[i]);
  fflush(stdin);
  quickSort(0, kount - 1);
  printf("Sorted List: ");
  for (i = 0; i < kount; i++)
    printf("%d ", intStorage[i]);
  printf("\nThank you.\n");
}
```


