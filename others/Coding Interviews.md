# Coding Interviews

- [001. PalindromeNumber](#001-palindromenumber)
- [002. AssignmentOperator](#002-assignmentoperator)
- [004. Scheduler](#004-scheduler)
- [005. Duplication](#005-duplication)
- [006. Duplication](#006-duplication)
- [007. FindInSortedMatrix](#007-findinsortedmatrix)
- [008. FindInPatiallySortedMatrix](#008-findinpatiallysortedmatrix)
- [009. ReplaceBlanks](#009-replaceblanks)
- [010. MergeSortedArrays](#010-mergesortedarrays)
- [011. SimpleRegularExpression](#011-simpleregularexpression)
- [012. NumericStrings](#012-numericstrings)
- [013. PrintListsReversely](#013-printlistsreversely)
- [014. SortLists](#014-sortlists)
- [list](#list)
- [list](#list)

## 001. PalindromeNumber


```c

#include <stdio.h>
#include <string.h>
int IsPalindrome(const char *const string) {
  int palindrome = 1;
  if (string != NULL) {
    int length = strlen(string);
    int half = length >> 1;
    int i;
    for (i = 0; i < half; ++i) {
      if (string[i] != string[length - 1 - i]) {
        palindrome = 0;
        break;
      }
    }
  }
  return palindrome;
}
#define NUMBER_LENGTH 20
int IsPalindrome_solution1(unsigned int number) {
  char string[NUMBER_LENGTH];
  sprintf(string, "%d", number);
  return IsPalindrome(string);
}
int IsPalindrome_solution2(unsigned int number) {
  int reversed = 0;
  int copy = number;
  while (number != 0) {
    reversed = reversed * 10 + number % 10;
    number /= 10;
  }
  return (reversed == copy) ? 1 : 0;
}
void Test(char *testName, int number, int expected) {
  if (testName != NULL)
    printf("%s begins: ", testName);
  if (IsPalindrome_solution1(number) == expected)
    printf("solution 1 passed; ");
  else
    printf("solution 1 FAILED; ");
  if (IsPalindrome_solution2(number) == expected)
    printf("solution 2 passed.\n");
  else
    printf("solution 2 FAILED.\n");
}
int main(int argc, char *argv[]) {
  Test("Test1", 5, 1);
  Test("Test2", 33, 1);
  Test("Test3", 242, 1);
  Test("Test4", 2332, 1);
  Test("Test5", 0, 1);
  Test("Test6", 32, 0);
  Test("Test7", 233, 0);
  Test("Test8", 2331, 0);
  Test("Test9", 2322, 0);
  return 0;
}
```

## 002. AssignmentOperator


```cpp

#include <stdio.h>
#include <string.h>
class CMyString {
public:
  CMyString(char *pData = NULL);
  CMyString(const CMyString &str);
  ~CMyString(void);
  CMyString &operator=(const CMyString &str);
  void Print();
private:
  char *m_pData;
};
CMyString::CMyString(char *pData) {
  if (pData == NULL) {
    m_pData = new char[1];
    m_pData[0] = '\0';
  } else {
    int length = strlen(pData);
    m_pData = new char[length + 1];
    strcpy(m_pData, pData);
  }
}
CMyString::CMyString(const CMyString &str) {
  int length = strlen(str.m_pData);
  m_pData = new char[length + 1];
  strcpy(m_pData, str.m_pData);
}
CMyString::~CMyString() { delete[] m_pData; }
CMyString &CMyString::operator=(const CMyString &str) {
  if (this == &str)
    return *this;
  delete[] m_pData;
  m_pData = NULL;
  m_pData = new char[strlen(str.m_pData) + 1];
  strcpy(m_pData, str.m_pData);
  return *this;
}
void CMyString::Print() { printf("%s", m_pData); }
void Test1() {
  printf("Test1 begins:\n");
  char *text = "Hello world";
  CMyString str1(text);
  CMyString str2;
  str2 = str1;
  printf("The expected result is: %s.\n", text);
  printf("The actual result is: ");
  str2.Print();
  printf(".\n\n");
}
void Test2() {
  printf("Test2 begins:\n");
  char *text = "Hello world";
  CMyString str1(text);
  str1 = str1;
  printf("The expected result is: %s.\n", text);
  printf("The actual result is: ");
  str1.Print();
  printf(".\n\n");
}
void Test3() {
  printf("Test3 begins:\n");
  char *text = "Hello world";
  CMyString str1(text);
  CMyString str2, str3;
  str3 = str2 = str1;
  printf("The expected result is: %s.\n", text);
  printf("The actual result is: ");
  str2.Print();
  printf(".\n");
  printf("The expected result is: %s.\n", text);
  printf("The actual result is: ");
  str3.Print();
  printf(".\n\n");
}
int main(int argc, char *argv[]) {
  Test1();
  Test2();
  Test3();
  return 0;
}
```

## 004. Scheduler


```java

package tester;
public class Scheduler {
    public class SimpleThread extends Thread {
        private int value;
        public SimpleThread(int num) {
            this.value = num;
            start();
        }
        public void run() {
            while (true) {
                synchronized (this) {
                    try {
                        wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    System.out.print(value + " ");
                }
            }
        }
    }
    static final int COUNT = 3;
    static final int SLEEP = 37;
    public static void main(String args[]) {
        Scheduler scheduler = new Scheduler();
        SimpleThread threads[] = new SimpleThread[COUNT];
        for (int i = 0; i < COUNT; ++i)
            threads[i] = scheduler.new SimpleThread(i + 1);
        int index = 0;
        while (true) {
            synchronized (threads[index]) {
                threads[index].notify();
            }
            try {
                Thread.sleep(SLEEP);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            index = (++index) % COUNT;
        }
    }
}
```

## 005. Duplication


```java

public class Duplication {
    public static int duplicate(int numbers[]) {
        int length = numbers.length;
        int sum1 = 0;
        for (int i = 0; i < length; ++i) {
            if (numbers[i] < 0 || numbers[i] > length - 2)
                throw new IllegalArgumentException("Invalid numbers.");
            sum1 += numbers[i];
        }
        int sum2 = ((length - 1) * (length - 2)) >> 1;
        return sum1 - sum2;
    }
    // ================= Test Code =================
    private static void test(String testName, int numbers[], int expected, boolean invalidArgument) {
        System.out.print(testName + " begins: ");
        try {
            int duplication = duplicate(numbers);
            if (!invalidArgument && duplication == expected)
                System.out.print("Passed.\n");
            else
                System.out.print("FAILED.\n");
        } catch (IllegalArgumentException e) {
            if (invalidArgument)
                System.out.print("Passed.\n");
            else
                System.out.print("FAILED.\n");
        }
    }
    private static void test1() {
        int numbers[] = { 2, 1, 3, 1, 0 };
        test("Test1", numbers, 1, false);
    }
    private static void test2() {
        int numbers[] = { 2, 0, 3, 1, 0 };
        test("Test2", numbers, 0, false);
    }
    private static void test3() {
        int numbers[] = { 2, 0, 4, 3, 1, 4 };
        test("Test3", numbers, 4, false);
    }
    private static void test4() {
        int numbers[] = { 2, 1, 3, 0, 4 };
        test("Test4", numbers, 0, true);
    }
    private static void test5() {
        int numbers[] = { 2, 1, 3, 0, -1 };
        test("Test5", numbers, 0, true);
    }
    private static void test6() {
        int numbers[] = { 0 };
        test("Test6", numbers, 0, true);
    }
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
    }
}
```

## 006. Duplication


```java

public class Duplication {
    public static int duplicate(int numbers[]) {
        int length = numbers.length;
        for (int i = 0; i < length; ++i) {
            if (numbers[i] < 0 || numbers[i] > length - 1)
                throw new IllegalArgumentException("Invalid numbers.");
        }
        for (int i = 0; i < length; ++i) {
            while (numbers[i] != i) {
                if (numbers[i] == numbers[numbers[i]]) {
                    return numbers[i];
                }
                // swap numbers[i] and numbers[numbers[i]]
                int temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;
            }
        }
        throw new IllegalArgumentException("No duplications.");
    }
    // ================= Test Code =================
    private static boolean contains(int array[], int number) {
        for (int i = 0; i < array.length; ++i) {
            if (array[i] == number)
                return true;
        }
        return false;
    }
    private static void test(String testName, int numbers[], int expected[], boolean invalidArgument) {
        System.out.print(testName + " begins: ");
        try {
            int duplication = duplicate(numbers);
            if (!invalidArgument && contains(expected, duplication))
                System.out.print("Passed.\n");
            else
                System.out.print("FAILED.\n");
        } catch (IllegalArgumentException e) {
            if (invalidArgument)
                System.out.print("Passed.\n");
            else
                System.out.print("FAILED.\n");
        }
    }
    // The duplicated number is the smallest number
    private static void test1() {
        int numbers[] = { 2, 1, 3, 1, 4 };
        int duplications[] = { 1 };
        test("Test1", numbers, duplications, false);
    }
    // The duplicated number is the greatest number
    private static void test2() {
        int numbers[] = { 2, 4, 3, 1, 4 };
        int duplications[] = { 4 };
        test("Test2", numbers, duplications, false);
    }
    // There are more than one duplicated number
    private static void test3() {
        int numbers[] = { 2, 4, 2, 1, 4 };
        int duplications[] = { 2, 4 };
        test("Test3", numbers, duplications, false);
    }
    // no duplicated numbers
    private static void test4() {
        int numbers[] = { 2, 1, 3, 0, 4 };
        int duplications[] = {};
        test("Test4", numbers, duplications, true);
    }
    // no duplicated numbers
    private static void test5() {
        int numbers[] = { 2, 1, 3, 5, 4 };
        int duplications[] = {};
        test("Test5", numbers, duplications, true);
    }
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
    }
}
```

## 007. FindInSortedMatrix


```java

public class FindInSortedMatrix {
    public static boolean find(int matrix[][], int value) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int start = 0;
        int end = rows * cols - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            int row = mid / cols;
            int col = mid % cols;
            int v = matrix[row][col];
            if (v == value)
                return true;
            if (v > value)
                end = mid - 1;
            else
                start = mid + 1;
        }
        return false;
    }
    private static void test(String testName, int matrix[][], int value, boolean expected) {
        System.out.print(testName + " begins: ");
        if (find(matrix, value) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    // 1 3 5
    // 7 9 11
    // 13 15 17
    // The value to be found, 7, is in the matrix;
    private static void test1() {
        int matrix[][] = { { 1, 3, 5 }, { 7, 9, 11 }, { 13, 15, 17 } };
        test("Test1", matrix, 7, true);
    }
    // 1 3 5
    // 7 9 11
    // 13 15 17
    // The value to be found, 1, is in the matrix;
    private static void test2() {
        int matrix[][] = { { 1, 3, 5 }, { 7, 9, 11 }, { 13, 15, 17 } };
        test("Test2", matrix, 1, true);
    }
    // 1 3 5
    // 7 9 11
    // 13 15 17
    // The value to be found, 17, is in the matrix;
    private static void test3() {
        int matrix[][] = { { 1, 3, 5 }, { 7, 9, 11 }, { 13, 15, 17 } };
        test("Test3", matrix, 17, true);
    }
    // 1 3 5
    // 7 9 11
    // 13 15 17
    // The value to be found, 6, is not in the matrix;
    private static void test4() {
        int matrix[][] = { { 1, 3, 5 }, { 7, 9, 11 }, { 13, 15, 17 } };
        test("Test4", matrix, 6, false);
    }
    // 1 3 5
    // 7 9 11
    // 13 15 17
    // The value to be found, 0, is not in the matrix;
    private static void test5() {
        int matrix[][] = { { 1, 3, 5 }, { 7, 9, 11 }, { 13, 15, 17 } };
        test("Test5", matrix, 0, false);
    }
    // 1 3 5
    // 7 9 11
    // 13 15 17
    // The value to be found, 18, is not in the matrix;
    private static void test6() {
        int matrix[][] = { { 1, 3, 5 }, { 7, 9, 11 }, { 13, 15, 17 } };
        test("Test6", matrix, 18, false);
    }
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
    }
}
```

## 008. FindInPatiallySortedMatrix


```java

public class FindInPatiallySortedMatrix {
    // ================= Solution 1 =================
    public static boolean find_solution1(int matrix[][], int value) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        return findCore(matrix, value, 0, 0, rows - 1, cols - 1);
    }
    private static boolean findCore(int matrix[][], int value, int row1, int col1, int row2, int col2) {
        if (value < matrix[row1][col1] || value > matrix[row2][col2])
            return false;
        if (value == matrix[row1][col1] || value == matrix[row2][col2])
            return true;
        int copyRow1 = row1, copyRow2 = row2;
        int copyCol1 = col1, copyCol2 = col2;
        int midRow = (row1 + row2) / 2;
        int midCol = (col1 + col2) / 2;
        // find the last element less than value on diagonal
        while ((midRow != row1 || midCol != col1) && (midRow != row2 || midCol != col2)) {
            if (value == matrix[midRow][midCol])
                return true;
            if (value < matrix[midRow][midCol]) {
                row2 = midRow;
                col2 = midCol;
            } else {
                row1 = midRow;
                col1 = midCol;
            }
            midRow = (row1 + row2) / 2;
            midCol = (col1 + col2) / 2;
        }
        // find value in two sub-matrices
        boolean found = false;
        if (midRow < matrix.length - 1)
            found = findCore(matrix, value, midRow + 1, copyCol1, copyRow2, midCol);
        if (!found && midCol < matrix[0].length - 1)
            found = findCore(matrix, value, copyRow1, midCol + 1, midRow, copyCol2);
        return found;
    }
    // ================= Solution 2 =================
    public static boolean find_solution2(int matrix[][], int value) {
        boolean found = false;
        int row = 0;
        int col = matrix[0].length - 1;
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == value) {
                found = true;
                break;
            }
            if (matrix[row][col] > value)
                --col;
            else
                ++row;
        }
        return found;
    }
    // ================= Test Code =================
    private static void test(String testName, int matrix[][], int value, boolean expected) {
        System.out.print(testName + " begins: ");
        if (find_solution1(matrix, value) == expected)
            System.out.print("Solution1 passed; ");
        else
            System.out.print("Solution1 FAILED; ");
        if (find_solution2(matrix, value) == expected)
            System.out.print("Solution2 passed.\n");
        else
            System.out.print("Solution2 FAILED.\n");
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // The value to be found, 7, is in the matrix;
    private static void test1() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 } };
        test("Test1", matrix, 7, true);
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // The value to be found, 5, is not in the matrix;
    private static void test2() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 } };
        test("Test2", matrix, 5, false);
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // The value to be found, 1, is the minimum in the matrix
    private static void test3() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 } };
        test("Test3", matrix, 1, true);
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // The value to be found, 15, is the maximum in the matrix
    private static void test4() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 } };
        test("Test4", matrix, 15, true);
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // The value to be found, 0, is less than the minimum in the matrix
    private static void test5() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 } };
        test("Test5", matrix, 0, false);
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // The value to be found, 16, is greater than the maximum in the matrix
    private static void test6() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 } };
        test("Test6", matrix, 16, false);
    }
    // 1 2 8 9 14 15
    // 2 4 9 12 16 23
    // 4 7 10 13 21 25
    // 6 8 11 15 23 27
    // The value to be found, 21, is in the matrix
    private static void test7() {
        int matrix[][] = { { 1, 2, 8, 9, 14, 15 }, { 2, 4, 9, 12, 16, 23 }, { 4, 7, 10, 13, 21, 25 },
                { 6, 8, 11, 15, 23, 27 } };
        test("Test7", matrix, 21, true);
    }
    // 1 2 8 9
    // 2 4 9 12
    // 4 7 10 13
    // 6 8 11 15
    // 8 10 13 20
    // 12 13 17 25
    // 19 22 25 30
    // The value to be found, 18, is not in the matrix
    private static void test8() {
        int matrix[][] = { { 1, 2, 8, 9 }, { 2, 4, 9, 12 }, { 4, 7, 10, 13 }, { 6, 8, 11, 15 }, { 8, 10, 13, 20 },
                { 12, 13, 17, 25 }, { 19, 22, 25, 30 } };
        test("Test8", matrix, 18, false);
    }
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
        test7();
        test8();
    }
}
```

## 009. ReplaceBlanks


```c

#include <stdio.h>
#include <string.h>
/*capacity is total capacity of a string, which is longer than its actual
 * length*/
void ReplaceBlank(char string[], int capacity) {
  int originalLength, numberOfBlank, newLength;
  int i, indexOfOriginal, indexOfNew;
  if (string == NULL || capacity <= 0)
    return;
  /*originalLength is the actual length of string*/
  originalLength = numberOfBlank = i = 0;
  while (string[i] != '\0') {
    ++originalLength;
    if (string[i] == ' ')
      ++numberOfBlank;
    ++i;
  }
  /*newLength is the length of the replaced string*/
  newLength = originalLength + numberOfBlank * 2;
  if (newLength > capacity)
    return;
  indexOfOriginal = originalLength;
  indexOfNew = newLength;
  while (indexOfOriginal >= 0 && indexOfNew > indexOfOriginal) {
    if (string[indexOfOriginal] == ' ') {
      string[indexOfNew--] = '0';
      string[indexOfNew--] = '2';
      string[indexOfNew--] = '%';
    } else {
      string[indexOfNew--] = string[indexOfOriginal];
    }
    --indexOfOriginal;
  }
}
void Test(char *testName, char string[], int length, char expected[]) {
  if (testName != NULL)
    printf("%s begins: ", testName);
  ReplaceBlank(string, length);
  if (expected == NULL && string == NULL)
    printf("passed.\n");
  else if (expected == NULL && string != NULL)
    printf("failed.\n");
  else if (strcmp(string, expected) == 0)
    printf("passed.\n");
  else
    printf("failed.\n");
}
#define LENGTH 100
// A blank is inside a sentence
void Test1() {
  char string[LENGTH] = "hello world";
  Test("Test1", string, LENGTH, "hello%20world");
}
// A blank is at the beginning of a sentence
void Test2() {
  char string[LENGTH] = " helloworld";
  Test("Test2", string, LENGTH, "%20helloworld");
}
// A blank is at the end of a sentence
void Test3() {
  char string[LENGTH] = "helloworld ";
  Test("Test3", string, LENGTH, "helloworld%20");
}
// Two adjcent blanks
void Test4() {
  char string[LENGTH] = "hello  world";
  Test("Test4", string, LENGTH, "hello%20%20world");
}
// NULL
void Test5() { Test("Test5", NULL, 0, NULL); }
// Empty string
void Test6() {
  char string[LENGTH] = "";
  Test("Test6", string, LENGTH, "");
}
// Input string only has a blank
void Test7() {
  char string[LENGTH] = " ";
  Test("Test7", string, LENGTH, "%20");
}
// No blanks in the input
void Test8() {
  char string[LENGTH] = "helloworld";
  Test("Test8", string, LENGTH, "helloworld");
}
// Input string only has blanks
void Test9() {
  char string[LENGTH] = "   ";
  Test("Test9", string, LENGTH, "%20%20%20");
}
// Multiple blanks among words
void Test10() {
  char string[LENGTH] = "Multiple blanks among words";
  Test("Test10", string, LENGTH, "Multiple%20blanks%20among%20words");
}
int main(int argc, char *argv[]) {
  Test1();
  Test2();
  Test3();
  Test4();
  Test5();
  Test6();
  Test7();
  Test8();
  Test9();
  Test10();
  return 0;
}
```

## 010. MergeSortedArrays


```c

#include <stdio.h>
// Supposing there is enough memory at the end of array1,
// in order to accommodate numbers in array2
void merge(int *array1, int length1, int *array2, int length2) {
  int index1, index2, indexMerged;
  if (array1 == NULL || array2 == NULL)
    return;
  index1 = length1 - 1;
  index2 = length2 - 1;
  indexMerged = length1 + length2 - 1;
  while (index1 >= 0 && index2 >= 0) {
    if (array1[index1] >= array2[index2])
      array1[indexMerged--] = array1[index1--];
    else
      array1[indexMerged--] = array2[index2--];
  }
  while (index2 >= 0)
    array1[indexMerged--] = array2[index2--];
}
// ==================== Test Code ====================
void Test(char *testName, int *array1, int length1, int *array2, int length2,
          int *expected) {
  int i;
  if (testName != NULL)
    printf("%s begins: ", testName);
  merge(array1, length1, array2, length2);
  if (expected != NULL && array1 != NULL && array2 != NULL) {
    for (i = 0; i < length1 + length2; ++i) {
      if (array1[i] != expected[i])
        break;
    }
  }
  if ((expected == NULL && (array1 == NULL || array2 == NULL)) ||
      (i == length1 + length2))
    printf("passed.\n");
  else
    printf("FAILED.\n");
}
// no duplicated numbers
void Test1() {
  int array1[20] = {1, 3, 5, 7, 9};
  int array2[] = {2, 4, 6, 8, 10};
  int expected[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  Test("Test1", array1, 5, array2, 5, expected);
}
// no duplicated numbers
void Test2() {
  int array1[20] = {2, 4, 6, 8, 10};
  int array2[] = {1, 3, 5, 7, 9};
  int expected[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  Test("Test2", array1, 5, array2, 5, expected);
}
// duplicated arrays
void Test3() {
  int array1[20] = {2, 4, 6, 8, 10};
  int array2[] = {2, 4, 6, 8, 10};
  int expected[] = {2, 2, 4, 4, 6, 6, 8, 8, 10, 10};
  Test("Test3", array1, 5, array2, 5, expected);
}
// numbers in array1 are less than numbers in array2
void Test4() {
  int array1[20] = {1, 2, 3, 4, 5};
  int array2[] = {6, 7, 8, 9, 10};
  int expected[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  Test("Test4", array1, 5, array2, 5, expected);
}
// numbers in array1 are greater than numbers in array2
void Test5() {
  int array1[20] = {6, 7, 8, 9, 10};
  int array2[] = {1, 2, 3, 4, 5};
  int expected[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  Test("Test5", array1, 5, array2, 5, expected);
}
// numbers in array1 are greater than numbers in array2
void Test6() { Test("Test6", NULL, 0, NULL, 0, NULL); }
int main(int argc, char *argv[]) {
  Test1();
  Test2();
  Test3();
  Test4();
  Test5();
  Test6();
}
```

## 011. SimpleRegularExpression


```cpp

#include <stdio.h>
bool matchCore(char *string, char *pattern);
bool match(char *string, char *pattern) {
  if (string == NULL || pattern == NULL)
    return false;
  return matchCore(string, pattern);
}
bool matchCore(char *string, char *pattern) {
  if (*string == '\0' && *pattern == '\0')
    return true;
  if (*string != '\0' && *pattern == '\0')
    return false;
  if (*(pattern + 1) == '*') {
    if (*pattern == *string || (*pattern == '.' && *string != '\0'))
      // move on the next state
      return matchCore(string + 1, pattern + 2)
             // stay on the current state
             || matchCore(string + 1, pattern)
             // ignore a '*'
             || matchCore(string, pattern + 2);
    else
      // ignore a '*'
      return matchCore(string, pattern + 2);
  }
  if (*string == *pattern || (*pattern == '.' && *string != '\0'))
    return matchCore(string + 1, pattern + 1);
  return false;
}
// ==================== Test Code ====================
void Test(char *testName, char *string, char *pattern, bool expected) {
  if (testName != NULL)
    printf("%s begins: ", testName);
  if (match(string, pattern) == expected)
    printf("Passed.\n");
  else
    printf("FAILED.\n");
}
int main(int argc, char *argv[]) {
  Test("Test01", "", "", true);
  Test("Test02", "", ".*", true);
  Test("Test03", "", ".", false);
  Test("Test04", "", "c*", true);
  Test("Test05", "a", ".*", true);
  Test("Test06", "a", "a.", false);
  Test("Test07", "a", "", false);
  Test("Test08", "a", ".", true);
  Test("Test09", "a", "ab*", true);
  Test("Test10", "a", "ab*a", false);
  Test("Test11", "aa", "aa", true);
  Test("Test12", "aa", "a*", true);
  Test("Test13", "aa", ".*", true);
  Test("Test14", "aa", ".", false);
  Test("Test15", "ab", ".*", true);
  Test("Test16", "ab", ".*", true);
  Test("Test17", "aaa", "aa*", true);
  Test("Test18", "aaa", "aa.a", false);
  Test("Test19", "aaa", "a.a", true);
  Test("Test20", "aaa", ".a", false);
  Test("Test21", "aaa", "a*a", true);
  Test("Test22", "aaa", "ab*a", false);
  Test("Test23", "aaa", "ab*ac*a", true);
  Test("Test24", "aaa", "ab*a*c*a", true);
  Test("Test25", "aaa", ".*", true);
  Test("Test26", "aab", "c*a*b", true);
  Test("Test27", "aaca", "ab*a*c*a", true);
  Test("Test28", "aaba", "ab*a*c*a", false);
  Test("Test29", "bbbba", ".*a*a", true);
  Test("Test30", "bcbbabab", ".*a*a", false);
  return 0;
}
```

## 012. NumericStrings


```cpp

#include <stdio.h>
void scanDigits(char **string);
bool isExponential(char **string);
bool isNumeric(char *string) {
  if (string == NULL)
    return false;
  if (*string == '+' || *string == '-')
    ++string;
  if (*string == '\0')
    return false;
  bool numeric = true;
  scanDigits(&string);
  if (*string != '\0') {
    // for floats
    if (*string == '.') {
      ++string;
      scanDigits(&string);
      if (*string == 'e' || *string == 'E')
        numeric = isExponential(&string);
    }
    // for integers
    else if (*string == 'e' || *string == 'E')
      numeric = isExponential(&string);
    else
      numeric = false;
  }
  return numeric && *string == '\0';
}
void scanDigits(char **string) {
  while (**string != '\0' && **string >= '0' && **string <= '9')
    ++(*string);
}
bool isExponential(char **string) {
  if (**string != 'e' && **string != 'E')
    return false;
  ++(*string);
  if (**string == '+' || **string == '-')
    ++(*string);
  if (**string == '\0')
    return false;
  scanDigits(string);
  return (**string == '\0') ? true : false;
}
// ==================== Test Code ====================
void Test(char *testName, char *string, bool expected) {
  if (testName != NULL)
    printf("%s begins: ", testName);
  if (isNumeric(string) == expected)
    printf("Passed.\n");
  else
    printf("FAILED.\n");
}
int main(int argc, char *argv[]) {
  Test("Test1", "100", true);
  Test("Test2", "123.45e+6", true);
  Test("Test3", "+500", true);
  Test("Test4", "5e2", true);
  Test("Test5", "3.1416", true);
  Test("Test6", "600.", true);
  Test("Test7", "-.123", true);
  Test("Test8", "-1E-16", true);
  Test("Test9", "1.79769313486232E+308", true);
  printf("\n\n");
  Test("Test10", "12e", false);
  Test("Test11", "1a3.14", false);
  Test("Test12", "1+23", false);
  Test("Test13", "1.2.3", false);
  Test("Test14", "+-5", false);
  Test("Test15", "12e+5.4", false);
  return 0;
}
```

## 013. PrintListsReversely


```cpp

// C:\Codes\Codes\C++\list.cpp
#include <stdio.h>
#include "list.h"
#include <stack>
void PrintListReversingly_Iteratively(ListNode *pHead) {
  std::stack<ListNode *> nodes;
  ListNode *pNode = pHead;
  while (pNode != NULL) {
    nodes.push(pNode);
    pNode = pNode->m_pNext;
  }
  while (!nodes.empty()) {
    pNode = nodes.top();
    printf("%d\t", pNode->m_nValue);
    nodes.pop();
  }
}
void PrintListReversingly_Recursively(ListNode *pHead) {
  if (pHead != NULL) {
    if (pHead->m_pNext != NULL) {
      PrintListReversingly_Recursively(pHead->m_pNext);
    }
    printf("%d\t", pHead->m_nValue);
  }
}
// ==================== Test Code ====================
void Test(ListNode *pHead) {
  PrintList(pHead);
  PrintListReversingly_Iteratively(pHead);
  printf("\n");
  PrintListReversingly_Recursively(pHead);
}
// 1->2->3->4->5
void Test1() {
  printf("\nTest1 begins.\n");
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(3);
  ListNode *pNode4 = CreateListNode(4);
  ListNode *pNode5 = CreateListNode(5);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  Test(pNode1);
  DestroyList(pNode1);
}
// Only one node: 1
void Test2() {
  printf("\nTest2 begins.\n");
  ListNode *pNode1 = CreateListNode(1);
  Test(pNode1);
  DestroyList(pNode1);
}
// Empty list
void Test3() {
  printf("\nTest3 begins.\n");
  Test(NULL);
}
int main(int argc, char *argv[]) {
  Test1();
  Test2();
  Test3();
  return 0;
}
```

## 014. SortLists


```cpp

// C:\Codes\Codes\C++\list.cpp
#include <stdlib.h>
#include <stdio.h>
#include "list.h"
void Sort(ListNode **pHead) {
  if (pHead == NULL || *pHead == NULL)
    return;
  ListNode *pLastSorted = *pHead;
  ListNode *pToBeSorted = pLastSorted->m_pNext;
  while (pToBeSorted != NULL) {
    if (pToBeSorted->m_nValue < (*pHead)->m_nValue) {
      pLastSorted->m_pNext = pToBeSorted->m_pNext;
      pToBeSorted->m_pNext = *pHead;
      *pHead = pToBeSorted;
    } else {
      ListNode *pNode = *pHead;
      while (pNode != pLastSorted &&
             pNode->m_pNext->m_nValue < pToBeSorted->m_nValue) {
        pNode = pNode->m_pNext;
      }
      if (pNode != pLastSorted) {
        pLastSorted->m_pNext = pToBeSorted->m_pNext;
        pToBeSorted->m_pNext = pNode->m_pNext;
        pNode->m_pNext = pToBeSorted;
      } else
        pLastSorted = pLastSorted->m_pNext;
    }
    pToBeSorted = pLastSorted->m_pNext;
  }
}
// ==================== Test Code ====================
void Test(char *testName, ListNode **pHead) {
  if (testName != NULL)
    printf("%s begins: \n", testName);
  if (pHead != NULL)
    PrintList(*pHead);
  Sort(pHead);
  if (pHead != NULL)
    PrintList(*pHead);
}
void Test1() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(3);
  ListNode *pNode3 = CreateListNode(5);
  ListNode *pNode4 = CreateListNode(7);
  ListNode *pNode5 = CreateListNode(2);
  ListNode *pNode6 = CreateListNode(4);
  ListNode *pNode7 = CreateListNode(6);
  ListNode *pNode8 = CreateListNode(8);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode6);
  ConnectListNodes(pNode6, pNode7);
  ConnectListNodes(pNode7, pNode8);
  ListNode *pHead = pNode1;
  Test("Test1", &pHead);
  DestroyList(pHead);
}
// nodes in a list are already sorted
void Test2() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(3);
  ListNode *pNode4 = CreateListNode(4);
  ListNode *pNode5 = CreateListNode(5);
  ListNode *pNode6 = CreateListNode(6);
  ListNode *pNode7 = CreateListNode(7);
  ListNode *pNode8 = CreateListNode(8);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode6);
  ConnectListNodes(pNode6, pNode7);
  ConnectListNodes(pNode7, pNode8);
  ListNode *pHead = pNode1;
  Test("Test2", &pHead);
  DestroyList(pHead);
}
// nodes in a list are decreasingly sorted
void Test3() {
  ListNode *pNode1 = CreateListNode(8);
  ListNode *pNode2 = CreateListNode(7);
  ListNode *pNode3 = CreateListNode(6);
  ListNode *pNode4 = CreateListNode(5);
  ListNode *pNode5 = CreateListNode(4);
  ListNode *pNode6 = CreateListNode(3);
  ListNode *pNode7 = CreateListNode(2);
  ListNode *pNode8 = CreateListNode(1);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode6);
  ConnectListNodes(pNode6, pNode7);
  ConnectListNodes(pNode7, pNode8);
  ListNode *pHead = pNode1;
  Test("Test3", &pHead);
  DestroyList(pHead);
}
// A list has only one node
void Test4() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pHead = pNode1;
  Test("Test4", &pHead);
  DestroyList(pHead);
}
// A list has duplicated nodes
void Test5() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(2);
  ListNode *pNode4 = CreateListNode(1);
  ListNode *pNode5 = CreateListNode(4);
  ListNode *pNode6 = CreateListNode(3);
  ListNode *pNode7 = CreateListNode(4);
  ListNode *pNode8 = CreateListNode(2);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode6);
  ConnectListNodes(pNode6, pNode7);
  ConnectListNodes(pNode7, pNode8);
  ListNode *pHead = pNode1;
  Test("Test5", &pHead);
  DestroyList(pHead);
}
// Empty List
void Test6() { Test("Test6", NULL); }
// Empty List
void Test7() {
  ListNode *pHead = NULL;
  Test("Test7", &pHead);
}
int main(int argc, char *argv[]) {
  Test1();
  Test2();
  Test3();
  Test4();
  Test5();
  Test6();
  Test7();
  return 0;
}
```

## list


```cpp

#include <stdio.h>
#include <stdlib.h>
#include "list.h"
ListNode *CreateListNode(int value) {
  ListNode *pNode = new ListNode();
  pNode->m_nValue = value;
  pNode->m_pNext = NULL;
  return pNode;
}
void ConnectListNodes(ListNode *pCurrent, ListNode *pNext) {
  if (pCurrent == NULL) {
    printf("Error to connect two nodes.\n");
    exit(1);
  }
  pCurrent->m_pNext = pNext;
}
void PrintListNode(ListNode *pNode) {
  if (pNode == NULL) {
    printf("The node is NULL\n");
  } else {
    printf("The key in node is %d.\n", pNode->m_nValue);
  }
}
void PrintList(ListNode *pHead) {
  printf("PrintList starts.\n");
  ListNode *pNode = pHead;
  while (pNode != NULL) {
    printf("%d\t", pNode->m_nValue);
    pNode = pNode->m_pNext;
  }
  printf("\nPrintList ends.\n");
}
void DestroyList(ListNode *pHead) {
  ListNode *pNode = pHead;
  while (pNode != NULL) {
    pHead = pHead->m_pNext;
    delete pNode;
    pNode = pHead;
  }
}
void AddToTail(ListNode **pHead, int value) {
  ListNode *pNew = new ListNode();
  pNew->m_nValue = value;
  pNew->m_pNext = NULL;
  if (*pHead == NULL) {
    *pHead = pNew;
  } else {
    ListNode *pNode = *pHead;
    while (pNode->m_pNext != NULL)
      pNode = pNode->m_pNext;
    pNode->m_pNext = pNew;
  }
}
void RemoveNode(ListNode **pHead, int value) {
  if (pHead == NULL || *pHead == NULL)
    return;
  ListNode *pToBeDeleted = NULL;
  if ((*pHead)->m_nValue == value) {
    pToBeDeleted = *pHead;
    *pHead = (*pHead)->m_pNext;
  } else {
    ListNode *pNode = *pHead;
    while (pNode->m_pNext != NULL && pNode->m_pNext->m_nValue != value)
      pNode = pNode->m_pNext;
    if (pNode->m_pNext != NULL && pNode->m_pNext->m_nValue == value) {
      pToBeDeleted = pNode->m_pNext;
      pNode->m_pNext = pNode->m_pNext->m_pNext;
    }
  }
  if (pToBeDeleted != NULL) {
    delete pToBeDeleted;
    pToBeDeleted = NULL;
  }
}
```

## list


```h

#pragma once
struct ListNode {
  int m_nValue;
  ListNode *m_pNext;
};
__declspec(dllexport) ListNode *CreateListNode(int value);
__declspec(dllexport) void ConnectListNodes(ListNode *pCurrent,
                                            ListNode *pNext);
__declspec(dllexport) void PrintListNode(ListNode *pNode);
__declspec(dllexport) void PrintList(ListNode *pHead);
__declspec(dllexport) void DestroyList(ListNode *pHead);
__declspec(dllexport) void AddToTail(ListNode **pHead, int value);
__declspec(dllexport) void RemoveNode(ListNode **pHead, int value);
```

