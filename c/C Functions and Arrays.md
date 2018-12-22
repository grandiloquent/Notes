# C Functions and Arrays

- [1. Determine the Value of Pi](#1-determine-the-value-of-pi)
- [2. Pick the Prime Numbers from a List of Numbers](#2-pick-the-prime-numbers-from-a-list-of-numbers)
- [3. Sum Numbers Using Recursion](#3-sum-numbers-using-recursion)
- [4. Compute the Fibonacci Sequence Using Recursion](#4-compute-the-fibonacci-sequence-using-recursion)
- [5. Compute the Factorial of a Number Using Recursion](#5-compute-the-factorial-of-a-number-using-recursion)
- [6. Search the Largest Element in an Array of Integers](#6-search-the-largest-element-in-an-array-of-integers)
- [7. Solve the Classic Problem of the Towers of Hanoi](#7-solve-the-classic-problem-of-the-towers-of-hanoi)
- [8. Solve the Eight Queens Problem](#8-solve-the-eight-queens-problem)
- [9. Compute Permutations and Combinations](#9-compute-permutations-and-combinations)
- [10. Perform the Summation of Two Matrices](#10-perform-the-summation-of-two-matrices)
- [11. Compute the Transpose of a Matrix](#11-compute-the-transpose-of-a-matrix)
- [12. Compute the Product of Matrices](#12-compute-the-product-of-matrices)


## 1. Determine the Value of Pi


```c

#include <stdio.h>
#include <stdlib.h>
#include <math.h>
main() {
  int intP, intCircle, intSquare, intToss, intRM, i;
  float fltPi, fltX, fltY, fltR;
  char ch;
  intRM = RAND_MAX;
  do {
    intCircle = 0;
    do {
      printf("Enter the number of tosses (2 <= N <= 5000) : ");
      scanf("%d", &intToss);
    } while ((intToss < 2) || (intToss > 5000));
    intSquare = intToss;
    for (i = 0; i < intToss; i++) {
      intP = rand();
      fltX = ((float)intP) / intRM;
      intP = rand();
      fltY = ((float)intP) / intRM;
      fltR = sqrt((fltX * fltX) + (fltY * fltY));
      if (fltR <= 1)
        intCircle = intCircle + 1;
    }
    fltPi = 4 * ((float)intCircle) / intSquare;
    printf("\nThe value of pi is : %f\n", fltPi);
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you\n");
  return (0);
}
```

## 2. Pick the Prime Numbers from a List of Numbers


```c

#include <stdio.h>
#include <math.h>
#define SIZE 1000
int status[SIZE];
void sieve() {
  int i, j, sq;
  for (i = 0; i < SIZE; i++) {
    status[i] = 0;
  }
  sq = sqrt(SIZE);
  for (i = 4; i <= SIZE; i += 2) {
    status[i] = 1;
  }
  for (i = 3; i <= sq; i += 2) {
    if (status[i] == 0) {
      for (j = 2 * i; j <= SIZE; j += i)
        status[j] = 1;
    }
  }
  status[1] = 1;
}
main() {
  int i, intN;
  sieve();
  do {
    printf("\n\nEnter the number (1 <= N <= 1000) : ");
    scanf("%d", &intN);
  } while ((intN < 1) || (intN > 1000));
  printf("\nFollowing numbers are prime in the range:                          "
         "                 1 to %d :\n",
         intN);
  for (i = 1; i < intN; i++)
    if (status[i] == 0)
      printf("%d\t", i);
  printf("\nThank you.\n");
  return 0;
}
```

## 3. Sum Numbers Using Recursion


```c

#include <stdio.h>
int summation(int intM);
main() {
  int intN = 4, intR;
  intR = summation(intN);
  printf("Sum : 1 + 2 + 3 + 4 = %d\n", intR);
  return (0);
}
int summation(int intM) {
  if (intM == 1)
    return 1;
  else
    return (intM + summation(intM - 1));
}
```

## 4. Compute the Fibonacci Sequence Using Recursion


```c

#include <stdio.h>
int fib(int);
main() {
  int intK, intN;
  do {
    printf("Enter a suitable number: 1 <= N <= 24: ");
    scanf("%d", &intN);
  } while (intN < 1 || intN > 24);
  printf("The first %d Fibonacci numbers are:\n", intN);
  for (intK = 0; intK < intN; intK++) {
    printf("\t%d ", fib(intK));
    if (((intK + 1) % 6) == 0)
      printf("\n");
  }
  printf("\nThank you.\n");
  return 0;
}
int fib(int intP) {
  if (intP <= 0)
    return 0;
  else if (intP == 1)
    return 1;
  else
    return fib(intP - 1) + fib(intP - 2);
}
```

## 5. Compute the Factorial of a Number Using Recursion


```c

#include <stdio.h>
unsigned long int fact(int intM);
main() {
  int intN;
  unsigned long int lngN;
  do {
    printf("Enter 0 to discontinue\n");
    printf("Enter a suitable number: 1 <= N <= 12: ");
    scanf("%d", &intN);
    if (intN == 0)
      break;
    lngN = fact(intN);
    printf("%d! = %ld\n", intN, lngN);
  } while (1);
  printf("Thank you.\n");
  return (0);
}
unsigned long int fact(int intM) {
  if (intM == 1)
    return 1;
  else
    return (intM * fact(intM - 1));
}
```

## 6. Search the Largest Element in an Array of Integers


```c

#include <stdio.h>
int largest(int xList[], int low, int up);
main() {
  int intN, i, myList[15];
  do {
    printf("Enter the length of array (2 <= N <= 14) : ");
    scanf("%d", &intN);
  } while ((intN < 2) || (intN > 14));
  printf("Enter %d Elements : ", intN);
  for (i = 0; i < intN; i++)
    scanf("%d", &myList[i]);
  printf("The largest element in array: %d", largest(myList, 0, (intN - 1)));
  printf("\nThank you.\n");
  return 0;
}
int largest(int xList[], int low, int up) {
  int max;
  if (low == up)
    return xList[low];
  else {
    max = largest(xList, low + 1, up);
    if (xList[low] >= max)
      return xList[low];
    else
      return max;
  }
}
```

## 7. Solve the Classic Problem of the Towers of Hanoi


```c

#include <stdio.h>
void move(int N, char chrFrom, char chrTo, char chrTemp);
main() {
  int intN;
  do {
    printf("\nEnter 0 to discontinue\n");
    do {
      printf("Enter a number (1 <= n <= 10): ");
      scanf("%d", &intN);
    } while ((intN < 0) || (intN > 10));
    if (intN == 0)
      break;
    move(intN, 'L', 'R', 'C');
  } while (1);
  printf("Thank you.\n");
  return (0);
}
void move(int N, char chrFrom, char chrTo, char chrTemp) {
  if (N > 0) {
    move(N - 1, chrFrom, chrTemp, chrTo);
    printf("Move disk %d from %c to %c\n", N, chrFrom, chrTo);
    move(N - 1, chrTemp, chrTo, chrFrom);
  }
  return;
}
```

## 8. Solve the Eight Queens Problem


```c

#include <stdio.h>
#include <math.h>
void queen(int row, int p);
int chess[8], count;
main() {
  int p = 8;
  queen(1, p);
  return 0;
}
void print(int p) {
  int i, j;
  char ch;
  printf("\n\nThis is Solution no. %d:\n\n", ++count);
  for (i = 1; i <= p; ++i)
    printf("\t%d", i);
  for (i = 1; i <= p; ++i) {
    printf("\n\n%d", i);
    for (j = 1; j <= p; ++j) {
      if (chess[i] == j)
        printf("\tQ");
      else
        printf("\t-");
    }
  }
  printf("\n\n\nThere are total 92 solutions for 8-queens problem.");
  printf("\nStrike Enter key to continue : ");
  scanf("%c", &ch);
}
int place(int row, int column) {
  int i;
  for (i = 1; i <= row - 1; ++i) {
    if (chess[i] == column)
      return 0;
    else if (abs(chess[i] - column) == abs(i - row))
      return 0;
  }
  return 1;
}
void queen(int row, int p) {
  int column;
  for (column = 1; column <= p; ++column) {
    if (place(row, column)) {
      chess[row] = column;
      if (row == p)
        print(p);
      else
        queen(row + 1, p);
    }
  }
}
```

## 9. Compute Permutations and Combinations


```c

#include <stdio.h>
int fact(int);
int combination(int, int);
int permutation(int, int);
int main() {
  int intN, intR, intC, intP;
  char ch;
  do {
    do {
      printf("Enter the total no. of objects (1 <= n <= 7) :");
      scanf("%d", &intN);
    } while ((intN < 1) || (intN > 7));
    do {
      printf("Enter the no. of objects to be picked at a time ");
      printf("(1 <= r <= %d) :", intN);
      scanf("%d", &intR);
    } while ((intR < 1) || (intR > intN));
    intC = combination(intN, intR);
    intP = permutation(intR, intR);
    printf("\nCombinations : %d", intC);
    printf("\nPermutations : %d", intP);
    fflush(stdin);
    printf("\nDo you want to continue? (Y/N) : ");
    scanf("%c", &ch);
  } while ((ch == 'Y') || (ch == 'y'));
  printf("\nThank you.\n");
  return 0;
}
int combination(int intN, int intR) {
  int intC;
  intC = fact(intN) / (fact(intR) * fact(intN - intR));
  return intC;
}
int permutation(int intN, int intR) {
  int intP;
  intP = fact(intN) / fact(intN - intR);
  return intP;
}
int fact(int intN) {
  int i;
  int facto = 1;
  for (i = 1; i <= intN; i++) {
    facto = facto * i;
  }
  return facto;
}
```
## 10. Perform the Summation of Two Matrices


```c

#include <stdio.h>
void input(int mat[][12], int, int);
void output(int mat[][12], int, int);
void add(int matA[][12], int matB[][12], int matC[][12], int, int);
int main() {
  int row, col;
  int A[12][12], B[12][12], C[12][12];
  do {
    printf("Enter number of rows (1 <= M <= 12) :");
    scanf("%d", &row);
  } while ((row < 1) || (row > 12));
  do {
    printf("Enter number of columns (0 < N <= 12) :");
    scanf("%d", &col);
  } while ((col < 1) || (col > 12));
  printf("\nEnter Data for Matrix A :\n");
  input(A, row, col);
  printf("\n");
  printf("\nMatrix A Entered by you :\n");
  output(A, row, col);
  printf("\nEnter Data for Matrix B :\n");
  input(B, row, col);
  printf("\n");
  printf("\nMatrix B Entered by you :\n");
  output(B, row, col);
  add(A, B, C, row, col);
  printf("\nMatirx A + Matrix B = Matrix C. \n");
  printf("Matrix C :\n");
  output(C, row, col);
  printf("\nThank you. \n");
  return 0;
}
void input(int mat[][12], int row, int col) {
  int i, j;
  for (i = 0; i < row; i++) {
    printf("Enter %d values for row no. %d : ", col, i);
    for (j = 0; j < col; j++)
      scanf("%d", &mat[i][j]);
  }
}
void output(int mat[][12], int row, int col) {
  int i, j;
  for (i = 0; i < row; i++) {
    for (j = 0; j < col; j++) {
      printf("%d\t", mat[i][j]);
    }
    printf("\n");
  }
}
void add(int matA[][12], int matB[][12], int matC[][12], int m, int n) {
  int i, j;
  for (i = 0; i < m; i++) {
    for (j = 0; j < n; j++) {
      matC[i][j] = matA[i][j] + matB[i][j];
    }
  }
}
```

## 11. Compute the Transpose of a Matrix


```c

#include <stdio.h>
main() {
  int mat[12][12], transpose[12][12];
  int i, j, row, col;
  do {
    printf("Enter number of rows R (0 < R < 13): ");
    scanf("%d", &row);
  } while ((row < 1) || (row > 12));
  do {
    printf("Enter number of columns C (0 < C < 13): ");
    scanf("%d", &col);
  } while ((col < 1) || (col > 12));
  for (i = 0; i < row; i++) {
    printf("Enter %d values for row no. %d : ", col, i);
    for (j = 0; j < col; j++)
      scanf("%d", &mat[i][j]);
  }
  printf("\nMatrix A:\n");
  for (i = 0; i < row; i++) {
    for (j = 0; j < col; j++) {
      printf("%d\t", mat[i][j]);
    }
    printf("\n");
  }
  for (i = 0; i < row; i++) {
    for (j = 0; j < col; j++) {
      transpose[j][i] = mat[i][j];
    }
  }
  printf("\nTranspose of matrix A: \n");
  for (i = 0; i < col; i++) {
    for (j = 0; j < row; j++) {
      printf("%d\t", transpose[i][j]);
    }
    printf("\n");
  }
  printf("\nThank you.\n");
  return 0;
}
```

## 12. Compute the Product of Matrices


```c

#include <stdio.h>
void input(int mat[][8], int, int);
void output(int mat[][8], int, int);
void product(int matA[][8], int matB[][8], int matC[][8], int, int, int);
int main() {
  int rowA, colA, rowB, colB;
  int matA[8][8], matB[8][8], matC[8][8];
  printf("This program performs product of matrices A and B (A x B).\n");
  do {
    printf("Enter number of rows in matrix A (1 <= M <= 8): ");
    scanf("%d", &rowA);
  } while ((rowA < 1) || (rowA > 8));
  do {
    printf("Enter number of columns in matrix A (1 <= N <= 8): ");
    scanf("%d", &colA);
  } while ((colA < 1) || (colA > 8));
  printf("\nNumber of rows in matrix B is equal ");
  printf("to number of columns in matirx A:\n");
  rowB = colA;
  do {
    printf("Enter number of columns in matrix B (1 <= P <= 8): ");
    scanf("%d", &colB);
  } while ((colB < 1) || (colB > 8));
  printf("\nEnter data for matrix A :\n");
  input(matA, rowA, colA);
  printf("\n");
  printf("Matrix A: \n");
  output(matA, rowA, colA);
  printf("\n");
  printf("\nEnter data for matrix B :\n");
  input(matB, rowB, colB);
  printf("\n");
  printf("Matrix B: \n");
  output(matB, rowB, colB);
  printf("\n");
  product(matA, matB, matC, rowA, colA, colB);
  printf("Matrix C (matrix A x matrix B = matrix C) : \n");
  output(matC, rowA, colB);
  printf("\nThank you.\n");
  return 0;
}
void input(int mat[][8], int row, int col) {
  int i, j;
  for (i = 0; i < row; i++) {
    printf("Enter %d values for row no. %d : ", col, i);
    for (j = 0; j < col; j++)
      scanf("%d", &mat[i][j]);
  }
}
void output(int mat[][8], int row, int col) {
  int i, j;
  for (i = 0; i < row; i++) {
    for (j = 0; j < col; j++) {
      printf("%d\t", mat[i][j]);
    }
    printf("\n");
  }
}
void product(int matA[][8], int matB[][8], int matC[][8], int m1, int n1,
             int n2){
  int i, j, t;
  for (i = 0; i < m1; i++) {
    for (j = 0; j < n2; j++) {
      matC[i][j] = 0;
      for (t = 0; t < n1; t++) {
        matC[i][j] += matA[i][t] * matB[t][j];
      }
    }
  }
}
```
