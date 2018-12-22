# C Control Statements

- [1. Sum 1 to N Numbers](#1-sum-1-to-n-numbers)
- [2. Compute the Factorial of a Number](#2-compute-the-factorial-of-a-number)
- [3. Generate a Fibonacci Sequence](#3-generate-a-fibonacci-sequence)
- [4. Determine Whether a Given Number Is Prime](#4-determine-whether-a-given-number-is-prime)
- [5. Compute the Sine Function](#5-compute-the-sine-function)
- [6. Compute the Cosine Function](#6-compute-the-cosine-function)
- [7. Compute the Roots of Quadratic Equation](#7-compute-the-roots-of-quadratic-equation)
- [8. Compute the Reverse of an Integer](#8-compute-the-reverse-of-an-integer)
- [9. Print a Geometrical Pattern Using Nested Loops](#9-print-a-geometrical-pattern-using-nested-loops)
- [10. Generate a Table of Future Value Interest Factors](#10-generate-a-table-of-future-value-interest-factors)

## 1. Sum 1 to N Numbers


```c

#include <stdio.h>
main() {
  int intN, intCounter, flag;
  unsigned long int ulngSum;
  char ch;
  do {
    do {
      flag = 0;
      printf("Enter a number (0 < N < 30000): ");
      scanf("%d", &intN);
      if ((intN <= 0) || (intN > 30000))
        flag = 1;
    } while (flag);
    ulngSum = 0;
    for (intCounter = 1; intCounter <= intN; intCounter++) {
      ulngSum = ulngSum + intCounter;
    }
    printf("Required sum is: %lu\n", ulngSum);
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return (0);
}
```

## 2. Compute the Factorial of a Number


```c

#include <stdio.h>
main() {
  int intN, intCounter, flag;
  unsigned long int ulngFact;
  char ch;
  do {
    do {
      flag = 0;
      printf("Enter a number (0 < N <= 12): ");
      scanf("%d", &intN);
      if ((intN <= 0) || (intN > 12))
        flag = 1;
    } while (flag);
    ulngFact = 1;
    for (intCounter = 1; intCounter <= intN; intCounter++) {
      ulngFact = ulngFact * intCounter;
    }
    printf("Required factorial is: %lu\n", ulngFact);
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return (0);
}
```

## 3. Generate a Fibonacci Sequence


```c

#include <stdio.h>
main() {
  int intN, intK, flag;
  long int lngA, lngB, lngC, lngD;
  char ch;
  do {
    do {
      flag = 0;
      printf("Enter a number (0 < N <= 45): ");
      scanf("%d", &intN);
      if ((intN <= 0) || (intN > 45))
        flag = 1;
    } while (flag);
    lngA = 0;
    lngB = 1;
    printf("Fibonacci Sequence:\n");
    for (intK = 1; intK <= intN; intK++) {
      printf("%d th term is : %ld\n", ((intK * 2) - 1), lngA);
      if (((intK * 2) - 1) == intN)
        break;
      printf("%d th term is : %ld\n", (intK * 2), lngB);
      if ((intK * 2) == intN)
        break;
      lngC = lngA + lngB;
      lngD = lngB + lngC;
      lngA = lngC;
      lngB = lngD;
    }
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return (0);
}
```

## 4. Determine Whether a Given Number Is Prime


```c

#include <stdio.h>
#include <math.h>
main() {
  int flag, isPrime;
  long int lngN, lngM, i;
  do {
    do {
      flag = 0;
      printf("Enter 0 to discontinue.\n");
      printf("Enter a number N (2 <= N <= 2000000000)\n");
      printf("to find whether it is prime or not: ");
      scanf("%ld", &lngN);
      if (lngN == 0)
        break;
      if ((lngN < 2) || (lngN > 2000000000))
        flag = 1;
    } while (flag);
    if (lngN == 0)
      break;
    if (lngN == 2) {
      printf("\n2 is a prime number\n\n");
      continue;
    }
    isPrime = 1;
    lngM = ceil(sqrt(lngN));
    for (i = 2; i <= lngM; i++) {
      if ((lngN % i) == 0) {
        isPrime = 0;
        break;
      }
    }
    if (isPrime)
      printf("\n%ld is a prime number\n\n", lngN);
    else
      printf("\n%ld is not a prime number\n\n", lngN);
  } while (1);
  printf("\nThank you.\n");
  return (0);
}
```

## 5. Compute the Sine Function


```c

#include <stdio.h>
main() {
  double dblSine, dblTerm, dblX, dblZ;
  int intK, i, flag;
  char ch;
  do {
    do {
      flag = 0;
      printf("Enter angle in radians (-1 <= X <= 1): ");
      scanf("%lf", &dblX);
      if ((dblX < -1) || (dblX > 1))
        flag = 1;
    } while (flag);
    dblTerm = dblX;
    dblSine = dblX;
    intK = 1;
    dblZ = dblX * dblX;
    for (i = 1; i <= 10; i++) {
      intK = intK + 2;
      dblTerm = -dblTerm * dblZ / (intK * (intK - 1));
      dblSine = dblSine + dblTerm;
    }
    printf("Sine of %lf is %lf\n", dblX, dblSine);
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return (0);
}
```

## 6. Compute the Cosine Function


```c

#include <stdio.h>
main() {
  double dblCosine, dblX, dblZ;
  int i, j, q, flag, factorial, sign;
  char ch;
  do {
    do {
      flag = 0;
      printf("Enter angle in radians (-1 <= X <= 1): ");
      scanf("%lf", &dblX);
      if ((dblX < -1) || (dblX > 1))
        flag = 1;
    } while (flag);
    dblCosine = 0;
    sign = -1;
    for (i = 2; i <= 10; i += 2) {
      dblZ = 1;
      factorial = 1;
      for (j = 1; j <= i; j++)
        dblZ = dblZ * dblX;
      factorial = factorial * j;
      dblCosine += sign * dblZ / factorial;
      sign = -1 * sign;
    }
    dblCosine = 1 + dblCosine;
    printf("Cosine of %lf is %lf\n", dblX, dblCosine);
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return (0);
}
```

## 7. Compute the Roots of Quadratic Equation


```c

#include <stdio.h>
#include <math.h>
main() {
  double dblA, dblB, dblC, dblD, dblRt1, dblRt2;
  char ch;
  do {
    printf("Enter the values of a, b and c : ");
    scanf("%lf %lf %lf", &dblA, &dblB, &dblC);
    dblD = dblB * dblB - 4 * dblA * dblC;
    if (dblD == 0) {
      dblRt1 = (-dblB) / (2 * dblA);
      dblRt2 = dblRt1;
      printf("Roots are real & equal\n");
      printf("Root1 = %f, Root2 = %f\n", dblRt1, dblRt2);
    } else if (dblD > 0) {
      dblRt1 = -(dblB + sqrt(dblD)) / (2 * dblA);
      dblRt2 = -(dblB - sqrt(dblD)) / (2 * dblA);
      printf("Roots are real & distinct\n");
      printf("Root1 = %f, Root2 = %f\n", dblRt1, dblRt2);
    } else {
      printf("Roots are imaginary\n");
    }
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return 0;
}
```

## 8. Compute the Reverse of an Integer


```c

#include <stdio.h>
main() {
  long int intN, intTemp, intRemainder, intReverse;
  char ch;
  do {
    do {
      printf("Enter a number (0 < N <= 30000): ");
      scanf("%ld", &intN);
    } while ((intN <= 0) || (intN > 30000));
    intTemp = intN;
    intReverse = 0;
    while (intTemp > 0) {
      intRemainder = intTemp % 10;
      intReverse = intReverse * 10 + intRemainder;
      intTemp /= 10;
    }
    printf("The reverse of %ld is %ld.\n", intN, intReverse);
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you.\n");
  return 0;
}
```

## 9. Print a Geometrical Pattern Using Nested Loops


```c

#include <stdio.h>
main() {
  int intI, intJ, intK, intL, intOrd;
  char ch;
  do {
    do {
      printf("Enter the order of pattern (0 < N < 10): ");
      scanf("%d", &intOrd);
    } while ((intOrd <= 0) || (intOrd >= 10));
    for (intI = 1; intI <= intOrd; intI++) {
      for (intJ = intOrd; intJ > intI; intJ--) {
        printf(" ");
      }
      for (intK = intI; intK >= 1; intK--) {
        printf("%d", intK);
      }
      for (intL = 2; intL <= intI; intL++) {
        printf("%d", intL);
      }
      printf("\n");
    }
    printf("Do you want to continue? (Y/N) : ");
    scanf(" %c", &ch);
  } while ((ch == 'y') || (ch == 'Y'));
  printf("Thank you\n");
  return 0;
}
```
## 10. Generate a Table of Future Value Interest Factors


```c

#include <stdio.h>
#include <math.h>
#define MAX_INTEREST 6
#define MAX_PERIOD 10
main() {
  int i, interest, years;
  float fvif;
  printf("\nTable of FVIF (Future Value Interest Factors).");
  printf("\nRate of interest varies from 1% to 6%.");
  printf("\nPeriod varies from 1 year to 10 years.");
  printf("\n\n                    Interest Rate  ");
  printf("\n\t ---------------------------------------------");
  printf("\nPeriod");
  for (i = 1; i <= MAX_INTEREST; i++)
    printf("\t   %d%", i);
  printf("\n------------------------------------------------\n");
  for (years = 1; years <= MAX_PERIOD; years++) {
    printf("%d\t", years);
    for (interest = 1; interest <= MAX_INTEREST; interest++) {
      fvif = pow((1 + interest * 0.01), years);
      printf("%6.3f\t", fvif);
    }
    printf("\n");
  }
  printf("------------------------------------------------\n");
  printf("Thank you.\n");
  return 0;
}
```
