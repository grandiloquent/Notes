# C Numerical Methods

- [1. To Find the Roots of an Equation Using the Bisection Method](#1-to-find-the-roots-of-an-equation-using-the-bisection-method)
- [2. To Find the Roots of an Equation Using the Regula Falsi Method](#2-to-find-the-roots-of-an-equation-using-the-regula-falsi-method)
- [3. To Find the Roots of an Equation Using Muller’s Method](#3-to-find-the-roots-of-an-equation-using-mullers-method)
- [4. To Find the Roots of an Equation Using the Newton Raphson Method](#4-to-find-the-roots-of-an-equation-using-the-newton-raphson-method)
- [5. To Construct the New Data Points Using Newton’s Forward Method of Interpolation](#5-to-construct-the-new-data-points-using-newtons-forward-method-of-interpolation)
- [6. To Construct the New Data Points Using Newton’s Backward Method of Interpolation](#6-to-construct-the-new-data-points-using-newtons-backward-method-of-interpolation)
- [7. To Construct the New Data Points Using Gauss’s Forward Method of Interpolation](#7-to-construct-the-new-data-points-using-gausss-forward-method-of-interpolation)
- [8. To Construct the New Data Points Using Gauss’s Backward Method of Interpolation](#8-to-construct-the-new-data-points-using-gausss-backward-method-of-interpolation)
- [9. To Construct the New Data Points Using Stirling’s Method of Interpolation](#9-to-construct-the-new-data-points-using-stirlings-method-of-interpolation)
- [10. To Construct the New Data Points Using Bessel’s Method of Interpolation](#10-to-construct-the-new-data-points-using-bessels-method-of-interpolation)
- [11. To Construct the New Data Points Using Laplace Everett’s Method of Interpolation](#11-to-construct-the-new-data-points-using-laplace-everetts-method-of-interpolation)
- [12. To Construct the New Data Points Using Lagrange’s Method of Interpolation](#12-to-construct-the-new-data-points-using-lagranges-method-of-interpolation)
- [13. To Compute the Value of Integration Using Trapezoidal Method of Numerical Integration](#13-to-compute-the-value-of-integration-using-trapezoidal-method-of-numerical-integration)
- [14. To Compute the Value of Integration Using Simpson’s 3 8th Method of Numerical Integration](#14-to-compute-the-value-of-integration-using-simpsons-3-8th-method-of-numerical-integration)
- [15. To Compute the Value of Integration Using Simpson’s 1 3rd Method of Numerical Integration](#15-to-compute-the-value-of-integration-using-simpsons-1-3rd-method-of-numerical-integration)
- [16. To Solve a Differential Equation Using Modified Euler’s Method](#16-to-solve-a-differential-equation-using-modified-eulers-method)
- [17. To Solve a Differential Equation Using Runge Kutta Method](#17-to-solve-a-differential-equation-using-runge-kutta-method)

## 1. To Find the Roots of an Equation Using the Bisection Method


```c

#include <stdio.h>
#include <math.h>
#define EPS 0.00001
#define F(x) (5 * x * x) * log10(x) - 5.3
void bisect();
int kount = 1, intN;
float root = 1;
void main() {
  printf("\nSolution of Equation by Bisection Method. ");
  printf("\nEquation: ");
  printf("   (5*x*x) * log10(x) - 5.3 = 0");
  printf("\nEnter the number of iterations: ");
  scanf("%d", &intN);
  bisect();
}
void bisect() {
  float x1, x2, x3, func1, func2, func3;
  x3 = 1;
  do {
    func3 = F(x3);
    if (func3 > 0) {
      break;
    }
    x3++;
  } while (1);
  x2 = x3 - 1;
  do {
    func2 = F(x2);
    if (func2 < 0) {
      break;
    }
    x3--;
  } while (1);
  while (kount <= intN) {
    x1 = (x2 + x3) / 2.0;
    func1 = F(x1);
    if (func1 == 0) {
      root = x1;
    }
    if (func1 * func2 < 0) {
      x3 = x1;
    } else {
      x2 = x1;
      func2 = func1;
    }
    printf("\nIteration No. %d", kount);
    printf("     :     Root, x = %f", x1);
    if (fabs((x2 - x3) / x2) < EPS) {
      printf("\n\nTotal No. of Iterations:  %d", kount);
      printf("\nRoot, x = %f", x1);
      printf("\n\nThank you.\n");
      exit(0)
          ;
    }
    kount++;
  }
  printf("\n\nTotal No. of Iterations = %d", kount - 1);
  printf("\nRoot, x = %8.6f", x1);
  printf("\n\nThank you.\n");
}
```

## 2. To Find the Roots of an Equation Using the Regula Falsi Method


```c

#include <stdio.h>
#include <math.h>
#define EPS 0.00001
#define f(x) 3 * x *x *x + 5 * x *x + 4 * cos(x) - 2 * exp(x)
void falsePosition();
void main() {
  printf("\nSolution of Equation by False Position Method\n");
  printf("\nEquation :    ");
  printf("3*x*x*x + 5*x*x + 4*cos(x) - 2*exp(x) = 0");
  falsePosition();
}
void falsePosition()
{
  float fun1, fun2, fun3;
  float x1, x2, x3;
  int iterations;
  int i;
  printf("\nEnter the Number of Iterations: ");
  scanf("%d", &iterations);
  x2 = 0.0;
  do {
    fun2 = f(x2);
    if (fun2 > 0) {
      break;
    } else {
      x2 = x2 + 0.1;
    }
  } while (1);
  x1 = x2 - 0.1;
  fun1 = f(x1);
  printf("\nIteration No.\t\tx\t\tF(x)\n");
  i = 0;
  while (i < iterations) {
    x3 = x1 - ((x2 - x1) / (fun2 - fun1)) * fun1;
    fun3 = f(x3);
    if (fun1 * fun3 > 0) {
      x2 = x3;
      fun2 = fun3;
    } else {
      x1 = x3;
      fun1 = fun3;
    }
    printf("\n%d\t\t\t%f\t%f\n", i + 1, x3, fun3);
    if (fabs(fun3) <= EPS)
      break;
    i++;
  }
  printf("\n\nTotal No. of Iterations:  %d", i + 1);
  printf("\nRoot, x = %8.6f \n", x3);
  printf("\nThank you.\n");
}
```

## 3. To Find the Roots of an Equation Using Muller’s Method


```c

#include <stdio.h>
#include <math.h>
#define EPS 0.00001
float f(float x) { return (x * x * x) - (2 * x) - 5; }
main() {
  int i, itr, maxItr;
  float x[4], m, n, p, q, r;
  printf("\nSolution of Equation by Muller's Method.");
  printf("\nEquation: x*x*x - 2*x - 5 = 0  \n");
  printf("\n\n Enter the first initial guess: ");
  scanf("%f", &x[0]);
  printf("\nEnter the second initial guess: ");
  scanf("%f", &x[1]);
  printf("\nEnter the third initial guess: ");
  scanf("%f", &x[2])
      ;
  printf("\nEnter the maximum number of iterations: ");
  scanf("%d", &maxItr);
  for (itr = 1; itr <= maxItr; itr++) {
    m = (x[2] - x[1]) / (x[1] - x[0]);
    n = (x[2] - x[0]) / (x[1] - x[0]);
    p = f(x[0]) * m * m - f(x[1]) * n * n + f(x[2]) * (n + m);
    q = sqrt(
        (p * p - 4 * f(x[2]) * n * m * (f(x[0]) * m - f(x[1]) * n + f(x[2]))));
    if (p < 0)
      r = (2 * f(x[2]) * n) / (-p + q);
    else
      r = (2 * f(x[2]) * n) / (-p - q);
    x[3] = x[2] + r * (x[2] - x[1]);
    printf("Iteration No. : %d,      x = %8.6f\n", itr, x[3]);
    if (fabs(x[3] - x[2]) < EPS) {
      printf("\nTotal No. of Iterations: %d\n", itr);
      printf("\Root, x = %8.6f\n", x[3]);
      printf("Thank you.\n");
      return 0;
    }
    for (i = 0; i < 3; i++)
      x[i] = x[i + 1];
  }
  printf("\nSolution Doesn't Converge or Iterations are Insufficient.\n");
  printf("Thank you.\n");
  return (1);
}
```

## 4. To Find the Roots of an Equation Using the Newton Raphson Method


```c

#include<stdio.h>
#include<math.h>

#define EPS  0.00001
#define f(x) 17*x*x*x - 13*x*x - 7*x - 2973
#define df(x) 51*x*x - 26*x - 7

void newtonRaphson();

void main()
{
  printf ("\nSolution of Equation by Newton Raphson method.\n");
  printf ("\nEquation is: 17*x*x*x - 13*x*x - 7*x - 2973 = 0 \n\n");
  newtonRaphson();
}

void newtonRaphson()
{
  long float x1, x2, f1, f2, df;
  int i=1, iterations;
  float error;
  x2 = 0;
  do {
    f2 = f(x2);
    if (f2 > 0)
      break;
    x2 += 0.01;
  } while (1);
  x1 = x2 - 0.01;
  f1 = f(x1);
  printf("Enter the number of iterations: ");
  scanf(" %d",&iterations);
  x1 = (x1 + x2) / 2;
  while (i <= iterations) {
    f1 = f(x1);
    df = df(x1);
    x2 = x1 - (f1/df);
    printf("\nThe %d th approximation , x = %f", i, x2);
    error = fabs(x2 - x1);
    if(error < EPS)
      break;
    x1 = x2;
    i++;
  }
  if(error > EPS)
    printf("Solution Doesn't Converge or No. of Iterations Insufficient.");
  printf("\nRoot,  x = %8.6f ", x2);
  printf("\nThank you.\n");
}
```

## 5. To Construct the New Data Points Using Newton’s Forward Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  float ax[MAX], ay[MAX], diff[MAX][5];
  float nr = 1.0, dr = 1.0, x, p, h, yp;
  int terms, i, j, k;
  printf("\nInterpolation by Newton's Forward Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for (i = 0; i < terms - 1; i++)
    diff[i][1] = ay[i + 1] - ay[i];
  for (j = 2; j <= 4; j++)
    for (i = 0; i <= terms - j; i++)
      diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
  i = 0;
  do {
    i++;
  } while (ax[i] < x);
  i--;
  p = (x - ax[i]) / h;
  yp = ay[i];
  for (k = 1; k <= 4; k++) {
    nr *= p - k + 1;
    dr *= k;
    yp += (nr / dr) * diff[i][k];
  }
  printf("\nFor x = %6.2f,    y = %6.4f", x, yp);
  printf("\nThank you.\n");
}
```

## 6. To Construct the New Data Points Using Newton’s Backward Method of Interpolation


```c

#include <stdio.h>
#include <math.h>
#define MAX 20
void main() {
  int i, j, k, terms;
  float ax[MAX], ay[MAX], x, x0 = 0, y0, sum, h, store, p;
  float diff[MAX][5], y1, y2, y3, y4;
  printf("\nInterpolation by Newton's Backward Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms)
      ;
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for (i = 0; i < terms - 1; i++) {
    diff[i][1] = ay[i + 1] - ay[i];
  }
  for (j = 2; j <= 4; j++) {
    for (i = 0; i < terms - j; i++) {
      diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
    }
  }
  i = 0;
  while (!ax[i] > x) {
    i++;
  }
  x0 = ax[i];
  sum = 0;
  y0 = ay[i];
  store = 1;
  p = (x - x0) / h;
  sum = y0;
  for (k = 1; k <= 4; k++) {
    store = (store * (p - (k - 1))) / k;
    sum = sum + store * diff[i][k];
  }
  printf("\nFor x = %6.2f,    y = %6.4f", x, sum);
  printf("\nThank you.\n");
}
```

## 7. To Construct the New Data Points Using Gauss’s Forward Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  int i, j, terms;
  float ax[MAX], ay[MAX], x, y = 0, h, p;
  float diff[MAX][5], y1, y2, y3, y4;
  printf("\nInterpolation by Gauss's Forward Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for (i = 0; i < terms - 1; i++)
    diff[i][1] = ay[i + 1] - ay[i];
  for (j = 2; j <= 4; j++)
    for (i = 0; i < terms - j; i++)
      diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
  i = 0;
  do {
    i++;
  } while (ax[i] < x);
  i--;
  p = (x - ax[i]) / h;
  y1 = p * diff[i][1]
      ;
  y2 = p * (p - 1) * diff[i - 1][2] / 2;
  y3 = (p + 1) * p * (p - 1) * diff[i - 2][3] / 6;
  y4 = (p + 1) * p * (p - 1) * (p - 2) * diff[i - 3][4] / 24;
  y = ay[i] + y1 + y2 + y3 + y4;
  printf("\nFor x = %6.2f,    y = %6.4f ", x, y);
  printf("\nThank you.\n");
}
```

## 8. To Construct the New Data Points Using Gauss’s Backward Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  int i, j, terms;
  float ax[MAX], ay[MAX], x, y = 0, h, p;
  float diff[MAX][5], y1, y2, y3, y4;
  printf("\nInterpolation by Gauss's Backward Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for (i = 0; i < terms - 1; i++)
    diff[i][1] = ay[i + 1] - ay[i];
  for (j = 2; j <= 4; j++)
    for (i = 0; i < terms - j; i++)
      diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
  i = 0;
  do {
    i++;
  } while (ax[i] < x);
  i--;
  p = (x - ax[i]) / h;
  y1 = p * diff[i - 1][1];
  y2 = p * (p + 1) * diff[i - 1][2] / 2;
  y3 = (p + 1) * p * (p - 1) * diff[i - 2][3] / 6;
  y4 = (p + 2) * (p + 1) * p * (p - 1) * diff[i - 3][4] / 24;
  y = ay[i] + y1 + y2 + y3 + y4;
  printf("\nFor x = %6.2f,      y = %6.4f ", x, y);
  printf("\nThank you.\n");
}
```

## 9. To Construct the New Data Points Using Stirling’s Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  int i, j, terms;
  float ax[MAX], ay[MAX], x, y, h, p;
  float diff[MAX][5], y1, y2, y3, y4;
  printf("\nInterpolation by Stirling Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for (i = 0; i < terms - 1; i++)
    diff[i][1] = ay[i + 1] - ay[i];
  for (j = 2; j <= 4; j++)
    for (i = 0; i < terms - j; i++)
      diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
  i = 0;
  do {
    i++;
  } while (ax[i] < x);
  i--;
  p = (x - ax[i]) / h;
  y1 = p * (diff[i][1] + diff[i - 1][1]) / 2;
  y2 = p * p * diff[i - 1][2] / 2;
  y3 = p * (p * p - 1) * (diff[i - 1][3] + diff[i - 2][3]) / 6;
  y4 = p * p * (p * p - 1) * diff[i - 2][4] / 24;
  y = ay[i] + y1 + y2 + y3 + y4;
  printf("\n\nFor x = %6.2f,     y = %6.4f", x, y);
  printf("\nThank you. \n");
}
```
## 10. To Construct the New Data Points Using Bessel’s Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  int i, j, terms;
  float ax[MAX], ay[MAX], x, y, h, p;
  float diff[MAX][5], y1, y2, y3, y4;
  printf("\nImplementation of Interpolation by Bessel's Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i=0; i<terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i=0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for(i=0; i < terms-1; i++)
    diff[i][1] = ay[i+1] - ay[i];
  for(j=2; j <= 4; j++)
    for(i=0; i < terms-j; i++)
      diff[i][j] = diff[i+1][j-1] - diff[i][j-1];
  i = 0;
  do {
    i++;
  } while (ax[i] < x);
  i--;
  p = (x-ax[i])/h;
  y1 = p * (diff[i][1]);
  y2 = p * (p-1) * (diff[i][2] + diff[i-1][2])/4;
  y3 = p * (p-1) * (p-0.5) * (diff[i-1][3])/6;
  y4 = (p+1) * p * (p-1) * (p-2) * (diff[i-2][4] + diff[i-1][4])/48;
  y = ay[i] + y1 + y2 + y3 + y4;
  printf("\For x = %6.2f,     y = %6.4f ", x, y);
  printf("\nThank you.\n");
}
```

## 11. To Construct the New Data Points Using Laplace Everett’s Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  int i, j, terms;
  float ax[MAX], ay[MAX], x, y = 0, h, p, q;
  float diff[MAX][5], y1, y2, y3, y4, py1, py2, py3, py4;
  printf("\nInterpolation by Laplace Everett's Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  h = ax[1] - ax[0];
  for (i = 0; i < terms - 1; i++)
    diff[i][1] = ay[i + 1] - ay[i];
  for (j = 2; j <= 4; j++)
    for (i = 0; i < terms - j; i++)
      diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
  i = 0;
  do {
    i++;
  } while (ax[i] < x);
  i--;
  p = (x - ax[i]) / h;
  q = 1 - p;
  y1 = q * (ay[i]);
  y2 = q * (q * q - 1) * diff[i - 1][2] / 6;
  y3 = q * (q * q - 1) * (q * q - 4) * (diff[i - 2][4]) / 120;
  py1 = p * ay[i + 1];
  py2 = p * (p * p - 1) * diff[i][2] / 6;
  py3 = p * (p * p - 1) * (p * p - 4) * (diff[i - 1][4]) / 120;
  y = y1 + y2 + y3 + y4 + py1 + py2 + py3;
  printf("\nFor x = %6.2f,     y = %6.4f ", x, y);
  printf("\nThank you.\n");
}
```

## 12. To Construct the New Data Points Using Lagrange’s Method of Interpolation


```c

#include <stdio.h>
#define MAX 20
void main() {
  int i, j, terms;
  float ax[MAX], ay[MAX], nr, dr, x, y = 0;
  printf("\nImplementation of Interpolation by Lagrange's Method.");
  printf("\nEnter the number of terms (Maximum 20): ");
  scanf("%d", &terms);
  printf("\nEnter the values of x upto 2 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of x%d: ", i + 1);
    scanf("%f", &ax[i]);
  }
  printf("\nNow enter the values of y upto 4 decimal points.\n");
  for (i = 0; i < terms; i++) {
    printf("Enter the value of y%d: ", i + 1);
    scanf("%f", &ay[i]);
  }
  printf("\nEnter the value of x for which the value of y is wanted: ");
  scanf("%f", &x);
  for (i = 0; i < terms; i++) {
    nr = 1;
    dr = 1;
    for (j = 0; j < terms; j++) {
      if (j != i) {
        nr = nr * (x - ax[j]);
        dr = dr * (ax[i] - ax[j]);
      }
    }
    y = y + ((nr / dr) * ay[i]);
  }
  printf("\nFor x = %6.2f,    y = %6.4f", x, y);
  printf("\nThank you.\n");
}
```

## 13. To Compute the Value of Integration Using Trapezoidal Method of Numerical Integration


```c

#include <stdio.h>
#define MAX 50
float trapezoid(float x) { return (1 / (1 + x * x)); }
void main() {
  int i, num;
  float a, b, h, x[MAX], y[MAX], sumOdd, sumEven, result;
  printf("\nTrapezoidal Method of Numerical Integration.");
  printf("\nIntegrand:  f(x) = 1/(1+x*x) \n");
  printf("\nEnter the lower limit of integration, a :  ");
  scanf("%f", &a);
  printf("Enter the upper limit of integration, b :  ");
  scanf("%f", &b);
  printf("Enter the width of trapezium, h :  ");
  scanf("%f", &h);
  num = (b - a) / h;
  if (num % 2 == 1)
    num = num + 1;
  h = (b - a) / num;
  printf("Refined value of h, the width of trapezium : %5.3f", h);
  printf("\nRefined value of num, the number of trapaziums : %d\n", num);
  for (i = 0; i <= num; i++) {
    x[i] = a + i * h;
    y[i] = trapezoid(x[i]);
  }
  sumOdd = 0;
  sumEven = 0;
  for (i = 1; i < num; i++) {
    if (i % 2 == 1)
      sumOdd = sumOdd + y[i];
    else
      sumEven = sumEven + y[i];
  }
  result = h / 3 * (y[0] + y[num] + 4 * sumOdd + 2 * sumEven);
  printf("\nValue of Integration : %5.3f", result);
  printf("\nThank you.\n");
}
```

## 14. To Compute the Value of Integration Using Simpson’s 3 8th Method of Numerical Integration


```c

#include <stdio.h>
#define MAX 50
float simpson(float x) { return (1 / (1 + x * x)); }
void main() {
  int i, j, num;
  float a, b, h, x[MAX], y[MAX], sum, result = 1;
  printf("\nSimpson's 3/8th Method of Computation of Integral.");
  printf("\nIntegrand:  f(x) = 1/(1+x*x) \n");
  printf("\nEnter the lower limit of integration, a :  ");
  scanf("%f", &a);
  printf("Enter the upper limit of integration, b :  ");
  scanf("%f", &b);
  printf("Enter the number of subintervals, num :  ");
  scanf("%d", &num);
  h = (b - a) / num;
  sum = 0;
  sum = simpson(a) + simpson(b);
  for (i = 1; i < num; i++) {
    if (i % 3 == 0) {
      sum += 2 * simpson(a + i * h);
    } else {
      sum += 3 * simpson(a + i * h);
    }
  }
  result = sum * 3 * h / 8;
  printf("\nValue of Integration : %5.3f", result);
  printf("\nThank you.\n");
}
```

## 15. To Compute the Value of Integration Using Simpson’s 1 3rd Method of Numerical Integration


```c

#include <stdio.h>
#define MAX 50
float simpson(float x) { return (1 / (1 + x * x)); }
void main() {
  int i, j, num;
  float a, b, h, x[MAX], y[MAX], sum, result = 1;
  printf("\nSimpson's 1/3rd Method of Computation of Integral.");
  printf("\nIntegrand:  f(x) = 1/(1+x*x) \n");
  printf("\nEnter the lower limit of integration, a :  ");
  scanf("%f", &a);
  printf("Enter the upper limit of integration, b :  ");
  scanf("%f", &b);
  printf("Enter the number of subintervals, num :  ");
  scanf("%d", &num);
  h = (b - a) / num;
  sum = 0;
  sum = simpson(a) + 4 * simpson(a + h) + simpson(b);
  for (i = 3; i < num; i += 2) {
    sum += 2 * simpson(a + (i - 1) * h) + 4 * simpson(a + i * h);
  }
  result = sum * h / 3;
  printf("\nValue of Integration : %5.3f", result);
  printf("\nThank you.\n");
}
```

## 16. To Solve a Differential Equation Using Modified Euler’s Method


```c

#include <stdio.h>
#define MAX 50
float euler(float p, float q) {
  float r;
  r = p * p + q;
  return (r);
}
void main() {
  int i = 1, j, k;
  float x[MAX], y[MAX], store1[MAX], store2[MAX];
  float b, h, u, v, w;
  printf("\nModified Euler's Method to Solve a Differential Equation. ");
  printf("\nFunction for calculation of slope: y' = x * x + y\n");
  printf("Enter the initial value of the variable x, x0: ");
  scanf("%f", &x[0]);
  printf("Enter the final value of the variable x, xn: ");
  scanf("%f", &b);
  printf("Enter the initial value of the variable y, y0: ");
  scanf("%f", &y[0]);
  printf("Enter the value of subinterval, h: ");
  scanf("%f", &h);
  store2[0] = y[0];
  while (x[i - 1] < b) {
    w = 100.0;
    x[i] = x[i - 1] + h;
    store1[i] = euler(x[i - 1], y[i - 1]);
    k = 0;
    while (w > 0.0001) {
      u = euler(x[i], store2[k]);
      v = (store1[i] + u) / 2;
      store2[k + 1] = y[i - 1] + v * h;
      w = store2[k] - store2[k + 1];
      w = fabs(w);
      k = k + 1;
    }
    y[i] = store2[k];
    i = i + 1;
  }
  printf("\nThe Values of X and Y are: \n");
  printf("\nX-values        Y-values\n");
  for (j = 0; j < i; j++) {
    printf("%f        %f\n", x[j], y[j]);
  }
  printf("\nThank you.\n");
}
```

## 17. To Solve a Differential Equation Using Runge Kutta Method


```c

#include<stdio.h>

#define F(x,y) (2*x-y)/(x+y)

void main()
{
  int i, n;
  float x0, y0, h, xn, k1, k2, k3, k4, x, y, k;
  printf("\nRunge Kutta Method to Solve a Differential Equation.");
  printf("\nEquation: y' = (2*x-y)/(x+y) ");
  printf("\nEnter initial value of the variable x, x0: ");
  scanf("%f", &x0);
  printf("Enter initial value of the variable y, y0: ");
  scanf("%f", &y0);
  printf("Enter final value of the variable x, xn: ");
  scanf("%f", &xn);
  printf("Enter the subinterval, h: ");
  scanf("%f", &h);
  n = (xn - x0)/h;
  x = x0;
  y = y0;
  i = 0;
  while (i <= n) {
    k1 = h * F(x,y);
    k2 = h * F(x+h/2.0, y+k1/2.0);
    k3 = h * F(x+h/2.0, y+k2/2.0);
    k4 = h * F(x+h, y+k3);
    k = (k1 + (k2+k3) * 2.0 + k4) / 6.0;
    printf("\nX = %f  Y = %f", x, y);
    x = x + h;
    y = y + k;
    i = i + 1;
  }
  printf("\n\nThank you.\n");
}
```
