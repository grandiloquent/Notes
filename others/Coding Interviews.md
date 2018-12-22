# Coding Interviews

https://www.apress.com/cn/book/9781430247616

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
- [015. MergeSortedLists](#015-mergesortedlists)
- [016. LoopsInLists](#016-loopsinlists)
- [017. EntryNodeInLoopsInLists](#017-entrynodeinloopsinlists)
- [018. NextNode](#018-nextnode)
- [019. VerrifyBinarySearchTrees](#019-verrifybinarysearchtrees)
- [020. LargestBinarySearchSubtrees](#020-largestbinarysearchsubtrees)
- [023. Fibonacci](#023-fibonacci)
- [027. ArrayRotation](#027-arrayrotation)
- [028. TurningNumber](#028-turningnumber)
- [029. MajorityElement](#029-majorityelement)
- [030. StringPath](#030-stringpath)
- [031. RobotMove](#031-robotmove)
- [035. NumberOf1](#035-numberof1)
- [037. ModifyANumberToAnother](#037-modifyanumbertoanother)
- [038. NumbersOccuringOnce](#038-numbersoccuringonce)
- [039. FindTwoMissingNumbers](#039-findtwomissingnumbers)
- [040. Power](#040-power)
- [041. Print1ToMaxOfNDigits](#041-print1tomaxofndigits)
- [042. AddNumericStrings](#042-addnumericstrings)
- [043. DeleteNodeInList](#043-deletenodeinlist)
- [044. DeleteDuplicationInList](#044-deleteduplicationinlist)
- [045. ReorderNumbers](#045-reordernumbers)
- [046. RemoveNumbers](#046-removenumbers)
- [047. KthNodeFromEnd](#047-kthnodefromend)
- [048. ReverseList](#048-reverselist)
- [049. ReverseListInGroups](#049-reverselistingroups)
- [050. SubtreeInTree](#050-subtreeintree)
- [051. MirrorOfBinaryTree](#051-mirrorofbinarytree)
- [052. SymmetricalBinaryTrees](#052-symmetricalbinarytrees)
- [053. PrintMatrix](#053-printmatrix)
- [054. CloneComplexList](#054-clonecomplexlist)
- [055. MinInStack](#055-mininstack)
- [056. StackPushPopOrder](#056-stackpushpoporder)
- [057. PrintTreeByLevel](#057-printtreebylevel)
- [058. PrintTreeALevelInALine](#058-printtreealevelinaline)
- [059. PrintTreeZigzag](#059-printtreezigzag)
- [060. PathInTree](#060-pathintree)
- [061. ConstructBinaryTree](#061-constructbinarytree)
- [062. SerializeBinaryTree](#062-serializebinarytree)
- [063. SequenceOfBST](#063-sequenceofbst)
- [064. ConvertBinarySearchTree](#064-convertbinarysearchtree)
- [065. StringPermutation](#065-stringpermutation)
- [066. EightQueens](#066-eightqueens)
- [067. ArrayPermutation](#067-arraypermutation)
- [068. StringCombination](#068-stringcombination)
- [069. MedianStream](#069-medianstream)
- [070. KLeastNumbers](#070-kleastnumbers)
- [071. ArrayIntersection](#071-arrayintersection)
- [072. GreatestSumOfSubarrays](#072-greatestsumofsubarrays)
- [073. NumberOf1](#073-numberof1)
- [074. SortArrayForMinNumber](#074-sortarrayforminnumber)
- [076. FirstNotRepeatingChar](#076-firstnotrepeatingchar)
- [077. FirstCharAppearingOnce](#077-firstcharappearingonce)
- [078. DelelteCharacters](#078-deleltecharacters)
- [079. DelelteDuplicatedCharacters](#079-delelteduplicatedcharacters)
- [080. Anagram](#080-anagram)
- [081. ReversedPairs](#081-reversedpairs)
- [082. FirstCommonNodesInLists](#082-firstcommonnodesinlists)
- [083. Occurrence](#083-occurrence)
- [084. KthNodeInBST](#084-kthnodeinbst)
- [085. TreeDepth](#085-treedepth)
- [086. BalancedBinaryTree](#086-balancedbinarytree)
- [087. TwoNumbersWithSum](#087-twonumberswithsum)
- [088. ThreeNumbersWithSum](#088-threenumberswithsum)
- [089. SubsetWithSum](#089-subsetwithsum)
- [090. ContinousSequenceWithSum](#090-continoussequencewithsum)
- [091. ReverseWordsInSentence](#091-reversewordsinsentence)
- [092. LeftRotateString](#092-leftrotatestring)
- [093. MaxInSlidingWindows](#093-maxinslidingwindows)
- [094. QueueWithMax](#094-queuewithmax)
- [095. DicesProbability](#095-dicesprobability)
- [096. LastNumberInCircle](#096-lastnumberincircle)
- [097. MinimalMoves](#097-minimalmoves)
- [098. MaximalProfitBuyingSellingStock](#098-maximalprofitbuyingsellingstock)
- [099. Accumulate](#099-accumulate)
- [100. 103. ArithmeticOperations](#100-103-arithmeticoperations)
- [104. SealedClass](#104-sealedclass)
- [105. ConstuctArray](#105-constuctarray)
- [106. StringToInt](#106-stringtoint)
- [107. LowestAncestorInTrees](#107-lowestancestorintrees)
- [BinaryTree](#binarytree)
- [BinaryTree](#binarytree)
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

## 015. MergeSortedLists


```cpp

// "C:\Codes\Codes\C++\Coding Interviews\list.cpp"
#include <stdio.h>
#include "List.h"
ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
{
    if(pHead1 == NULL)
        return pHead2;
    else if(pHead2 == NULL)
        return pHead1;
    ListNode* pMergedHead = NULL;
    if(pHead1->m_nValue < pHead2->m_nValue)
    {
        pMergedHead = pHead1;
        pMergedHead->m_pNext = Merge(pHead1->m_pNext, pHead2);
    }
    else
    {
        pMergedHead = pHead2;
        pMergedHead->m_pNext = Merge(pHead1, pHead2->m_pNext);
    }
    return pMergedHead;
}
// ==================== Test Code ====================
ListNode* Test(char* testName, ListNode* pHead1, ListNode* pHead2)
{
    if(testName != NULL)
        printf("%s begins:\n", testName);
    printf("The first list is:\n");
    PrintList(pHead1);
    printf("The second list is:\n");
    PrintList(pHead2);
    printf("The merged list is:\n");
    ListNode* pMergedHead = Merge(pHead1, pHead2);
    PrintList(pMergedHead);
    
    printf("\n\n");
    return pMergedHead;
}
// list1: 1->3->5
// list2: 2->4->6
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode3);
    ConnectListNodes(pNode3, pNode5);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode6 = CreateListNode(6);
    ConnectListNodes(pNode2, pNode4);
    ConnectListNodes(pNode4, pNode6);
    ListNode* pMergedHead = Test("Test1", pNode1, pNode2);
    DestroyList(pMergedHead);
}
// There are duplicated nodes in two lists
// list1: 1->3->5
// list2: 1->3->5
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode3);
    ConnectListNodes(pNode3, pNode5);
    ListNode* pNode2 = CreateListNode(1);
    ListNode* pNode4 = CreateListNode(3);
    ListNode* pNode6 = CreateListNode(5);
    ConnectListNodes(pNode2, pNode4);
    ConnectListNodes(pNode4, pNode6);
    ListNode* pMergedHead = Test("Test2", pNode1, pNode2);
    DestroyList(pMergedHead);
}
// Anyone of the two lists has only one node
// list1: 1
// list2: 2
void Test3()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pMergedHead = Test("Test3", pNode1, pNode2);
    DestroyList(pMergedHead);
}
// One of the lists is empty
// list1: 1->3->5
// list2: empty
void Test4()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode3);
    ConnectListNodes(pNode3, pNode5);
    ListNode* pMergedHead = Test("Test4", pNode1, NULL);
    DestroyList(pMergedHead);
}
// One of the lists is empty
// list1: empty
// list2: 1->3->5
void Test5()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode3);
    ConnectListNodes(pNode3, pNode5);
    ListNode* pMergedHead = Test("Test5", NULL, pNode1);
    DestroyList(pMergedHead);
}
// Two lists are empty
// list1: empty
// list2: empty
void Test6()
{
    ListNode* pMergedHead = Test("Test6", NULL, NULL);
}
// nodes in list1 are less than nodes in list2
// list1: 1->2->3
// list2: 4->5->6
void Test7()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ListNode* pMergedHead = Test("Test7", pNode1, pNode4);
    DestroyList(pMergedHead);
}
// nodes in list1 are greater than nodes in list2
// list1: 4->5->6
// list2: 1->2->3
void Test8()
{
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ListNode* pMergedHead = Test("Test8", pNode4, pNode1);
    DestroyList(pMergedHead);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
    return 0;
}
```

## 016. LoopsInLists


```cpp

// "C:\Codes\Codes\C++\Coding Interviews\list.cpp"
#include <stdio.h>
#include "List.h"
bool HasLoop(ListNode *pHead) {
  if (pHead == NULL)
    return false;
  ListNode *pSlow = pHead->m_pNext;
  if (pSlow == NULL)
    return false;
  ListNode *pFast = pSlow->m_pNext;
  while (pFast != NULL && pSlow != NULL) {
    if (pFast == pSlow)
      return true;
    pSlow = pSlow->m_pNext;
    pFast = pFast->m_pNext;
    if (pFast != NULL)
      pFast = pFast->m_pNext;
  }
  return false;
}
// ==================== Test Code ====================
void Test(char *testName, ListNode *pHead, bool expected) {
  if (testName != NULL)
    printf("%s begins: ", testName);
  if (HasLoop(pHead) == expected)
    printf("HasLoop Passed.\n");
  else
    printf("HasLoop FAILED.\n");
}
// A list has a node, without a loop
void Test1() {
  ListNode *pNode1 = CreateListNode(1);
  Test("Test1", pNode1, false);
  DestroyList(pNode1);
}
// A list has a node, with a loop
void Test2() {
  ListNode *pNode1 = CreateListNode(1);
  ConnectListNodes(pNode1, pNode1);
  Test("Test2", pNode1, true);
  delete pNode1;
  pNode1 = NULL;
}
// A list has multiple nodes, with a loop
void Test3() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(3);
  ListNode *pNode4 = CreateListNode(4);
  ListNode *pNode5 = CreateListNode(5);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode3);
  Test("Test3", pNode1, true);
  delete pNode1;
  pNode1 = NULL;
  delete pNode2;
  pNode2 = NULL;
  delete pNode3;
  pNode3 = NULL;
  delete pNode4;
  pNode4 = NULL;
  delete pNode5;
  pNode5 = NULL;
}
// A list has multiple nodes, with a loop
void Test4() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(3);
  ListNode *pNode4 = CreateListNode(4);
  ListNode *pNode5 = CreateListNode(5);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode1);
  Test("Test4", pNode1, true);
  delete pNode1;
  pNode1 = NULL;
  delete pNode2;
  pNode2 = NULL;
  delete pNode3;
  pNode3 = NULL;
  delete pNode4;
  pNode4 = NULL;
  delete pNode5;
  pNode5 = NULL;
}
// A list has multiple nodes, with a loop
void Test5() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(3);
  ListNode *pNode4 = CreateListNode(4);
  ListNode *pNode5 = CreateListNode(5);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  ConnectListNodes(pNode5, pNode5);
  Test("Test5", pNode1, true);
  delete pNode1;
  pNode1 = NULL;
  delete pNode2;
  pNode2 = NULL;
  delete pNode3;
  pNode3 = NULL;
  delete pNode4;
  pNode4 = NULL;
  delete pNode5;
  pNode5 = NULL;
}
// A list has multiple nodes, without a loop
void Test6() {
  ListNode *pNode1 = CreateListNode(1);
  ListNode *pNode2 = CreateListNode(2);
  ListNode *pNode3 = CreateListNode(3);
  ListNode *pNode4 = CreateListNode(4);
  ListNode *pNode5 = CreateListNode(5);
  ConnectListNodes(pNode1, pNode2);
  ConnectListNodes(pNode2, pNode3);
  ConnectListNodes(pNode3, pNode4);
  ConnectListNodes(pNode4, pNode5);
  Test("Test6", pNode1, false);
  DestroyList(pNode1);
}
// Empty list
void Test7() { Test("Test7", NULL, false); }
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

## 017. EntryNodeInLoopsInLists


```cpp

// "C:\Codes\Codes\C++\Coding Interviews\list.cpp"
#include <stdio.h>
#include "List.h"
ListNode* MeetingNode(ListNode* pHead)
{
    if(pHead == NULL)
        return NULL;
    ListNode* pSlow = pHead->m_pNext;
    if(pSlow == NULL)
        return NULL;
    ListNode* pFast = pSlow->m_pNext;
    while(pFast != NULL && pSlow != NULL)
    {
        if(pFast == pSlow)
            return pFast;
        pSlow = pSlow->m_pNext;
        pFast = pFast->m_pNext;
        if(pFast != NULL)
            pFast = pFast->m_pNext;
    }
    return NULL;
}
ListNode* EntryNodeOfLoop(ListNode* pHead)
{
    ListNode* meetingNode = MeetingNode(pHead);
    if(meetingNode == NULL)
        return NULL;
    // get the number of nodes in loop
    int nodesInLoop = 1;
    ListNode* pNode1 = meetingNode;
    while(pNode1->m_pNext != meetingNode)
    {
        pNode1 = pNode1->m_pNext;
        ++nodesInLoop;
    }
    // move pNode1
    pNode1 = pHead;
    for(int i = 0; i < nodesInLoop; ++i)
        pNode1 = pNode1->m_pNext;
    // move pNode1 and pNode2
    ListNode* pNode2 = pHead;
    while(pNode1 != pNode2)
    {
        pNode1 = pNode1->m_pNext;
        pNode2 = pNode2->m_pNext;
    }
    return pNode1;
}
// ==================== Test Code ====================
void Test(char* testName, ListNode* pHead, ListNode* entryNode)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(EntryNodeOfLoop(pHead) == entryNode)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
// A list has a node, without a loop
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    Test("Test1", pNode1, NULL);
    DestroyList(pNode1);
}
// A list has a node, with a loop
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ConnectListNodes(pNode1, pNode1);
    Test("Test2", pNode1, pNode1);
    delete pNode1;
    pNode1 = NULL;
}
// A list has multiple nodes, with a loop 
void Test3()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode3);
    Test("Test3", pNode1, pNode3);
    delete pNode1;
    pNode1 = NULL;
    delete pNode2;
    pNode2 = NULL;
    delete pNode3;
    pNode3 = NULL;
    delete pNode4;
    pNode4 = NULL;
    delete pNode5;
    pNode5 = NULL;
}
// A list has multiple nodes, with a loop 
void Test4()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode1);
    Test("Test4", pNode1, pNode1);
    delete pNode1;
    pNode1 = NULL;
    delete pNode2;
    pNode2 = NULL;
    delete pNode3;
    pNode3 = NULL;
    delete pNode4;
    pNode4 = NULL;
    delete pNode5;
    pNode5 = NULL;
}
// A list has multiple nodes, with a loop 
void Test5()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode5);
    Test("Test5", pNode1, pNode5);
    delete pNode1;
    pNode1 = NULL;
    delete pNode2;
    pNode2 = NULL;
    delete pNode3;
    pNode3 = NULL;
    delete pNode4;
    pNode4 = NULL;
    delete pNode5;
    pNode5 = NULL;
}
// A list has multiple nodes, without a loop 
void Test6()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test6", pNode1, NULL);
    DestroyList(pNode1);
}
// Empty list
void Test7()
{
    Test("Test7", NULL, NULL);
}
int main(int argc, char* argv[])
{
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

## 018. NextNode


```cpp

#include <stdio.h>
struct BinaryTreeNode 
{
    int                    m_nValue; 
    BinaryTreeNode*        m_pLeft;  
    BinaryTreeNode*        m_pRight;
    BinaryTreeNode*        m_pParent; 
};
BinaryTreeNode* GetNext(BinaryTreeNode* pNode)
{
    if(pNode == NULL)
        return NULL;
    
    BinaryTreeNode* pNext = NULL;
    if(pNode->m_pRight != NULL)
    {
        BinaryTreeNode* pRight = pNode->m_pRight;
        while(pRight->m_pLeft != NULL)
            pRight = pRight->m_pLeft;
            
        pNext = pRight;
    }
    else if(pNode->m_pParent != NULL)
    {
        BinaryTreeNode* pCurrent = pNode;
        BinaryTreeNode* pParent = pNode->m_pParent;
        while(pParent != NULL && pCurrent == pParent->m_pRight)
        {
            pCurrent = pParent;
            pParent = pParent->m_pParent;
        }
        
        pNext = pParent;
    }
    
    return pNext;
}
// ==================== Code for Binary Trees ====================
BinaryTreeNode* CreateBinaryTreeNode(int value)
{
    BinaryTreeNode* pNode = new BinaryTreeNode();
    pNode->m_nValue = value;
    pNode->m_pLeft = NULL;
    pNode->m_pRight = NULL;
    pNode->m_pParent = NULL;
    return pNode;
}
void ConnectTreeNodes(BinaryTreeNode* pParent, BinaryTreeNode* pLeft, BinaryTreeNode* pRight)
{
    if(pParent != NULL)
    {
        pParent->m_pLeft = pLeft;
        pParent->m_pRight = pRight;
        
        if(pLeft != NULL)
            pLeft->m_pParent = pParent;
        if(pRight != NULL)
            pRight->m_pParent = pParent;
    }
}
void PrintTreeNode(BinaryTreeNode* pNode)
{
    if(pNode != NULL)
    {
        printf("value of this node is: %d\n", pNode->m_nValue);
        if(pNode->m_pLeft != NULL)
            printf("value of its left child is: %d.\n", pNode->m_pLeft->m_nValue);
        else
            printf("left child is null.\n");
        if(pNode->m_pRight != NULL)
            printf("value of its right child is: %d.\n", pNode->m_pRight->m_nValue);
        else
            printf("right child is null.\n");
    }
    else
    {
        printf("this node is null.\n");
    }
    printf("\n");
}
void PrintTree(BinaryTreeNode* pRoot)
{
    PrintTreeNode(pRoot);
    if(pRoot != NULL)
    {
        if(pRoot->m_pLeft != NULL)
            PrintTree(pRoot->m_pLeft);
        if(pRoot->m_pRight != NULL)
            PrintTree(pRoot->m_pRight);
    }
}
void DestroyTree(BinaryTreeNode* pRoot)
{
    if(pRoot != NULL)
    {
        BinaryTreeNode* pLeft = pRoot->m_pLeft;
        BinaryTreeNode* pRight = pRoot->m_pRight;
        delete pRoot;
        pRoot = NULL;
        DestroyTree(pLeft);
        DestroyTree(pRight);
    }
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pNode, BinaryTreeNode* expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
        
    BinaryTreeNode* pNext = GetNext(pNode);
    if(pNext == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
//            8
//        6      10
//       5 7    9  11
void Test1_7()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    Test("Test1", pNode8, pNode9);
    Test("Test2", pNode6, pNode7);
    Test("Test3", pNode10, pNode11);
    Test("Test4", pNode5, pNode6);
    Test("Test5", pNode7, pNode8);
    Test("Test6", pNode9, pNode10);
    Test("Test7", pNode11, NULL);
    DestroyTree(pNode8);
}
//            5
//          4
//        3
//      2
void Test8_11()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    Test("Test8", pNode5, NULL);
    Test("Test9", pNode4, pNode5);
    Test("Test10", pNode3, pNode4);
    Test("Test11", pNode2, pNode3);
    DestroyTree(pNode5);
}
//        2
//         3
//          4
//           5
void Test12_15()
{
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("Test12", pNode5, NULL);
    Test("Test13", pNode4, pNode5);
    Test("Test14", pNode3, pNode4);
    Test("Test15", pNode2, pNode3);
    DestroyTree(pNode2);
}
void Test16()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    Test("Test16", pNode5, NULL);
    DestroyTree(pNode5);
}
int main(int argc, char* argv[])
{
    Test1_7();
    Test8_11();
    Test12_15();
    Test16();
}
```

## 019. VerrifyBinarySearchTrees


```cpp

// "C:\Codes\Codes\C++\Coding Interviews\BinaryTree.cpp"
#include <stdio.h>
#include "BinaryTree.h"
#include <limits>
using namespace std;
bool isBSTCore_Solution1(BinaryTreeNode* pRoot, int min, int max);
bool isBSTCore_Solution2(BinaryTreeNode* pRoot, int& prev);
// ==================== Solution 1 ====================
bool isBST_Solution1(BinaryTreeNode* pRoot)
{
    int min = numeric_limits<int>::min();
    int max = numeric_limits<int>::max();
    return isBSTCore_Solution1(pRoot, min, max);
}
bool isBSTCore_Solution1(BinaryTreeNode* pRoot, int min, int max)
{
    if(pRoot == NULL)
        return true;
    if(pRoot->m_nValue < min || pRoot->m_nValue > max)
        return false;
    return isBSTCore_Solution1(pRoot->m_pLeft, min, pRoot->m_nValue)
        && isBSTCore_Solution1(pRoot->m_pRight, pRoot->m_nValue, max);
}
// ==================== Solution 2 ====================
bool isBST_Solution2(BinaryTreeNode* pRoot)
{
    int prev = numeric_limits<int>::min();
    return isBSTCore_Solution2(pRoot, prev);
}
bool isBSTCore_Solution2(BinaryTreeNode* pRoot, int& prev)
{
    if(pRoot == NULL)
        return true;
    return isBSTCore_Solution2(pRoot->m_pLeft, prev) // previous node
        && (pRoot->m_nValue >= prev) // current node
        && isBSTCore_Solution2(pRoot->m_pRight, prev = pRoot->m_nValue); // next node
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot, bool expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(isBST_Solution1(pRoot) == expected)
        printf("Solution 1 Passed; ");
    else
        printf("Solution 1 FAILED; ");
    if(isBST_Solution2(pRoot) == expected)
        printf("Solution 2 Passed.\n");
    else
        printf("Solution 2 FAILED.\n");
}
//            8
//        6      10
//       5 7    9  11
void Test1()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    Test("Test1", pNode8, true);
    DestroyTree(pNode8);
}
//            8
//        10      6
//      11   9  7   5
void Test2()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode10, pNode6);
    ConnectTreeNodes(pNode10, pNode11, pNode9);
    ConnectTreeNodes(pNode6, pNode7, pNode5);
    Test("Test2", pNode8, false);
    DestroyTree(pNode8);
}
//            5
//        5      5
//      3  
void Test3()
{
    BinaryTreeNode* pNode51 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode52 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode53 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    ConnectTreeNodes(pNode51, pNode52, pNode53);
    ConnectTreeNodes(pNode52, pNode3, NULL);
    Test("Test3", pNode51, true);
    DestroyTree(pNode51);
}
//            5
//        5       5
//              3  
void Test4()
{
    BinaryTreeNode* pNode51 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode52 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode53 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    ConnectTreeNodes(pNode51, pNode52, pNode53);
    ConnectTreeNodes(pNode53, pNode3, NULL);
    Test("Test4", pNode51, false);
    DestroyTree(pNode51);
}
//            5
//          4
//        3
//      2
void Test5()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    Test("Test5", pNode5, true);
    DestroyTree(pNode5);
}
//        5
//         4
//          3
//           2
void Test6()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode2);
    Test("Test6", pNode5, false);
    DestroyTree(pNode5);
}
void Test7()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    Test("Test7", pNode5, true);
    DestroyTree(pNode5);
}
void Test8()
{
    Test("Test8", NULL, true);
}
//        100
//        /
//       50   
//         \
//         150
void Test9()
{
    BinaryTreeNode* pNode100 = CreateBinaryTreeNode(100);
    BinaryTreeNode* pNode50 = CreateBinaryTreeNode(50);
    BinaryTreeNode* pNode150 = CreateBinaryTreeNode(150);
    ConnectTreeNodes(pNode100, pNode50, NULL);
    ConnectTreeNodes(pNode50, NULL, pNode150);
    Test("Test9", pNode100, false);
    DestroyTree(pNode100);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
    Test9();
	return 0;
}
```

## 020. LargestBinarySearchSubtrees


```cpp

// "C:\Codes\Codes\C++\Coding Interviews\BinaryTree.cpp"
#include <stdlib.h>
#include <stdio.h>
#include "BinaryTree.h"
bool LargestBST(BinaryTreeNode* pRoot, int& min, int& max, int& largestSize);
int LargestBST(BinaryTreeNode* pRoot)
{
    int min, max, largestSize;
    LargestBST(pRoot, min, max, largestSize);
    
    return largestSize;
}
bool LargestBST(BinaryTreeNode* pRoot, int& min, int& max, int& largestSize)
{
    if(pRoot == NULL)
    {
        max = 0x80000000;
        min = 0x7FFFFFFF;
        largestSize = 0;
        return true;
    }
    int minLeft, maxLeft, leftSize;
    bool left = LargestBST(pRoot->m_pLeft, minLeft, maxLeft, leftSize);
    int minRight, maxRight, rightSize;
    bool right  = LargestBST(pRoot->m_pRight, minRight, maxRight, rightSize);
    
    bool overall = false;
    if(left && right && pRoot->m_nValue >= maxLeft && pRoot->m_nValue <= minRight)
    {
        largestSize = leftSize + rightSize + 1;
        overall = true;
    
        min = (pRoot->m_nValue < minLeft) ? pRoot->m_nValue : minLeft;
        max = (pRoot->m_nValue > maxRight) ? pRoot->m_nValue : maxRight;
    }
    else
        largestSize = (leftSize > rightSize) ? leftSize : rightSize;
        
    return overall;
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(LargestBST(pRoot) == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
//            8
//        6      10
//       5 7    9  11
void Test1()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    Test("Test1", pNode8, 7);
    DestroyTree(pNode8);
}
//            11
//        6      16
//       5 7       9
void Test2()
{
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode16 = CreateBinaryTreeNode(16);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    ConnectTreeNodes(pNode11, pNode6, pNode16);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode16, NULL, pNode9);
    Test("Test2", pNode11, 3);
    DestroyTree(pNode11);
}
//            12
//        6      9
//       5     8   10  
void Test3()
{
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(12);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    ConnectTreeNodes(pNode12, pNode6, pNode9);
    ConnectTreeNodes(pNode6, pNode5, NULL);
    ConnectTreeNodes(pNode9, pNode8, pNode10);
    Test("Test3", pNode12, 3);
    DestroyTree(pNode12);
}
//               5
//              /
//             3
//            /
//           4
//          /
//         2
//        /
//       1
void Test4()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode1, NULL);
    Test("Test4", pNode5, 3);
    DestroyTree(pNode5);
}
// 2
//  \
//   1
//    \
//     3
//      \
//       4
//        \
//         5
void Test5()
{
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode2, NULL, pNode1);
    ConnectTreeNodes(pNode1, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("Test5", pNode2, 4);
    DestroyTree(pNode2);
}
//            8
//        10      6
//      11  9   7   5
void Test6()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode8, pNode10, pNode6);
    ConnectTreeNodes(pNode10, pNode11, pNode9);
    ConnectTreeNodes(pNode6, pNode7, pNode5);
    Test("Test6", pNode8, 1);
    DestroyTree(pNode8);
}
void Test7()
{
    Test("Test7", NULL, 0);
}
int main(int agrc, char* argv[])
{
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

## 023. Fibonacci


```c

#include <stdio.h>
// ==================== Solution 1 ====================
long long Fibonacci_Solution1(unsigned int n)
{
    if(n <= 0)
        return 0;
    if(n == 1)
        return 1;
    return Fibonacci_Solution1(n - 1) + Fibonacci_Solution1(n - 2);
}
// ==================== Solution 2 ====================
long long Fibonacci_Solution2(unsigned int n)
{
    int result[2] = {0, 1};
    long long  fibNMinusOne = 1;
    long long  fibNMinusTwo = 0;
    long long  fibN = 0;
    unsigned int i;
    if(n < 2)
        return result[n];
    for(i = 2; i <= n; ++ i)
    {
        fibN = fibNMinusOne + fibNMinusTwo;
        fibNMinusTwo = fibNMinusOne;
        fibNMinusOne = fibN;
    }
     return fibN;
}
// ==================== Solution 3 ====================
#include <assert.h>
struct Matrix2By2
{
    long long m_00;
    long long m_01;
    long long m_10;
    long long m_11;
};
struct Matrix2By2 MatrixMultiply
(
    const struct Matrix2By2* matrix1, 
    const struct Matrix2By2* matrix2
)
{
    struct Matrix2By2 result;
    result.m_00 = matrix1->m_00 * matrix2->m_00 + matrix1->m_01 * matrix2->m_10;
    result.m_01 = matrix1->m_00 * matrix2->m_01 + matrix1->m_01 * matrix2->m_11;
    result.m_10 = matrix1->m_10 * matrix2->m_00 + matrix1->m_11 * matrix2->m_10;
    result.m_11 = matrix1->m_10 * matrix2->m_01 + matrix1->m_11 * matrix2->m_11;
    
    return result;
}
struct Matrix2By2 MatrixPower(unsigned int n)
{
    struct Matrix2By2 result;
    struct Matrix2By2 unit = {1, 1, 1, 0};
    assert(n > 0);
    if(n == 1)
    {
        result = unit;
    }
    else if(n % 2 == 0)
    {
        result = MatrixPower(n / 2);
        result = MatrixMultiply(&result, &result);
    }
    else if(n % 2 == 1)
    {
        result = MatrixPower((n - 1) / 2);
        result = MatrixMultiply(&result, &result);
        result = MatrixMultiply(&result, &unit);
    }
    return result;
}
long long Fibonacci_Solution3(unsigned int n)
{
    struct Matrix2By2 PowerNMinus2;
    int result[2] = {0, 1};
    
    if(n < 2)
        return result[n];
    PowerNMinus2 = MatrixPower(n - 1);
    return PowerNMinus2.m_00;
}
// ==================== Test Code ====================
void Test(int n, int expected)
{
    if(Fibonacci_Solution1(n) == expected)
        printf("Test for %d in solution1 passed.\n", n);
    else
        printf("Test for %d in solution1 failed.\n", n);
    if(Fibonacci_Solution2(n) == expected)
        printf("Test for %d in solution2 passed.\n", n);
    else
        printf("Test for %d in solution2 failed.\n", n);
    if(Fibonacci_Solution3(n) == expected)
        printf("Test for %d in solution3 passed.\n", n);
    else
        printf("Test for %d in solution3 failed.\n", n);
}
int main(int argc, char* argv[])
{
    Test(0, 0);
    Test(1, 1);
    Test(2, 1);
    Test(3, 2);
    Test(4, 3);
    Test(5, 5);
    Test(6, 8);
    Test(7, 13);
    Test(8, 21);
    Test(9, 34);
    Test(10, 55);
    Test(40, 102334155);
    return 0;
}
```

## 027. ArrayRotation


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class Minimum {
    public static int getMin(int numbers[]) {
        int index1 = 0;
        int index2 = numbers.length - 1;
        int indexMid = index1;
        
        while(numbers[index1] >= numbers[index2]) {
            if(index2 - index1 == 1) {
                indexMid = index2;
                break;
            }
            
            indexMid = (index1 + index2) / 2;
            
            // if numbers with indexes index1, index2, indexMid
            // are equal, search sequentially
            if(numbers[index1] == numbers[index2]
                    && numbers[indexMid] == numbers[index1])
                return getMinSequentially(numbers, index1, index2);
            
            if(numbers[indexMid] >= numbers[index1])
                index1 = indexMid;
            else if(numbers[indexMid] <= numbers[index2])
                index2 = indexMid;
        
        }
        
        return numbers[indexMid];
    }
    
    private static int getMinSequentially(int numbers[], int index1, int index2) {
        int result = numbers[index1];
        for(int i = index1 + 1; i <= index2; ++i) {
            if(result > numbers[i])
            result = numbers[i];
        }
        
        return result;
    }
    
    
    //================= Test Code =================
    private static void test(String testName, int[] numbers, int expected) {
        System.out.print(testName + " begins: ");
        if(getMin(numbers) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    public static void main(String argv[]) {
        // typical input, a rotation of a increasingly sorted array
        int array1[] = {3, 4, 5, 1, 2};
        test("Test1", array1, 1);
        // duplicated number is the minimal
        int array2[] = {3, 4, 5, 1, 1, 2};
        test("Test2", array2, 1);
        // duplicated number is not in both beginning and end
        int array3[] = {3, 4, 5, 1, 2, 2};
        test("Test3", array3, 1);
        // duplicated number is in both beginning and end
        int array4[] = {1, 0, 1, 1, 1};
        test("Test4", array4, 0);
        // rotate 0 numbers in an array
        int array5[] = {1, 2, 3, 4, 5};
        test("Test5", array5, 1);
        // only one number in an array
        int array6[] = {2};
        test("Test6", array6, 2);
    }
}
```

## 028. TurningNumber


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class TurningNumber {
    public static int getTurningIndex(int numbers[]) {
        if(numbers.length <= 2)
            return -1;
        int left = 0;
        int right = numbers.length - 1;
        while(right > left + 1) {
            int middle = (left + right) / 2;
            if(middle == 0 || middle == numbers.length - 1)
                return -1;
            if(numbers[middle] > numbers[middle - 1]
                && numbers[middle] > numbers[middle + 1])
                return middle;
            else if(numbers[middle] > numbers[middle - 1]
                && numbers[middle] < numbers[middle + 1])
                left = middle;
            else
                right = middle;
        }
        return -1;
    }
    
    //================= Test Code =================
    private static void test(String testName, int[] numbers, int expected) {
        System.out.print(testName + " begins: ");
        if(getTurningIndex(numbers) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    private static void test1() {
        int numbers[] = {1, 2, 3, 4, 5, 10, 9, 8, 7, 6};
        int expected = 5;
        test("test1", numbers, expected);
    }
    private static void test2() {
        int numbers[] = {1, 10, 9, 8, 7, 6, 5, 4, 3, 2};
        int expected = 1;
        test("test2", numbers, expected);
    }
    private static void test3() {
        int numbers[] = {1, 2, 3, 4, 5, 6, 7, 8, 10, 9};
        int expected = 8;
        test("test3", numbers, expected);
    }
    private static void test4() {
        int numbers[] = {1, 2, 1};
        int expected = 1;
        test("test4", numbers, expected);
    }
    
    public static void main(String args[]) {
        test1();
        test2();
        test3();
        test4();
    }
}
```

## 029. MajorityElement


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.Random;
public class MajorityElement {
    //================= Solution 1 =================
    public static int getMajority_1(int[] numbers){
        int length = numbers.length;
        int middle = length >> 1;
        int start = 0;
        int end = length - 1;
        
        int index = partition(numbers, start, end);
        while(index != middle){
            if(index > middle){
                end = index - 1;
                index = partition(numbers, start, end);
            }
            else{
                start = index + 1;
                index = partition(numbers, start, end);
            }
        }
        
        int result = numbers[middle];
        if(!checkMajorityExistence(numbers, result))
            throw new IllegalArgumentException("No majority exisits.");
        
        return result;
    }
    
    private static int partition(int[] numbers, int start, int end){
        int index = randomInRange(start, end);
        swap(numbers, index, end);
        
        int small = start - 1;
        for(index = start; index < end; ++index){
            if(numbers[index] < numbers[end]){
                ++small;
                if(small != index)
                    swap(numbers, index, small);
            }
        }
        
        ++small;
        swap(numbers, small, end);
        
        return small;
    }
    
    private static int randomInRange(int start, int end){
        Random random = new Random();
        return random.nextInt(end - start + 1) + start;
    }
    
    private static void swap(int[] numbers, int index1, int index2){
        int temp = numbers[index1];
        numbers[index1] = numbers[index2];
        numbers[index2] = temp;
    }
    
    //================= Solution 2 =================
    public static int getMajority_2(int[] numbers){
        int result = numbers[0];
        int times = 1;
        for(int i = 1; i < numbers.length; ++i){
            if(times == 0){
                result = numbers[i];
                times = 1;
            }
            else if(numbers[i] == result)
                times++;
            else
                times--;
        }
        
        if(!checkMajorityExistence(numbers, result))
            throw new IllegalArgumentException("No majority exisits.");
        
        return result;
    }
    
    //================= Common methods =================
    private static boolean checkMajorityExistence(int[] numbers, int number){
        int times = 0;
        for(int i = 0; i < numbers.length; ++i){
            if(numbers[i] == number)
                times++;
        }
        
        return (times * 2 > numbers.length);
    }
    
    //================= Test Code =================
    
    private static void test(String testName, int[] numbers, int expected, boolean invalidArgument){
        System.out.print(testName + " begins: ");
        
        try {
            int majority = getMajority_1(numbers);
            
            if(!invalidArgument && majority == expected)
                System.out.print("Solution1 Passed; ");
            else
                System.out.print("Solution1 FAILED; ");
        }
        catch(IllegalArgumentException e){
            if(invalidArgument)
                System.out.print("Solution1 Passed; ");
            else
                System.out.print("Solution1 FAILED; ");
        }
        
        try {
            int majority = getMajority_2(numbers);
            
            if(!invalidArgument && majority == expected)
                System.out.print("Solution2 Passed.\n");
            else
                System.out.print("Solution2 FAILED.\n");
        }
        catch(IllegalArgumentException e){
            if(invalidArgument)
                System.out.print("Solution2 Passed.\n");
            else
                System.out.print("Solution2 FAILED.\n");
        }
    }
    
    private static void test1(){
        int[] numbers = {1, 2, 3, 2, 2, 2, 5, 4, 2};
        test("Test1", numbers, 2, false); 
    }
    
    private static void test2(){
        int[] numbers = {1, 2, 3, 2, 4, 2, 5, 2, 3};
        test("test2", numbers, 0, true); 
    }
    
    private static void test3(){
        int[] numbers = {2, 2, 2, 2, 2, 1, 3, 4, 5};
        test("test3", numbers, 2, false); 
    }
    
    private static void test4(){
        int[] numbers = {1, 3, 4, 5, 2, 2, 2, 2, 2};
        test("test4", numbers, 2, false); 
    }
    
    private static void test5(){
        int[] numbers = {1};
        test("test4", numbers, 1, false); 
    }
   
    public static void main(String[] argv){
        test1();
        test2();
        test3();
        test4();
        test5();
    }
}
```

## 030. StringPath


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string>
#include <stack>
using namespace std;
bool hasPathCore(char* matrix, int rows, int cols, int row, int col, char* str, int& pathLength, bool* visited);
bool hasPath(char* matrix, int rows, int cols, char* str)
{
    if(matrix == NULL || rows < 1 || cols < 1 || str == NULL)
        return false;
    bool *visited = new bool[rows * cols];
    memset(visited, 0, rows * cols);
    int pathLength = 0;
    for(int row = 0; row < rows; ++row)
    {
        for(int col = 0; col < cols; ++col)
        {
            if(hasPathCore(matrix, rows, cols, row, col, str, pathLength, visited))
                return true;
        }
    }
    
    delete[] visited;
    return false;
}
bool hasPathCore(char* matrix, int rows, int cols, int row, int col, char* str, int& pathLength, bool* visited)
{
    if(str[pathLength] == '\0')
        return true;
        
    bool hasPath = false;
    if(row >= 0 && row < rows && col >= 0 && col < cols 
            && matrix[row * cols + col] == str[pathLength]
            && !visited[row * cols + col])
    {
        ++pathLength;
        visited[row * cols + col] = true;
        
        hasPath = hasPathCore(matrix, rows, cols, row, col - 1, str, pathLength, visited)
                || hasPathCore(matrix, rows, cols, row - 1, col, str, pathLength, visited)
                || hasPathCore(matrix, rows, cols, row, col + 1, str, pathLength, visited) 
                || hasPathCore(matrix, rows, cols, row + 1, col, str, pathLength, visited);
        
        if(!hasPath)
        {
            --pathLength;
            visited[row * cols + col] = false;
        }
    }
    
    return hasPath;
}
void Test(char* testName, char* matrix, int rows, int cols, char* str, bool expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(hasPath(matrix, rows, cols, str) == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
//ABCE
//SFCS
//ADEE
//ABCCED
void Test1()
{
    char matrix[] = "ABCESFCSADEE";
    char* str = "ABCCED";
    Test("Test1", (char*)matrix, 3, 4, str, true);
}
//ABCE
//SFCS
//ADEE
//SEE
void Test2()
{
    char matrix[] = "ABCESFCSADEE";
    char* str = "SEE";
    Test("Test2", (char*)matrix, 3, 4, str, true);
}
//ABCE
//SFCS
//ADEE
//ABCB
void Test3()
{
    char matrix[] = "ABCESFCSADEE";
    char* str = "ABCB";
    Test("Test3", (char*)matrix, 3, 4, str, false);
}
//ABCEHJIG
//SFCSLOPQ
//ADEEMNOE
//ADIDEJFM
//VCEIFGGS
//SLHECCEIDEJFGGFIE
void Test4()
{
    char matrix[] = "ABCEHJIGSFCSLOPQADEEMNOEADIDEJFMVCEIFGGS";
    char* str = "SLHECCEIDEJFGGFIE";
    Test("Test4", (char*)matrix, 5, 8, str, true);
}
//ABCEHJIG
//SFCSLOPQ
//ADEEMNOE
//ADIDEJFM
//VCEIFGGS
//SGGFIECVAASABCEHJIGQEM
void Test5()
{
    char matrix[] = "ABCEHJIGSFCSLOPQADEEMNOEADIDEJFMVCEIFGGS";
    char* str = "SGGFIECVAASABCEHJIGQEM";
    Test("Test5", (char*)matrix, 5, 8, str, true);
}
//ABCEHJIG
//SFCSLOPQ
//ADEEMNOE
//ADIDEJFM
//VCEIFGGS
//SGGFIECVAASABCEEJIGOEM
void Test6()
{
    char matrix[] = "ABCEHJIGSFCSLOPQADEEMNOEADIDEJFMVCEIFGGS";
    char* str = "SGGFIECVAASABCEEJIGOEM";
    Test("Test6", (char*)matrix, 5, 8, str, false);
}
//ABCEHJIG
//SFCSLOPQ
//ADEEMNOE
//ADIDEJFM
//VCEIFGGS
//SGGFIECVAASABCEHJIGQEMS
void Test7()
{
    char matrix[] = "ABCEHJIGSFCSLOPQADEEMNOEADIDEJFMVCEIFGGS";
    char* str = "SGGFIECVAASABCEHJIGQEMS";
    Test("Test7", (char*)matrix, 5, 8, str, false);
}
//AAAA
//AAAA
//AAAA
//AAAAAAAAAAAA
void Test8()
{
    char matrix[] = "AAAAAAAAAAAA";
    char* str = "AAAAAAAAAAAA";
    Test("Test8", (char*)matrix, 3, 4, str, true);
}
//AAAA
//AAAA
//AAAA
//AAAAAAAAAAAAA
void Test9()
{
    char matrix[] = "AAAAAAAAAAAA";
    char* str = "AAAAAAAAAAAAA";
    Test("Test9", (char*)matrix, 3, 4, str, false);
}
//A
//A
void Test10()
{
    char matrix[] = "A";
    char* str = "A";
    Test("Test10", (char*)matrix, 1, 1, str, true);
}
//A
//B
void Test11()
{
    char matrix[] = "A";
    char* str = "B";
    Test("Test11", (char*)matrix, 1, 1, str, false);
}
int main(int argc, char* argv[])
{
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
    Test11();
	return 0;
}
```

## 031. RobotMove


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
int getDigitSum(int number)
{
    int sum = 0;
    while(number > 0)
    {
        sum += number % 10;
        number /= 10;
    }
    
    return sum;
}
bool check(int threshold, int rows, int cols, int row, int col, bool* visited)
{
    if(row >=0 && row < rows && col >= 0 && col < cols
        && getDigitSum(row) + getDigitSum(col) <= threshold
        && !visited[row* cols + col])
        return true;
    
    return false;
}
int movingCountCore(int threshold, int rows, int cols, int row, int col, bool* visited)
{
    int count = 0;
    if(check(threshold, rows, cols, row, col, visited)) 
    {
        visited[row * cols + col] = true;
    
        count = 1 + movingCountCore(threshold, rows, cols, row - 1, col, visited)
                + movingCountCore(threshold, rows, cols, row, col - 1, visited)
                + movingCountCore(threshold, rows, cols, row + 1, col, visited)
                + movingCountCore(threshold, rows, cols, row, col + 1, visited);
    }
    
    return count;
}
int movingCount(int threshold, int rows, int cols)
{
    bool *visited = new bool[rows * cols];
    for(int i = 0; i < rows * cols; ++i)
        visited[i] = false;
        
    int count = movingCountCore(threshold, rows, cols, 0, 0, visited);
    
    delete[] visited;
    
    return count; 
}
// ================================ test code ================================
void test(char* testName, int threshold, int rows, int cols, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    if(movingCount(threshold, rows, cols) == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
void test1()
{
    test("Test1", 5, 10, 10, 21);
}
void test2()
{
    test("Test2", 15, 20, 20, 359);
}
void test3() 
{
    test("Test3", 10, 1, 100, 29);
}
void test4() 
{
    test("Test4", 10, 1, 10, 10);
}
void test5()
{
    test("Test5", 15, 100, 1, 79);
}
void test6()
{
    test("Test6", 15, 10, 1, 10);
}
void test7()
{
    test("Test7", 15, 1, 1, 1);
}
void test8()
{
    test("Test8", -10, 10, 10, 0);
}
int main(int agrc, char* argv[])
{
    test1();
    test2();
    test3();
    test4();
    test5();
    test6();
    test7();
    test8();
}
```

## 035. NumberOf1


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
int NumberOf1_Solution1(int n)
{
    int count = 0;
    unsigned int flag = 1;
    while(flag)
    {
        if(n & flag)
            count ++;
        flag = flag << 1;
    }
    return count;
}
int NumberOf1_Solution2(int n)
{
    int count = 0;
    while (n)
    {
        ++ count;
        n = (n - 1) & n;
    }
    return count;
}
// ==================== Test Code ====================
void Test(int number, unsigned int expected)
{
    int actual = NumberOf1_Solution1(number);
    if(actual == expected)
        printf("Solution1: Test for %p passed.\n", number);
    else
        printf("Solution1: Test for %p failed.\n", number);
    actual = NumberOf1_Solution2(number);
    if(actual == expected)
        printf("Solution2: Test for %p passed.\n", number);
    else
        printf("Solution2: Test for %p failed.\n", number);
    printf("\n");
}
int main(int argc, char* argv[])
{
    // input 0, expected output is 0
    Test(0, 0);
    // input 1, expected output is 1
    Test(1, 1);
    // input 10, expected output is 2
    Test(10, 2);
    // input 0x7FFFFFFF, expected output is 31
    Test(0x7FFFFFFF, 31);
    // input 0xFFFFFFFF (negative), expected output is 32
    Test(0xFFFFFFFF, 32);
    // input 0x80000000 (negative), expected output is 1
    Test(0x80000000, 1);
    return 0;
}
```

## 037. ModifyANumberToAnother


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
int bitsToModify(int number1, int number2)
{
    int temp = number1 ^ number2;
    
    // the number of 1 bits in temp
    int bits = 0;
    while(temp != 0) {
        ++bits;
        temp = (temp - 1) & temp;
    }
    
    return bits;
}
// ======== Test Code ========
void test(char* testName, int number1, int number2, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    if(bitsToModify(number1, number2) == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
void test1()
{
    int number1 = 0;
    int number2 = 13;
    int expected = 3;
    
    test("Test1", number1, number2, expected);
}
void test2()
{
    int number1 = 0;
    int number2 = 15;
    int expected = 4;
    
    test("Test2", number1, number2, expected);
}
void test3()
{
    int number1 = 7;
    int number2 = 0;
    int expected = 3;
    
    test("Test3", number1, number2, expected);
}
void test4()
{
    int number1 = 7;
    int number2 = 7;
    int expected = 0;
    
    test("Test4", number1, number2, expected);
}
void test5()
{
    int number1 = 7;
    int number2 = 15;
    int expected = 1;
    
    test("Test5", number1, number2, expected);
}
int main(int argc, char* argv[])
{
    test1();
    test2();
    test3();
    test4();
    test5();
}
```

## 038. NumbersOccuringOnce


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class NumbersOccuringOnce {
    public class NumbersOccurringOnce {
        public int num1;
        public int num2;
    }
    
    public static void getOnce(int numbers[], NumbersOccurringOnce once){
        if (numbers.length < 2)
            return;
     
        int resultExclusiveOR = 0;
        for (int i = 0; i < numbers.length; ++ i)
            resultExclusiveOR ^= numbers[i];
     
        int indexOf1 = findFirstBitIs1(resultExclusiveOR);    
        once.num1 = once.num2 = 0;
        for (int j = 0; j < numbers.length; ++ j) {
            if(isBit1(numbers[j], indexOf1))
                once.num1 ^= numbers[j];
            else
                once.num2 ^= numbers[j];
        }
    }
     
    // The first 1 bit from the rightmost
    private static int findFirstBitIs1(int num){
        int indexBit = 0;
        while (((num & 1) == 0) && (indexBit < 32)) {
            num = num >> 1;
            ++ indexBit;
        }
     
        return indexBit;
    }
    // check whether the bit with index indexBit is 1
    private static boolean isBit1(int num, int indexBit) {
        num = num >> indexBit;
        return (num & 1) == 1;
    }
    
    //================= Test Code =================
    private static void test(String testName, int[] numbers, NumbersOccurringOnce expected) {
        System.out.print(testName + " begins: ");
        NumbersOccuringOnce numbersOnce = new NumbersOccuringOnce();
        NumbersOccurringOnce once = numbersOnce.new NumbersOccurringOnce();
        once.num1 = Integer.MIN_VALUE;
        once.num2 = Integer.MAX_VALUE;
        
        getOnce(numbers, once);
        
        if((once.num1 == expected.num1 && once.num2 == expected.num2) 
                || (once.num1 == expected.num2 && once.num2 == expected.num1))
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    
    private static void test1() {
        int numbers[] = {2, 4, 3, 6, 3, 2, 5, 5};
        NumbersOccuringOnce numbersOnce = new NumbersOccuringOnce();
        NumbersOccurringOnce expected = numbersOnce.new NumbersOccurringOnce();
        expected.num1 = 4;
        expected.num2 = 6;
        
        test("test1", numbers, expected);
    }
    
    private static void test2() {
        int numbers[] = {4, 6};
        NumbersOccuringOnce numbersOnce = new NumbersOccuringOnce();
        NumbersOccurringOnce expected = numbersOnce.new NumbersOccurringOnce();
        expected.num1 = 4;
        expected.num2 = 6;
        
        test("test2", numbers, expected);
    }
    
    private static void test3() {
        int numbers[] = {4, 6, 1, 1, 1, 1};
        NumbersOccuringOnce numbersOnce = new NumbersOccuringOnce();
        NumbersOccurringOnce expected = numbersOnce.new NumbersOccurringOnce();
        expected.num1 = 4;
        expected.num2 = 6;
        
        test("test3", numbers, expected);
    }
    
    public static void main(String args[]) {
        test1();
        test2();
        test3();
    }
}
```

## 039. FindTwoMissingNumbers


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class FindTwoMissingNumbers {
    public class NumbersOccurringOnce {
        public int num1;
        public int num2;
    }
    //================= Solution 1 =================
    public static void findMissing_solution1(int numbers[], NumbersOccurringOnce missing){
        int sum1 = 0;
        int product1 = 1;
        for(int i = 0; i < numbers.length; ++i){
            sum1 += numbers[i];
            product1 *= numbers[i];
        }
        
        int sum2 = 0;
        int product2 = 1;
        for(int i = 1; i <= numbers.length + 2; ++i){
            sum2 += i;
            product2 *= i;
        }
        
        int s = sum2 - sum1;
        int p = product2 / product1;
        
        missing.num1 = (s + (int)(Math.sqrt(s * s - 4 * p))) / 2;
        missing.num2 = s - missing.num1;
    }
    
    //================= Solution 2 =================
    public static void findMissing_solution2(int numbers[], NumbersOccurringOnce missing){
        int originalLength = numbers.length;
        int extendedLength = originalLength * 2 + 2;
        int extention[] = new int[extendedLength];
        for(int i = 0; i < originalLength; ++i)
            extention[i] = numbers[i];
        for(int i = originalLength; i < extendedLength; ++i)
            extention[i] = i - originalLength + 1;
        
        getOnce(extention, missing);
    }
    
    private static void getOnce(int numbers[], NumbersOccurringOnce once){
        if (numbers.length < 2)
            return;
     
        int resultExclusiveOR = 0;
        for (int i = 0; i < numbers.length; ++ i)
            resultExclusiveOR ^= numbers[i];
     
        int indexOf1 = findFirstBitIs1(resultExclusiveOR);    
        once.num1 = once.num2 = 0;
        for (int j = 0; j < numbers.length; ++ j) {
            if(isBit1(numbers[j], indexOf1))
                once.num1 ^= numbers[j];
            else
                once.num2 ^= numbers[j];
        }
    }
     
    // The first 1 bit from the rightmost
    private static int findFirstBitIs1(int num){
        int indexBit = 0;
        while (((num & 1) == 0) && (indexBit < 32)) {
            num = num >> 1;
            ++ indexBit;
        }
     
        return indexBit;
    }
    // check whether the bit with index indexBit is 1
    private static boolean isBit1(int num, int indexBit) {
        num = num >> indexBit;
        return (num & 1) == 1;
    }
    
    //================= Test Code =================
    private static void test(String testName, int[] numbers, NumbersOccurringOnce expected) {
        System.out.print(testName + " begins: ");
        FindTwoMissingNumbers missingNumbers = new FindTwoMissingNumbers();
        NumbersOccurringOnce missing = missingNumbers.new NumbersOccurringOnce();
        missing.num1 = Integer.MIN_VALUE;
        missing.num2 = Integer.MAX_VALUE;
        
        findMissing_solution1(numbers, missing);
        
        if((missing.num1 == expected.num1 && missing.num2 == expected.num2) 
                || (missing.num1 == expected.num2 && missing.num2 == expected.num1))
            System.out.print("Solution1 passed; ");
        else
            System.out.print("Solution1 FAILED; ");
        
        findMissing_solution2(numbers, missing);
        
        if((missing.num1 == expected.num1 && missing.num2 == expected.num2) 
                || (missing.num1 == expected.num2 && missing.num2 == expected.num1))
            System.out.print("Solution2 passed.\n");
        else
            System.out.print("Solution2 FAILED.\n");
    }
    
    private static void test1() {
        int numbers[] = {1, 3, 5, 6, 4, 7};
        FindTwoMissingNumbers missingNumbers = new FindTwoMissingNumbers();
        NumbersOccurringOnce expected = missingNumbers.new NumbersOccurringOnce();
        expected.num1 = 2;
        expected.num2 = 8;
        
        test("test1", numbers, expected);
    }
    
    private static void test2() {
        int numbers[] = {1};
        FindTwoMissingNumbers missingNumbers = new FindTwoMissingNumbers();
        NumbersOccurringOnce expected = missingNumbers.new NumbersOccurringOnce();
        expected.num1 = 2;
        expected.num2 = 3;
        
        test("test2", numbers, expected);
    }
    
    private static void test3() {
        int numbers[] = {3, 4};
        FindTwoMissingNumbers missingNumbers = new FindTwoMissingNumbers();
        NumbersOccurringOnce expected = missingNumbers.new NumbersOccurringOnce();
        expected.num1 = 1;
        expected.num2 = 2;
        
        test("test3", numbers, expected);
    }
    
    public static void main(String args[]) {
        test1();
        test2();
        test3();
    }
}
```

## 040. Power


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <math.h>
#include <cerrno>
bool Equal(double num1, double num2);
double PowerWithUnsignedExponent(double base, unsigned int exponent);
 
double Power(double base, int exponent)
{
    errno = 0;
 
    if(Equal(base, 0.0) && exponent < 0)
    {
        errno = EDOM;
        return 0.0;
    }
 
    unsigned int absExponent = (unsigned int)(exponent);
    if(exponent < 0)
        absExponent = (unsigned int)(-exponent);
 
    double result = PowerWithUnsignedExponent(base, absExponent);
    if(exponent < 0)
        result = 1.0 / result;
 
    return result;
}
 
/*
double PowerWithUnsignedExponent(double base, unsigned int exponent)
{
    double result = 1.0;
    
    for(int i = 1; i <= exponent; ++i)
        result *= base;
 
    return result;
}
*/
double PowerWithUnsignedExponent(double base, unsigned int exponent)
{
    if(exponent == 0)
        return 1;
    if(exponent == 1)
        return base;
    double result = PowerWithUnsignedExponent(base, exponent >> 1);
    result *= result;
    if((exponent & 0x1) == 1)
        result *= base;
    return result;
}
bool Equal(double num1, double num2)
{
    if((num1 - num2 > -0.0000001)
        && (num1 - num2 < 0.0000001))
        return true;
    else
        return false;
}
// ==================== Test Code ====================
void Test(char* testName, double base, int exponent, double expectedResult, int expectedFlag)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    double result = Power(base, exponent);
    if(abs(result - expectedResult) < 0.00000001 && errno == expectedFlag)
        printf("Test passed.\n");
    else
        printf("Test failed.\n");
}
int main(int argc, char* argv[])
{
    Test("Test1", 2, 3, 8, 0);
    Test("Test2", -2, 3, -8, 0);
    Test("Test3", 2, -3, 0.125, 0);
    Test("Test4", 2, 0, 1, 0);
    Test("Test5", 0, 0, 1, 0);
    Test("Test6", 0, 4, 0, 0);
    Test("Test7", 0, -4, 0, EDOM);
    return 0;
}
```

## 041. Print1ToMaxOfNDigits


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <memory>
void PrintNumber(char* number);
bool Increment(char* number, int length);
void Print1ToMaxOfNDigitsCore(char* number, int length, int index);
// ==================== Solution 1====================
void Print1ToMaxOfNDigits_1(int n)
{
    if(n <= 0)
        return;
 
    char *number = new char[n + 1];
    memset(number, '0', n);
    number[n] = '\0';
 
    while(!Increment(number, n))
    {
        PrintNumber(number);
    }
 
    delete []number;
}
 
bool Increment(char* number, int length)
{
    bool isOverflow = false;
    int carry = 0;
 
    for(int i = length - 1; i >= 0; i --)
    {
        int sum = number[i] - '0' + carry;
        if(i == length - 1)
            sum ++;
 
        if(sum >= 10)
        {
            if(i == 0)
                isOverflow = true;
            else
            {
                sum -= 10;
                carry = 1;
                number[i] = '0' + sum;
            }
        }
        else
        {
            number[i] = '0' + sum;
            break;
        }
    }
 
    return isOverflow;
}
// ==================== Solution 2 ====================
void Print1ToMaxOfNDigits_2(int n)
{
    if(n <= 0)
        return;
 
    char* number = new char[n + 1];
    number[n] = '\0';
    Print1ToMaxOfNDigitsCore(number, n, -1);
 
    delete[] number;
}
 
void Print1ToMaxOfNDigitsCore(char* number, int length, int index)
{
    if(index == length - 1)
    {
        PrintNumber(number);
        return;
    }
 
    for(int i = 0; i < 10; ++i)
    {
        number[index + 1] = i + '0';
        Print1ToMaxOfNDigitsCore(number, length, index + 1);
    }
}
// ==================== Common Function ====================
void PrintNumber(char* number)
{
    char* pChar = number;
    while(*pChar == '0')
        ++pChar;
 
    if(*pChar != '\0')
        printf("%s\t", pChar);
}
// ==================== Test Code ====================
void Test(int n)
{
    printf("Test for %d begins:\n", n);
    Print1ToMaxOfNDigits_1(n);
    printf("\n");
    
    Print1ToMaxOfNDigits_2(n);
    printf("\n");
    printf("Test for %d ends.\n", n);
}
int main(int argc, char* argv[])
{
    Test(1);
    Test(2);
    Test(3);
    Test(0);
    Test(-1);
    return 0;
}
```

## 042. AddNumericStrings


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string.h>
int checkInvalidInput(char* num1, char* num2, char* sum);
void reverse(char* str);
/* It returns -1 if the input invalid, otherwise returns 0. */
int add(char* num1, char* num2, char* sum)
{
    int index1, index2, indexSum;
    int sumDigit, carry, digit1, digit2;
    
    if(checkInvalidInput(num1, num2, sum))
        return -1;
    
    reverse(num1);
    reverse(num2);
    
    index1 = index2 = indexSum = 0;
    carry = 0;
    while(num1[index1] != '\0' || num2[index2] != '\0')
    {
         digit1 = (num1[index1] == '\0') ? 0 : num1[index1] - '0';
         digit2 = (num2[index2] == '\0') ? 0 : num2[index2] - '0';
         
         sumDigit = digit1 + digit2 + carry;
         carry = (sumDigit >= 10) ? 1 : 0;
         sumDigit = (sumDigit >= 10) ? sumDigit - 10 : sumDigit;
         
         sum[indexSum++] = sumDigit + '0';
         
         if(num1[index1] != '\0')
            ++index1;
         if(num2[index2] != '\0')
            ++index2;
    }
    
    if(carry != 0)
        sum[indexSum++] = carry + '0';
    
    sum[indexSum] = '\0';    
    reverse(sum);
    
    return 0;
}
int checkInvalidInput(char* num1, char* num2, char* sum)
{
    int length1, length2, i;
    
    if(num1 == NULL || num2 == NULL || sum == NULL)
        return -1;
    
    length1 = strlen(num1);
    for(i = 0; i < length1; ++i)
    {
        if(num1[i] < '0' || num1[i] > '9')
            return -1;
    }
    
    length2 = strlen(num2);
    for(i = 0; i < length2; ++i)
    {
        if(num2[i] < '0' || num2[i] > '9')
            return -1;
    }
    
    return 0;
}
void reverse(char* str)
{
    int i, length;
    char temp;
    
    length = strlen(str);
    for(i = 0; i < length / 2; ++i)
    {
        temp = str[i];
        str[i] = str[length - 1 - i];
        str[length - 1 - i] = temp;
    }
}
// ==================== Test Code ====================
void test(char* testName, char* num1, char* num2, char* sum, char* expected, int valid)
{
    int result;
    
    if(testName != NULL)
        printf("%s begins: ", testName);
    result = add(num1, num2, sum);
    if((result == -1 && valid == -1) || (valid == 0 && strcmp(sum, expected) == 0))
        printf("Passed.\n");
    else
        printf("Failed.\n");
}
void test1()
{
    char num1[] = "999";
    char num2[] = "3";
    char sum[20];
    char* expected = "1002";
    test("Test1", num1, num2, sum, expected, 0);
}
void test2()
{
    char num1[] = "33";
    char num2[] = "9999";
    char sum[20];
    char* expected = "10032";
    test("Test2", num1, num2, sum, expected, 0);
}
void test3()
{
    char num1[] = "3333333";
    char num2[] = "222";
    char sum[20];
    char* expected = "3333555";
    test("Test3", num1, num2, sum, expected, 0);
}
void test4()
{
    char num1[] = "33abc33";
    char num2[] = "222";
    char sum[20];
    char* expected = "";
    test("Test4", num1, num2, sum, expected, -1);
}
void test5()
{
    test("Test5", NULL, NULL, NULL, NULL, -1);
}
void test6()
{
    char num1[] = "3333333344445555";
    char num2[] = "222222222222222";
    char sum[20];
    char* expected = "3555555566667777";
    test("Test6", num1, num2, sum, expected, 0);
}
int main(int argc, char* argv[])
{
    test1();
    test2();
    test3();
    test4();
    test5();
    test6();
    
    return 0;
}
```

## 043. DeleteNodeInList


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\List.h"
void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted)
{
    if(!pListHead || !pToBeDeleted)
        return;
    // The node to be deleted is not the tail of the list.
    if(pToBeDeleted->m_pNext != NULL)
    {
        ListNode* pNext = pToBeDeleted->m_pNext;
        pToBeDeleted->m_nValue = pNext->m_nValue;
        pToBeDeleted->m_pNext = pNext->m_pNext;
 
        delete pNext;
        pNext = NULL;
    }
    // The list has only one note. Delete the only node.
    else if(*pListHead == pToBeDeleted)
    {
        delete pToBeDeleted;
        pToBeDeleted = NULL;
        *pListHead = NULL;
    }
    // Delete the tail node of a list with multiple nodes
    else
    {
        ListNode* pNode = *pListHead;
        while(pNode->m_pNext != pToBeDeleted)
        {
            pNode = pNode->m_pNext;            
        }
 
        pNode->m_pNext = NULL;
        delete pToBeDeleted;
        pToBeDeleted = NULL;
    }
}
// ==================== Test Code ====================
void Test(ListNode* pListHead, ListNode* pNode)
{
    printf("The original list is: \n");
    PrintList(pListHead);
    printf("The node to be deleted is: \n");
    PrintListNode(pNode);
    DeleteNode(&pListHead, pNode);
    
    printf("The result list is: \n");
    PrintList(pListHead);
    printf("\n\n");
}
// Delete a node
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test(pNode1, pNode3);
    DestroyList(pNode1);
}
// Delete a tail
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test(pNode1, pNode5);
    DestroyList(pNode1);
}
// Delete a header node
void Test3()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test(pNode1, pNode1);
    DestroyList(pNode1);
}
// Delete the only node
void Test4()
{
    ListNode* pNode1 = CreateListNode(1);
    Test(pNode1, pNode1);
}
// empty list
void Test5()
{
    Test(NULL, NULL);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    return 0;
}
```

## 044. DeleteDuplicationInList


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "../Utility/list.h"
void deleteDuplication(ListNode** pHead)
{
    if(pHead == NULL || *pHead == NULL)
        return;
        
    ListNode* pPreNode = NULL;
    ListNode* pNode = *pHead;
    while(pNode != NULL)
    {
        ListNode *pNext = pNode->m_pNext;
        bool needDelete = false;
        if(pNext != NULL && pNext->m_nValue == pNode->m_nValue)
            needDelete = true;
        if(!needDelete)
        {
            pPreNode = pNode;
            pNode = pNode->m_pNext;
        }
        else
        {
            int value = pNode->m_nValue;    
            ListNode* pToBeDeleted = pNode;
            while(pToBeDeleted != NULL && pToBeDeleted->m_nValue == value)
            {
                pNext = pToBeDeleted->m_pNext;
                
                delete pToBeDeleted;
                pToBeDeleted = NULL;
                
                pToBeDeleted = pNext;
            }
            
            if(pPreNode == NULL)
                *pHead = pNext;
            else
                pPreNode->m_pNext = pNext;
            pNode = pNext;
        }
    }
}
// ==================== Test Code ====================
void Test(char* testName, ListNode** pHead, int* expectedValues, int expectedLength)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    deleteDuplication(pHead);
    
    int index = 0;
    ListNode* pNode = *pHead;
    while(pNode != NULL && index < expectedLength)
    {
        if(pNode->m_nValue != expectedValues[index])
            break;
            
        pNode = pNode->m_pNext;
        index++;
    }
    
    if(pNode == NULL && index == expectedLength)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
// some nodes are duplicated
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(3);
    ListNode* pNode5 = CreateListNode(4);
    ListNode* pNode6 = CreateListNode(4);
    ListNode* pNode7 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ListNode* pHead = pNode1;
    
    int expectedValues[] = {1, 2, 5};
    Test("Test1", &pHead, expectedValues, sizeof(expectedValues)/sizeof(int));
    
    DestroyList(pHead);
}
// all nodes are unique
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ListNode* pNode7 = CreateListNode(7);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ListNode* pHead = pNode1;
    
    int expectedValues[] = {1, 2, 3, 4, 5, 6, 7};
    Test("Test2", &pHead, expectedValues, sizeof(expectedValues)/sizeof(int));
    
    DestroyList(pHead);
}
// all nodes are duplicated except one
void Test3()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(1);
    ListNode* pNode4 = CreateListNode(1);
    ListNode* pNode5 = CreateListNode(1);
    ListNode* pNode6 = CreateListNode(1);
    ListNode* pNode7 = CreateListNode(2);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ListNode* pHead = pNode1;
    
    int expectedValues[] = {2};
    Test("Test3", &pHead, expectedValues, sizeof(expectedValues)/sizeof(int));
    
    DestroyList(pHead);
}
// all nodes are duplicated
void Test4()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(1);
    ListNode* pNode4 = CreateListNode(1);
    ListNode* pNode5 = CreateListNode(1);
    ListNode* pNode6 = CreateListNode(1);
    ListNode* pNode7 = CreateListNode(1);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ListNode* pHead = pNode1;
    
    Test("Test4", &pHead, NULL, 0);
    
    DestroyList(pHead);
}
// all nodes are duplicated in pairs
void Test5()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(2);
    ListNode* pNode4 = CreateListNode(2);
    ListNode* pNode5 = CreateListNode(3);
    ListNode* pNode6 = CreateListNode(3);
    ListNode* pNode7 = CreateListNode(4);
    ListNode* pNode8 = CreateListNode(4);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ConnectListNodes(pNode7, pNode8);
    ListNode* pHead = pNode1;
    
    Test("Test5", &pHead, NULL, 0);
    
    DestroyList(pHead);
}
// nodes are duplicated in pairs except two
void Test6()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(1);
    ListNode* pNode3 = CreateListNode(2);
    ListNode* pNode4 = CreateListNode(3);
    ListNode* pNode5 = CreateListNode(3);
    ListNode* pNode6 = CreateListNode(4);
    ListNode* pNode7 = CreateListNode(5);
    ListNode* pNode8 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ConnectListNodes(pNode7, pNode8);
    ListNode* pHead = pNode1;
    
    int expectedValues[] = {2, 4};
    Test("Test6", &pHead, expectedValues, sizeof(expectedValues)/sizeof(int));
    
    DestroyList(pHead);
}
// a list with two unique nodes
void Test7()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ConnectListNodes(pNode1, pNode2);
    ListNode* pHead = pNode1;
    
    int expectedValues[] = {1, 2};
    Test("Test7", &pHead, expectedValues, sizeof(expectedValues)/sizeof(int));
    
    DestroyList(pHead);
}
// only one node in a list
void Test8()
{
    ListNode* pNode1 = CreateListNode(1);
    ConnectListNodes(pNode1, NULL);
    ListNode* pHead = pNode1;
    
    int expectedValues[] = {1};
    Test("Test8", &pHead, expectedValues, sizeof(expectedValues)/sizeof(int));
    
    DestroyList(pHead);
}
// a list with only two duplidated nodes
void Test9()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(1);
    ConnectListNodes(pNode1, pNode2);
    ListNode* pHead = pNode1;
    
    Test("Test9", &pHead, NULL, 0);
    
    DestroyList(pHead);
}
// empty list
void Test10()
{
    ListNode* pHead = NULL;
    
    Test("Test10", &pHead, NULL, 0);
}
int main(int argc, char* argv[])
{
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

## 045. ReorderNumbers


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class ReorderNumbers {
    //================= Solution 1 =================
   public static void reorderOddEven_solution1(int nums[]) {
        int begin = 0;
        int end = nums.length - 1;
        while(begin < end) {
            // Move begin forwards until it meets an even number
            while(begin < end && (nums[begin] & 0x1) != 0)
                begin++;
            
            // Move end backwards until it meets an odd number
            while(begin < end && (nums[end] & 0x1) == 0)
                end--;
            
            if(begin < end) {
                int temp = nums[begin];
                nums[begin] = nums[end];
                nums[end] = temp;
            }
        }
    }
    
   //================= Solution 2 =================
    public static void reorderOddEven_solution2(int nums[]) {
        Criterion isEven = new Criterion() {
            public boolean check(int num) {
                if((num & 0x1) == 0)
                    return true;
               
                return false;
            }
        };
       
        reorder(nums, isEven);
    }
   
    public interface Criterion{
        boolean check(int num);
    }
    private static void reorder(int nums[], Criterion criterion) {
        int begin = 0;
        int end = nums.length - 1;
        while(begin < end) {
            while(begin < end && !criterion.check(nums[begin]))
                begin++;
            
            while(begin < end && criterion.check(nums[end]))
                end--;
            
            if(begin < end) {
                int temp = nums[begin];
                nums[begin] = nums[end];
                nums[end] = temp;
            }
        }
    }
    
    //================= Test Code =================
    private static void test(String testName, int nums[]) {
        System.out.print(testName + " begins: ");
        
        int copy[] = new int[nums.length];
        for(int i = 0; i < nums.length; ++i)
            copy[i] = nums[i];
        
        reorderOddEven_solution1(nums);
        if(checkOddBeforeEven(nums))
            System.out.print("Solution1 Passed; ");
        else
            System.out.print("Solution1 FAILED; ");
        
        reorderOddEven_solution2(copy);
        if(checkOddBeforeEven(copy))
            System.out.print("Solution2 Passed.\n");
        else
            System.out.print("Solution2 FAILED.\n");
    }
    
    private static boolean checkOddBeforeEven(int nums[]) {
        int i = 0;
        while(i < nums.length) {
            if((nums[i] & 0x01) == 0)
                break;
            
            ++i;
        }
        
        while(i < nums.length) {
            if((nums[i] & 0x01) == 1)
                break;
            
            ++i;
        }
        
        return (i == nums.length);
    }
    
    private static void test1() {
        int nums[] = {1, 2, 3, 4, 5, 6, 7};
        test("Test1", nums);
    }
    
    private static void test2() {
        int nums[] = {2, 4, 6, 1, 3, 5, 7};
        test("Test2", nums);
    }
    
    private static void test3() {
        int nums[] = {1, 3, 5, 7, 2, 4, 6};
        test("Test3", nums);
    }
    
    private static void test4() {
        int nums[] = {1};
        test("Test4", nums);
    }
    
    private static void test5() {
        int nums[] = {2};
        test("Test5", nums);
    }
    
    public static void main(String args[]) {
        test1();
        test2();
        test3();
        test4();
        test5();
    }
}
```

## 046. RemoveNumbers


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class RemoveNumbers {
    public static int remove_solution1(int numbers[], int n) {
        int i = 0;
        for (int j = 0; j < numbers.length; j++) {
            if (numbers[j] != n) 
                numbers[i++] = numbers[j];
        }
        return i;
    }
    
    public static int remove_solution2(int numbers[], int n) {
        int p1 = 0;
        while(p1 < numbers.length && numbers[p1] != n)
            ++p1;
        int p2 = numbers.length - 1;
        while(p1 < p2){
            while(p1 < numbers.length && numbers[p1] != n)
                ++p1;
            while(p2 > 0 && numbers[p2] == n)
                --p2;
            if(p1 < p2){
                numbers[p1] = numbers[p2];
                numbers[p2] = n;
            }
        }
        return p1;        
    }
    
    //================= Test Code =================
    private static boolean checkRemoved(int numbers[], int n, int expected) {
        int i = 0;
        while(i < expected)
        {
            if(numbers[i] == n)
                break;
            
            ++i;
        }
        
        return (i == expected);
    }
    
    private static void test(String testName, int nums[], int n, int expected) {
        System.out.print(testName + " begins: ");
        
        int copy[] = new int[nums.length];
        for(int i = 0; i < nums.length; ++i)
            copy[i] = nums[i];
        
        int result1 = remove_solution1(nums, n);
        if((result1 == expected) && checkRemoved(nums, n, expected))
            System.out.print("Solution1 Passed; ");
        else
            System.out.print("Solution1 FAILED; ");
        
        int result2 = remove_solution2(copy, n);
        if((result2 == expected) && checkRemoved(copy, n, expected))
            System.out.print("Solution2 Passed.\n");
        else
            System.out.print("Solution2 FAILED.\n");
    }
    // multiple targets to be deleted
    private static void test1() {
        int numbers[] = {4, 3, 2, 1, 2, 3, 6};
        int n = 2;
        int expected = 5;
        test("Test1", numbers, n, expected);
    }
    // targets are the first and last elements
    private static void test2() {
        int numbers[] = {4, 3, 2, 1, 2, 3, 4};
        int n = 4;
        int expected = 5;
        test("Test2", numbers, n, expected);
    }
    // all elements are targets to be deleted
    private static void test3() {
        int numbers[] = {4, 4, 4, 4};
        int n = 4;
        int expected = 0;
        test("Test3", numbers, n, expected);
    }
    // no target to be deleted
    private static void test4() {
        int numbers[] = {4, 3, 2, 1, 2, 3, 6};
        int n = 5;
        int expected = 7;
        test("Test4", numbers, n, expected);
    }
    
    // the only number is the target
    private static void test5() {
        int numbers[] = {4};
        int n = 4;
        int expected = 0;
        test("Test5", numbers, n, expected);
    }
    
    // the only number is the target
    private static void test6() {
        int numbers[] = {4};
        int n = 5;
        int expected = 1;
        test("Test6", numbers, n, expected);
    }
    // all elements except one are targets to be deleted
    private static void test7() {
        int numbers[] = {4, 4, 3, 4};
        int n = 4;
        int expected = 1;
        test("Test7", numbers, n, expected);
    }
    
    public static void main(String args[]) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
        test7();
    }
}
```

## 047. KthNodeFromEnd


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\List.h"
ListNode* FindKthToTail(ListNode* pListHead, unsigned int k)
{
    if(pListHead == NULL || k == 0)
        return NULL;
    ListNode *pAhead = pListHead;
    ListNode *pBehind = NULL;
    for(unsigned int i = 0; i < k - 1; ++ i)
    {
        if(pAhead->m_pNext != NULL)
            pAhead = pAhead->m_pNext;
        else
        {
            return NULL;
        }
    }
    pBehind = pListHead;
    while(pAhead->m_pNext != NULL)
    {
        pAhead = pAhead->m_pNext;
        pBehind = pBehind->m_pNext;
    }
    return pBehind;
}
// ==================== Test Code ====================
void Test(char* testName, ListNode* pHead, int index, int expected, bool isNull)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    ListNode* pNode = FindKthToTail(pHead, index);
    if((isNull && pNode == NULL) || (!isNull && (pNode->m_nValue == expected)))
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
// The target node is inside the list
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test1", pNode1, 2, 4, false);
    DestroyList(pNode1);
}
// The target node is the tail node
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test2", pNode1, 1, 5, false);
    DestroyList(pNode1);
}
// The target node is the head node
void Test3()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test3", pNode1, 5, 1, false);
    DestroyList(pNode1);
}
// empty list
void Test4()
{
    Test("Test4", NULL, 100, 1, true);
}
// the input k is greater than the number of nodes in a list
void Test5()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test5", pNode1, 6, 1, true);
    DestroyList(pNode1);
}
// the input k is 0
void Test6()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test6", pNode1, 0, 1, true);
    DestroyList(pNode1);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    return 0;
}
```

## 048. ReverseList


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\List.h"
ListNode* ReverseList(ListNode* pHead)
{
    ListNode* pReversedHead = NULL;
    ListNode* pNode = pHead;
    ListNode* pPrev = NULL;
    while(pNode != NULL)
    {
        ListNode* pNext = pNode->m_pNext;
        if(pNext == NULL)
            pReversedHead = pNode;
        pNode->m_pNext = pPrev;
        pPrev = pNode;
        pNode = pNext;
    }
    return pReversedHead;
}
// ==================== Test Code ====================
ListNode* Test(ListNode* pHead)
{
    printf("The original list is: \n");
    PrintList(pHead);
    ListNode* pReversedHead = ReverseList(pHead);
    printf("The reversed list is: \n");
    PrintList(pReversedHead);
    
    printf("\n");
    return pReversedHead;
}
// Multiple nodes
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ListNode* pReversedHead = Test(pNode1);
    DestroyList(pReversedHead);
}
// Only one node
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pReversedHead = Test(pNode1);
    DestroyList(pReversedHead);
}
// Empty list
void Test3()
{
    Test(NULL);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    return 0;
}
```

## 049. ReverseListInGroups


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "../Utility/list.h"
void ReverseGroup(ListNode* pNode1, ListNode* pNode2);
ListNode* Reverse(ListNode* pHead, unsigned int k)
{
    if(pHead == NULL || k <= 1)
        return pHead;
    ListNode* pReversedHead = NULL;
    ListNode* pNode1 = pHead;
    ListNode* pPrev = NULL;
    while(pNode1 != NULL)
    {
        // find k nodes within a group
        ListNode* pNode2 = pNode1;
        ListNode* pNext = NULL;
        for(unsigned int i = 1; pNode2->m_pNext != NULL && i < k; ++i)
            pNode2 = pNode2->m_pNext;
        pNext = pNode2->m_pNext;
        // reverse nodes within a group
        ReverseGroup(pNode1, pNode2);
        // connect groups together
        if(pReversedHead == NULL)
            pReversedHead = pNode2;
        if(pPrev != NULL)
            pPrev->m_pNext = pNode2;
        pPrev = pNode1;
        pNode1 = pNext;
    }
    return pReversedHead;
}
void ReverseGroup(ListNode* pNode1, ListNode* pNode2)
{
    ListNode* pNode = pNode1;
    ListNode* pPrev = NULL;
    while(pNode != pNode2)
    {
        ListNode* pNext = pNode->m_pNext;
        pNode->m_pNext = pPrev;
        pPrev = pNode;
        pNode = pNext;
    }
    
    pNode->m_pNext = pPrev;
}
// ==================== Test Code ====================
ListNode* Test(char* testName, ListNode* pHead, unsigned int k)
{
    if(testName != NULL)
        printf("====%s begins: ====\n", testName);
    printf("The original list is: ");
    PrintList(pHead);
    
    printf("The reversed list is: ");
    ListNode* pReversedHead = Reverse(pHead, k);
    PrintList(pReversedHead);
    printf("\n\n");
    return pReversedHead;
}
ListNode* BuildList()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ListNode* pNode7 = CreateListNode(7);
    ListNode* pNode8 = CreateListNode(8);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    ConnectListNodes(pNode7, pNode8);
    return pNode1;
}
void Test1()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test1", pHead, 1);
    DestroyList(pReversedHead);
}
void Test2()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test2", pHead, 2);
    DestroyList(pReversedHead);
}
void Test3()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test3", pHead, 3);
    DestroyList(pReversedHead);
}
void Test4()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test4", pHead, 4);
    DestroyList(pReversedHead);
}
void Test5()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test5", pHead, 5);
    DestroyList(pReversedHead);
}
void Test6()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test6", pHead, 6);
    DestroyList(pReversedHead);
}
void Test7()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test7", pHead, 7);
    DestroyList(pReversedHead);
}
void Test8()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test8", pHead, 8);
    DestroyList(pReversedHead);
}
void Test9()
{
    ListNode* pHead = BuildList();
    ListNode* pReversedHead = Test("Test9", pHead, 9);
    DestroyList(pReversedHead);
}
void Test10()
{
    ListNode* pHead = NULL;
    ListNode* pReversedHead = Test("Test10", pHead, 0);
    DestroyList(pReversedHead);
}
int main(int argc, char* argv[])
{
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

## 050. SubtreeInTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
bool HasSubtreeCore(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2);
bool DoesTree1HaveTree2(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2);
bool HasSubtree(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2)
{
    bool result = false;
    if(pRoot1 != NULL && pRoot2 != NULL)
    {
        if(pRoot1->m_nValue == pRoot2->m_nValue)
            result = DoesTree1HaveTree2(pRoot1, pRoot2);
        if(!result)
            result = HasSubtree(pRoot1->m_pLeft, pRoot2);
        if(!result)
            result = HasSubtree(pRoot1->m_pRight, pRoot2);
    }
    return result;
}
bool DoesTree1HaveTree2(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2)
{
    if(pRoot2 == NULL)
        return true;
    if(pRoot1 == NULL)
        return false;
    if(pRoot1->m_nValue != pRoot2->m_nValue)
        return false;
    return DoesTree1HaveTree2(pRoot1->m_pLeft, pRoot2->m_pLeft) &&
        DoesTree1HaveTree2(pRoot1->m_pRight, pRoot2->m_pRight);
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2, bool expected)
{
    if(HasSubtree(pRoot1, pRoot2) == expected)
        printf("%s passed.\n", testName);
    else
        printf("%s failed.\n", testName);
}
// There are branches in trees, and tree B is a subtree of tree A
//                  8                8
//              /       \           / \
//             8         7         9   2
//           /   \
//          9     2
//               / \
//              4   7
void Test1()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA5 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNodeA6 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNodeA7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNodeA1, pNodeA2, pNodeA3);
    ConnectTreeNodes(pNodeA2, pNodeA4, pNodeA5);
    ConnectTreeNodes(pNodeA5, pNodeA6, pNodeA7);
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeB1, pNodeB2, pNodeB3);
    Test("Test1", pNodeA1, pNodeB1, true);
    DestroyTree(pNodeA1);
    DestroyTree(pNodeB1);
}
// There are branches in trees, and tree B is not a subtree of tree A
//                  8                8
//              /       \           / \
//             8         7         9   2
//           /   \
//          9     3
//               / \
//              4   7
void Test2()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA5 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNodeA6 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNodeA7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNodeA1, pNodeA2, pNodeA3);
    ConnectTreeNodes(pNodeA2, pNodeA4, pNodeA5);
    ConnectTreeNodes(pNodeA5, pNodeA6, pNodeA7);
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeB1, pNodeB2, pNodeB3);
    Test("Test2", pNodeA1, pNodeB1, false);
    DestroyTree(pNodeA1);
    DestroyTree(pNodeB1);
}
// Nodes in trees only have left children, and tree B is a subtree of tree A
//                8                  8
//              /                   / 
//             8                   9   
//           /                    /
//          9                    2
//         /      
//        2        
//       /
//      5
void Test3()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNodeA5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNodeA1, pNodeA2, NULL);
    ConnectTreeNodes(pNodeA2, pNodeA3, NULL);
    ConnectTreeNodes(pNodeA3, pNodeA4, NULL);
    ConnectTreeNodes(pNodeA4, pNodeA5, NULL);
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeB1, pNodeB2, NULL);
    ConnectTreeNodes(pNodeB2, pNodeB3, NULL);
    Test("Test3", pNodeA1, pNodeB1, true);
    DestroyTree(pNodeA1);
    DestroyTree(pNodeB1);
}
// Nodes in trees only have left children, and tree B is not a subtree of tree A
//                8                  8
//              /                   / 
//             8                   9   
//           /                    /
//          9                    3
//         /      
//        2        
//       /
//      5
void Test4()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNodeA5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNodeA1, pNodeA2, NULL);
    ConnectTreeNodes(pNodeA2, pNodeA3, NULL);
    ConnectTreeNodes(pNodeA3, pNodeA4, NULL);
    ConnectTreeNodes(pNodeA4, pNodeA5, NULL);
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(3);
    ConnectTreeNodes(pNodeB1, pNodeB2, NULL);
    ConnectTreeNodes(pNodeB2, pNodeB3, NULL);
    Test("Test4", pNodeA1, pNodeB1, false);
    DestroyTree(pNodeA1);
    DestroyTree(pNodeB1);
}
// Nodes in trees only have right children, and tree B is a subtree of tree A
//       8                   8
//        \                   \ 
//         8                   9   
//          \                   \
//           9                   2
//            \      
//             2        
//              \
//               5
void Test5()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNodeA5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNodeA1, NULL, pNodeA2);
    ConnectTreeNodes(pNodeA2, NULL, pNodeA3);
    ConnectTreeNodes(pNodeA3, NULL, pNodeA4);
    ConnectTreeNodes(pNodeA4, NULL, pNodeA5);
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeB1, NULL, pNodeB2);
    ConnectTreeNodes(pNodeB2, NULL, pNodeB3);
    Test("Test5", pNodeA1, pNodeB1, true);
    DestroyTree(pNodeA1);
    DestroyTree(pNodeB1);
}
// Nodes in tree A only have right children, and tree B is not a subtree of tree A
//       8                   8
//        \                   \ 
//         8                   9   
//          \                 / \
//           9               3   2
//            \      
//             2        
//              \
//               5
void Test6()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNodeA5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNodeA1, NULL, pNodeA2);
    ConnectTreeNodes(pNodeA2, NULL, pNodeA3);
    ConnectTreeNodes(pNodeA3, NULL, pNodeA4);
    ConnectTreeNodes(pNodeA4, NULL, pNodeA5);
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNodeB4 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeB1, NULL, pNodeB2);
    ConnectTreeNodes(pNodeB2, pNodeB3, pNodeB4);
    Test("Test6", pNodeA1, pNodeB1, false);
    DestroyTree(pNodeA1);
    DestroyTree(pNodeB1);
}
// Tree A is empty
void Test7()
{
    BinaryTreeNode* pNodeB1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeB2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeB3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNodeB4 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeB1, NULL, pNodeB2);
    ConnectTreeNodes(pNodeB2, pNodeB3, pNodeB4);
    Test("Test7", NULL, pNodeB1, false);
    DestroyTree(pNodeB1);
}
// Tree B is empty
void Test8()
{
    BinaryTreeNode* pNodeA1 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNodeA2 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNodeA3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNodeA4 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNodeA1, NULL, pNodeA2);
    ConnectTreeNodes(pNodeA2, pNodeA3, pNodeA4);
    Test("Test8", pNodeA1, NULL, false);
    DestroyTree(pNodeA1);
}
// Two empty trees
void Test9()
{
    Test("Test9", NULL, NULL, false);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
    Test9();
    return 0;
}
```

## 051. MirrorOfBinaryTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <stack>
void MirrorRecursively(BinaryTreeNode *pNode)
{
    if(pNode == NULL)
        return; 
    if(pNode->m_pLeft == NULL && pNode->m_pRight == NULL)
        return;
    BinaryTreeNode *pTemp = pNode->m_pLeft;
    pNode->m_pLeft = pNode->m_pRight;
    pNode->m_pRight = pTemp;
    
    if(pNode->m_pLeft)
        MirrorRecursively(pNode->m_pLeft);  
    if(pNode->m_pRight)
        MirrorRecursively(pNode->m_pRight); 
}
void MirrorIteratively(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return;
    std::stack<BinaryTreeNode*> stackTreeNode;
    stackTreeNode.push(pRoot);
    while(stackTreeNode.size() > 0)
    {
        BinaryTreeNode *pNode = stackTreeNode.top();
        stackTreeNode.pop();
        BinaryTreeNode *pTemp = pNode->m_pLeft;
        pNode->m_pLeft = pNode->m_pRight;
        pNode->m_pRight = pTemp;
        if(pNode->m_pLeft)
            stackTreeNode.push(pNode->m_pLeft);
        if(pNode->m_pRight)
            stackTreeNode.push(pNode->m_pRight);
    }
}
// ==================== Test Code ====================
// full binary tree
//            8
//        6      10
//       5 7    9  11
void Test1()
{
    printf("=====Test1 starts:=====\n");
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    PrintTree(pNode8);
    printf("=====Test1: MirrorRecursively=====\n");
    MirrorRecursively(pNode8);
    PrintTree(pNode8);
    printf("=====Test1: MirrorIteratively=====\n");
    MirrorIteratively(pNode8);
    PrintTree(pNode8);
    DestroyTree(pNode8);
}
// Every node except the leaf has only a node as its left child
//            8
//          7   
//        6 
//      5
//    4
void Test2()
{
    printf("=====Test2 starts:=====\n");
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    ConnectTreeNodes(pNode8, pNode7, NULL);
    ConnectTreeNodes(pNode7, pNode6, NULL);
    ConnectTreeNodes(pNode6, pNode5, NULL);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    PrintTree(pNode8);
    printf("=====Test2: MirrorRecursively=====\n");
    MirrorRecursively(pNode8);
    PrintTree(pNode8);
    printf("=====Test2: MirrorIteratively=====\n");
    MirrorIteratively(pNode8);
    PrintTree(pNode8);
    DestroyTree(pNode8);
}
// Every node except the leaf has only a node as its right child
//            8
//             7   
//              6 
//               5
//                4
void Test3()
{
    printf("=====Test3 starts:=====\n");
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    ConnectTreeNodes(pNode8, NULL, pNode7);
    ConnectTreeNodes(pNode7, NULL, pNode6);
    ConnectTreeNodes(pNode6, NULL, pNode5);
    ConnectTreeNodes(pNode5, NULL, pNode4);
    PrintTree(pNode8);
    printf("=====Test3: MirrorRecursively=====\n");
    MirrorRecursively(pNode8);
    PrintTree(pNode8);
    printf("=====Test3: MirrorIteratively=====\n");
    MirrorIteratively(pNode8);
    PrintTree(pNode8);
    DestroyTree(pNode8);
}
// empty tree
void Test4()
{
    printf("=====Test4 starts:=====\n");
    BinaryTreeNode* pNode = NULL;
    PrintTree(pNode);
    printf("=====Test4: MirrorRecursively=====\n");
    MirrorRecursively(pNode);
    PrintTree(pNode);
    printf("=====Test4: MirrorIteratively=====\n");
    MirrorIteratively(pNode);
    PrintTree(pNode);
}
// only one node in a tree
void Test5()
{
    printf("=====Test5 starts:=====\n");
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    PrintTree(pNode8);
    printf("=====Test4: MirrorRecursively=====\n");
    MirrorRecursively(pNode8);
    PrintTree(pNode8);
    printf("=====Test4: MirrorIteratively=====\n");
    MirrorIteratively(pNode8);
    PrintTree(pNode8);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    return 0;
}
```

## 052. SymmetricalBinaryTrees


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "../Utility/BinaryTree.h"
bool isSymmetrical(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2);
bool isSymmetrical(BinaryTreeNode* pRoot)
{
    return isSymmetrical(pRoot, pRoot);
}
bool isSymmetrical(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2)
{
    if(pRoot1 == NULL && pRoot2 == NULL)
        return true;
    
    if(pRoot1 == NULL || pRoot2 == NULL)
        return false;
    
    if(pRoot1->m_nValue != pRoot2->m_nValue)
        return false;
        
    return isSymmetrical(pRoot1->m_pLeft, pRoot2->m_pRight)
        && isSymmetrical(pRoot1->m_pRight, pRoot2->m_pLeft);
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot, bool expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(isSymmetrical(pRoot) == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
//            8
//        6      6
//       5 7    7 5
void Test1()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode61 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode62 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode51 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode71 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode72 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode52 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode8, pNode61, pNode62);
    ConnectTreeNodes(pNode61, pNode51, pNode71);
    ConnectTreeNodes(pNode62, pNode72, pNode52);
    Test("Test1", pNode8, true);
    DestroyTree(pNode8);
}
//            8
//        6      9
//       5 7    7 5
void Test2()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode61 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode51 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode71 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode72 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode52 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode8, pNode61, pNode9);
    ConnectTreeNodes(pNode61, pNode51, pNode71);
    ConnectTreeNodes(pNode9, pNode72, pNode52);
    Test("Test2", pNode8, false);
    DestroyTree(pNode8);
}
//            8
//        6      6
//       5 7    7
void Test3()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode61 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode62 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode51 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode71 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode72 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNode8, pNode61, pNode62);
    ConnectTreeNodes(pNode61, pNode51, pNode71);
    ConnectTreeNodes(pNode62, pNode72, NULL);
    Test("Test3", pNode8, false);
    DestroyTree(pNode8);
}
//               5
//              / \
//             3   3
//            /     \
//           4       4
//          /         \
//         2           2
//        /             \
//       1               1
void Test4()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode31 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode32 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode41 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode42 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode21 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode22 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode31, pNode32);
    ConnectTreeNodes(pNode31, pNode41, NULL);
    ConnectTreeNodes(pNode32, NULL, pNode42);
    ConnectTreeNodes(pNode41, pNode21, NULL);
    ConnectTreeNodes(pNode42, NULL, pNode22);
    ConnectTreeNodes(pNode21, pNode11, NULL);
    ConnectTreeNodes(pNode22, NULL, pNode12);
    Test("Test4", pNode5, true);
    DestroyTree(pNode5);
}
//               5
//              / \
//             3   3
//            /     \
//           4       4
//          /         \
//         6           2
//        /             \
//       1               1
void Test5()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode31 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode32 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode41 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode42 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode22 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode31, pNode32);
    ConnectTreeNodes(pNode31, pNode41, NULL);
    ConnectTreeNodes(pNode32, NULL, pNode42);
    ConnectTreeNodes(pNode41, pNode6, NULL);
    ConnectTreeNodes(pNode42, NULL, pNode22);
    ConnectTreeNodes(pNode6, pNode11, NULL);
    ConnectTreeNodes(pNode22, NULL, pNode12);
    Test("Test5", pNode5, false);
    DestroyTree(pNode5);
}
//               5
//              / \
//             3   3
//            /     \
//           4       4
//          /         \
//         2           2
//                      \
//                       1
void Test6()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode31 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode32 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode41 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode42 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode21 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode22 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode31, pNode32);
    ConnectTreeNodes(pNode31, pNode41, NULL);
    ConnectTreeNodes(pNode32, NULL, pNode42);
    ConnectTreeNodes(pNode41, pNode21, NULL);
    ConnectTreeNodes(pNode42, NULL, pNode22);
    ConnectTreeNodes(pNode21, NULL, NULL);
    ConnectTreeNodes(pNode22, NULL, pNode12);
    Test("Test6", pNode5, false);
    DestroyTree(pNode5);
}
// Only one node
void Test7()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    Test("Test7", pNode1, true);
    DestroyTree(pNode1);
}
// No nodes
void Test8()
{
    Test("Test8", NULL, true);
}
void main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
}
```

## 053. PrintMatrix


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class PrintMatrix {
    public static void printMatrixClockwise(int numbers[][]) {
        int rows = numbers.length;
        int columns = numbers[0].length;
        int start = 0;
        while(columns > start * 2 && rows > start * 2) {
            printRing(numbers, start);
            ++start;
        }
    }
    private static void printRing(int numbers[][], int start)
    {
        int rows = numbers.length;
        int columns = numbers[0].length;
        int endX = columns - 1 - start;
        int endY = rows - 1 - start;
        // Print a row from left to right
        for(int i = start; i <= endX; ++i) {
            int number = numbers[start][i];
            printNumber(number);
        }
        // print a column top down
        if(start < endY) {
            for(int i = start + 1; i <= endY; ++i) {
                int number = numbers[i][endX];
                printNumber(number);
            }
        }
        // print a row from right to left
        if(start < endX && start < endY) {
            for(int i = endX - 1; i >= start; --i) {
                int number = numbers[endY][i];
                printNumber(number);
            }
        }
        // print a column bottom up
        if(start < endX && start < endY - 1) {
            for(int i = endY - 1; i >= start + 1; --i) {
                int number = numbers[i][start];
                printNumber(number);
            }
        }
    }
    private static void printNumber(int number) {
        System.out.print(number);
        System.out.print("\t");
    }
    
    //================= Test Code =================
    private static void test(int columns, int rows)
    {
        System.out.print("Test Begin: ");
        System.out.print(columns);
        System.out.print(" columns, ");
        System.out.print(rows);
        System.out.print(" rows.\n");
        if(columns < 1 || rows < 1)
            return;
        int numbers[][] = new int[rows][];
        for(int i = 0; i < rows; ++i) {
            numbers[i] = new int[columns];
            for(int j = 0; j < columns; ++j) {
                numbers[i][j] = i * columns + j + 1;
            }
        }
        printMatrixClockwise(numbers);
        System.out.print("\n");
    }
    
    public static void main(String[] args) {
        /*
        1    
        */
        test(1, 1);
        /*
        1    2
        3    4
        */
        test(2, 2);
        /*
        1    2    3    4
        5    6    7    8
        9    10   11   12
        13   14   15   16
        */
        test(4, 4);
        /*
        1    2    3    4    5
        6    7    8    9    10
        11   12   13   14   15
        16   17   18   19   20
        21   22   23   24   25
        */
        test(5, 5);
        /*
        1
        2
        3
        4
        5
        */
        test(1, 5);
        /*
        1    2
        3    4
        5    6
        7    8
        9    10
        */
        test(2, 5);
        /*
        1    2    3
        4    5    6
        7    8    9
        10   11   12
        13   14   15
        */
        test(3, 5);
        /*
        1    2    3    4
        5    6    7    8
        9    10   11   12
        13   14   15   16
        17   18   19   20
        */
        test(4, 5);
        /*
        1    2    3    4    5
        */
        test(5, 1);
        /*
        1    2    3    4    5
        6    7    8    9    10
        */
        test(5, 2);
        /*
        1    2    3    4    5
        6    7    8    9    10
        11   12   13   14    15
        */
        test(5, 3);
        /*
        1    2    3    4    5
        6    7    8    9    10
        11   12   13   14   15
        16   17   18   19   20
        */
        test(5, 4);
    }
}
```

## 054. CloneComplexList


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
struct ComplexListNode
{
    int                 m_nValue;
    ComplexListNode*    m_pNext;
    ComplexListNode*    m_pSibling;
};
void CloneNodes(ComplexListNode* pHead);
void ConnectSiblingNodes(ComplexListNode* pHead);
ComplexListNode* ReconnectNodes(ComplexListNode* pHead);
ComplexListNode* Clone(ComplexListNode* pHead)
{
    CloneNodes(pHead);
    ConnectSiblingNodes(pHead);
    return ReconnectNodes(pHead);
}
void CloneNodes(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    while(pNode != NULL)
    {
        ComplexListNode* pCloned = new ComplexListNode();
        pCloned->m_nValue = pNode->m_nValue;
        pCloned->m_pNext = pNode->m_pNext;
        pCloned->m_pSibling = NULL;
 
        pNode->m_pNext = pCloned;
        pNode = pCloned->m_pNext;
    }
}
void ConnectSiblingNodes(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    while(pNode != NULL)
    {
        ComplexListNode* pCloned = pNode->m_pNext;
        if(pNode->m_pSibling != NULL)
        {
            pCloned->m_pSibling = pNode->m_pSibling->m_pNext;
        }
 
        pNode = pCloned->m_pNext;
    }
}
ComplexListNode* ReconnectNodes(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    ComplexListNode* pClonedHead = NULL;
    ComplexListNode* pClonedNode = NULL;
 
    if(pNode != NULL)
    {
        pClonedHead = pClonedNode = pNode->m_pNext;
        pNode->m_pNext = pClonedNode->m_pNext;
        pNode = pNode->m_pNext;
    }
 
    while(pNode != NULL)
    {
        pClonedNode->m_pNext = pNode->m_pNext;
        pClonedNode = pClonedNode->m_pNext;
 
        pNode->m_pNext = pClonedNode->m_pNext;
        pNode = pNode->m_pNext;
    }
 
    return pClonedHead;
}
// ==================== Test Code ====================
ComplexListNode* CreateNode(int nValue)
{
    ComplexListNode* pNode = new ComplexListNode();
    
    pNode->m_nValue = nValue;
    pNode->m_pNext = NULL;
    pNode->m_pSibling = NULL;
    return pNode;
}
void BuildNodes(ComplexListNode* pNode, ComplexListNode* pNext, ComplexListNode* pSibling)
{
    if(pNode != NULL)
    {
        pNode->m_pNext = pNext;
        pNode->m_pSibling = pSibling;
    }
}
void PrintList(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    while(pNode != NULL)
    {
        printf("The value of this node is: %d.\n", pNode->m_nValue);
        if(pNode->m_pSibling != NULL)
            printf("The value of its sibling is: %d.\n", pNode->m_pSibling->m_nValue);
        else
            printf("This node does not have a sibling.\n");
        printf("\n");
        pNode = pNode->m_pNext;
    }
}
void Test(char* testName, ComplexListNode* pHead)
{
    if(testName != NULL)
        printf("%s begins:\n", testName);
    printf("The original list is:\n");
    PrintList(pHead);
    ComplexListNode* pClonedHead = Clone(pHead);
    printf("The cloned list is:\n");
    PrintList(pClonedHead);
}
//          -----------------
//         \|/              |
//  1-------2-------3-------4-------5
//  |       |      /|\             /|\
//  --------+--------               |
//          -------------------------
void Test1()
{
    ComplexListNode* pNode1 = CreateNode(1);
    ComplexListNode* pNode2 = CreateNode(2);
    ComplexListNode* pNode3 = CreateNode(3);
    ComplexListNode* pNode4 = CreateNode(4);
    ComplexListNode* pNode5 = CreateNode(5);
    BuildNodes(pNode1, pNode2, pNode3);
    BuildNodes(pNode2, pNode3, pNode5);
    BuildNodes(pNode3, pNode4, NULL);
    BuildNodes(pNode4, pNode5, pNode2);
    Test("Test1", pNode1);
}
// m_pSibling points back to its owner node
//          -----------------
//         \|/              |
//  1-------2-------3-------4-------5
//         |       | /|\           /|\
//         |       | --             |
//         |------------------------|
void Test2()
{
    ComplexListNode* pNode1 = CreateNode(1);
    ComplexListNode* pNode2 = CreateNode(2);
    ComplexListNode* pNode3 = CreateNode(3);
    ComplexListNode* pNode4 = CreateNode(4);
    ComplexListNode* pNode5 = CreateNode(5);
    BuildNodes(pNode1, pNode2, NULL);
    BuildNodes(pNode2, pNode3, pNode5);
    BuildNodes(pNode3, pNode4, pNode3);
    BuildNodes(pNode4, pNode5, pNode2);
    Test("Test2", pNode1);
}
// m_pSibling pointers form a loop
//          -----------------
//         \|/              |
//  1-------2-------3-------4-------5
//          |              /|\
//          |               |
//          |---------------|
void Test3()
{
    ComplexListNode* pNode1 = CreateNode(1);
    ComplexListNode* pNode2 = CreateNode(2);
    ComplexListNode* pNode3 = CreateNode(3);
    ComplexListNode* pNode4 = CreateNode(4);
    ComplexListNode* pNode5 = CreateNode(5);
    BuildNodes(pNode1, pNode2, NULL);
    BuildNodes(pNode2, pNode3, pNode4);
    BuildNodes(pNode3, pNode4, NULL);
    BuildNodes(pNode4, pNode5, pNode2);
    Test("Test3", pNode1);
}
// only one node
void Test4()
{
    ComplexListNode* pNode1 = CreateNode(1);
    BuildNodes(pNode1, NULL, pNode1);
    Test("Test4", pNode1);
}
// empty list
void Test5()
{
    Test("Test5", NULL);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    return 0;
}
```

## 055. MinInStack


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <stack>
#include <cassert>
template <typename T> class StackWithMin
{
public:
    virtual ~StackWithMin(void) {}
    virtual T& top(void) = 0;
    virtual const T& top(void) const = 0;
    virtual void push(const T& value) = 0;
    virtual void pop(void) = 0;
    virtual const T& min(void) const = 0;
    virtual bool empty() const = 0;
    virtual size_t size() const = 0;
};
// ==================== Solution 1 ====================
template <typename T> class StackWithMin_Solution1 : public StackWithMin<T>
{
public:
    StackWithMin_Solution1(void) {}
    virtual ~StackWithMin_Solution1(void) {}
    T& top(void);
    const T& top(void) const;
    void push(const T& value);
    void pop(void);
    const T& min(void) const;
    bool empty() const;
    size_t size() const;
private:
    std::stack<T>   m_data;     // data stack, to store numbers
    std::stack<T>   m_min;      // auxiliary stack, to store minimal numbers
};
template <typename T> void StackWithMin_Solution1<T>::push(const T& value)
{
    m_data.push(value);
    if(m_min.size() == 0 || value < m_min.top())
        m_min.push(value);
    else
        m_min.push(m_min.top());
}
template <typename T> void StackWithMin_Solution1<T>::pop()
{
    assert(m_data.size() > 0 && m_min.size() > 0);
    m_data.pop();
    m_min.pop();
}
template <typename T> const T& StackWithMin_Solution1<T>::min() const
{
    assert(m_data.size() > 0 && m_min.size() > 0);
    return m_min.top();
}
template <typename T> T& StackWithMin_Solution1<T>::top()
{
    return m_data.top();
}
template <typename T> const T& StackWithMin_Solution1<T>::top() const
{
    return m_data.top();
}
template <typename T> bool StackWithMin_Solution1<T>::empty() const
{
    return m_data.empty();
}
template <typename T> size_t StackWithMin_Solution1<T>::size() const
{
    return m_data.size();
}
// ==================== Solution 2 ====================
template <typename T> class StackWithMin_Solution2 : public StackWithMin<T>
{
public:
    StackWithMin_Solution2(void) {}
    virtual ~StackWithMin_Solution2(void) {}
    T& top(void);
    const T& top(void) const;
    void push(const T& value);
    void pop(void);
    const T& min(void) const;
    bool empty() const;
    size_t size() const;
private:
    std::stack<T>   m_data;     // data stack, to store numbers
    T               m_min;      // minimal number
};
template <typename T> void StackWithMin_Solution2<T>::push(const T& value) 
{
    if(m_data.size() == 0) 
    {
        m_data.push(value);
        m_min = value;
    }
    else if(value >= m_min) 
    {
        m_data.push(value);
    }
    else 
    {
        m_data.push(2 * value - m_min);
        m_min = value;
    }
}
template <typename T> void StackWithMin_Solution2<T>::pop() 
{
    assert(m_data.size() > 0);
    if(m_data.top() < m_min)
        m_min = 2 * m_min - m_data.top();
    m_data.pop();
}
template <typename T> const T& StackWithMin_Solution2<T>::min() const 
{
    assert(m_data.size() > 0);
    return m_min;
}
template <typename T> T& StackWithMin_Solution2<T>::top() 
{
    T top = m_data.top();
    if(top < m_min)
        top = m_min;
    return top;
}
template <typename T> const T& StackWithMin_Solution2<T>::top() const
{
    T top = m_data.top();
    if(top < m_min)
        top = m_min;
    return top;
}
template <typename T> bool StackWithMin_Solution2<T>::empty() const
{
    return m_data.empty();
}
template <typename T> size_t StackWithMin_Solution2<T>::size() const
{
    return m_data.size();
}
// ==================== Test Code ====================
void Test(char* testName, const StackWithMin<int>& stack, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(stack.min() == expected)
        printf("Passed.\n");
    else
        printf("Failed.\n");
}
void test_Solution1()
{
    printf("===== Test for Solution1 begins: =====\n");
    
    StackWithMin_Solution1<int> stack;
    stack.push(3);
    Test("Test1", stack, 3);
    stack.push(4);
    Test("Test2", stack, 3);
    stack.push(2);
    Test("Test3", stack, 2);
    stack.push(3);
    Test("Test4", stack, 2);
    stack.pop();
    Test("Test5", stack, 2);
    stack.pop();
    Test("Test6", stack, 3);
    stack.pop();
    Test("Test7", stack, 3);
    stack.push(0);
    Test("Test8", stack, 0);
}
void test_Solution2()
{
    printf("===== Test for Solution2 begins: =====\n");
    
    StackWithMin_Solution2<int> stack;
    stack.push(3);
    Test("Test1", stack, 3);
    stack.push(4);
    Test("Test2", stack, 3);
    stack.push(2);
    Test("Test3", stack, 2);
    stack.push(3);
    Test("Test4", stack, 2);
    stack.pop();
    Test("Test5", stack, 2);
    stack.pop();
    Test("Test6", stack, 3);
    stack.pop();
    Test("Test7", stack, 3);
    stack.push(0);
    Test("Test8", stack, 0);
}
int main(int argc, char* argv[])
{
    test_Solution1();
    test_Solution2();
}
```

## 056. StackPushPopOrder


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <stack>
bool IsPopOrder(const int* pPush, const int* pPop, int nLength)
{
    bool bPossible = false;
    if(pPush != NULL && pPop != NULL && nLength > 0)
    {
        const int* pNextPush = pPush;
        const int* pNextPop = pPop;
        std::stack<int> stackData;
        while(pNextPop - pPop < nLength)
        {
            // Push some numbers when the number to be popped is not 
            // is not on the top of the stack
            while(stackData.empty() || stackData.top() != *pNextPop)
            {
                // Break when all numbers have been pushed
                if(pNextPush - pPush == nLength)
                    break;
                stackData.push(*pNextPush);
                pNextPush ++;
            }
            if(stackData.top() != *pNextPop)
                break;
            stackData.pop();
            pNextPop ++;
        }
        if(stackData.empty() && pNextPop - pPop == nLength)
            bPossible = true;
    }
    return bPossible;
}
// ==================== Test Code ====================
void Test(char* testName, const int* pPush, const int* pPop, int nLength, bool expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(IsPopOrder(pPush, pPop, nLength) == expected)
        printf("Passed.\n");
    else
        printf("failed.\n");
}
void Test1()
{
    const int nLength = 5;
    int push[nLength] = {1, 2, 3, 4, 5};
    int pop[nLength] = {4, 5, 3, 2, 1};
    
    Test("Test1", push, pop, nLength, true);
}
void Test2()
{
    const int nLength = 5;
    int push[nLength] = {1, 2, 3, 4, 5};
    int pop[nLength] = {3, 5, 4, 2, 1};
    
    Test("Test2", push, pop, nLength, true);
}
void Test3()
{
    const int nLength = 5;
    int push[nLength] = {1, 2, 3, 4, 5};
    int pop[nLength] = {4, 3, 5, 1, 2};
    
    Test("Test3", push, pop, nLength, false);
}
void Test4()
{
    const int nLength = 5;
    int push[nLength] = {1, 2, 3, 4, 5};
    int pop[nLength] = {3, 5, 4, 1, 2};
    
    Test("Test4", push, pop, nLength, false);
}
// There is only one element in the push and pop sequences
void Test5()
{
    const int nLength = 1;
    int push[nLength] = {1};
    int pop[nLength] = {2};
    Test("Test5", push, pop, nLength, false);
}
void Test6()
{
    const int nLength = 1;
    int push[nLength] = {1};
    int pop[nLength] = {1};
    Test("Test6", push, pop, nLength, true);
}
void Test7()
{
    Test("Test7", NULL, NULL, 0, false);
}
 
int main(int argc, char* argv[])
{
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

## 057. PrintTreeByLevel


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <queue>
void PrintFromTopToBottom(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return;
    std::queue<BinaryTreeNode *> queueTreeNodes;
    queueTreeNodes.push(pRoot);
    while(queueTreeNodes.size() > 0)
    {
        BinaryTreeNode *pNode = queueTreeNodes.front();
        queueTreeNodes.pop();
        printf("%d ", pNode->m_nValue);
        if(pNode->m_pLeft)
            queueTreeNodes.push(pNode->m_pLeft);
        if(pNode->m_pRight)
            queueTreeNodes.push(pNode->m_pRight);
    }
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot)
{
    if(testName != NULL)
        printf("%s begins: \n", testName);
    PrintTree(pRoot);
    printf("The nodes from top to bottom, from left to right are: \n");
    PrintFromTopToBottom(pRoot);
    printf("\n\n");
}
//            10
//         /      \
//        6        14
//       /\        /\
//      4  8     12  16
void Test1()
{
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode14 = CreateBinaryTreeNode(14);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(12);
    BinaryTreeNode* pNode16 = CreateBinaryTreeNode(16);
    ConnectTreeNodes(pNode10, pNode6, pNode14);
    ConnectTreeNodes(pNode6, pNode4, pNode8);
    ConnectTreeNodes(pNode14, pNode12, pNode16);
    Test("Test1", pNode10);
    DestroyTree(pNode10);
}
//               5
//              /
//             4
//            /
//           3
//          /
//         2
//        /
//       1
void Test2()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode1, NULL);
    Test("Test2", pNode5);
    DestroyTree(pNode5);
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
void Test3()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, NULL, pNode2);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("Test3", pNode1);
    DestroyTree(pNode1);
}
// There is only one node in a tree
void Test4()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    Test("Test4", pNode1);
    DestroyTree(pNode1);
}
// empty tree
void Test5()
{
    Test("Test5", NULL);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
   return 0;
}
```

## 058. PrintTreeALevelInALine


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <queue>
void Print(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return;
    std::queue<BinaryTreeNode*> nodes;
    nodes.push(pRoot);
    int nextLevel = 0;
    int toBePrinted = 1;
    while(!nodes.empty())
    {
        BinaryTreeNode* pNode = nodes.front();
        printf("%d ", pNode->m_nValue);
        if(pNode->m_pLeft != NULL)
        {
            nodes.push(pNode->m_pLeft);
            ++nextLevel;
        }
        if(pNode->m_pRight != NULL)
        {
            nodes.push(pNode->m_pRight);
            ++nextLevel;
        }
        nodes.pop();
        --toBePrinted;
        if(toBePrinted == 0)
        {
            printf("\n");
            toBePrinted = nextLevel;
            nextLevel = 0;
        }
    }
}
// ==================== Test Code ====================
//            8
//        6      10
//       5 7    9  11
void Test1()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    printf("====Test1 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("8 \n");
    printf("6 10 \n");
    printf("5 7 9 11 \n\n");
    printf("Actual Result is: \n");
    Print(pNode8);
    printf("\n");
    DestroyTree(pNode8);
}
//            5
//          4
//        3
//      2
void Test2()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    printf("====Test2 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("5 \n");
    printf("4 \n");
    printf("3 \n");
    printf("2 \n\n");
    printf("Actual Result is: \n");
    Print(pNode5);
    printf("\n");
    DestroyTree(pNode5);
}
//        5
//         4
//          3
//           2
void Test3()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode2);
    printf("====Test3 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("5 \n");
    printf("4 \n");
    printf("3 \n");
    printf("2 \n\n");
    printf("Actual Result is: \n");
    Print(pNode5);
    printf("\n");
    DestroyTree(pNode5);
}
void Test4()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    printf("====Test4 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("5 \n\n");
    printf("Actual Result is: \n");
    Print(pNode5);
    printf("\n");
    DestroyTree(pNode5);
}
void Test5()
{
    printf("====Test5 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("Actual Result is: \n");
    Print(NULL);
    printf("\n");
}
//        100
//        /
//       50   
//         \
//         150
void Test6()
{
    BinaryTreeNode* pNode100 = CreateBinaryTreeNode(100);
    BinaryTreeNode* pNode50 = CreateBinaryTreeNode(50);
    BinaryTreeNode* pNode150 = CreateBinaryTreeNode(150);
    ConnectTreeNodes(pNode100, pNode50, NULL);
    ConnectTreeNodes(pNode50, NULL, pNode150);
    printf("====Test6 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("100 \n");
    printf("50 \n");
    printf("150 \n\n");
    printf("Actual Result is: \n");
    Print(pNode100);
    printf("\n");
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
	return 0;
}
```

## 059. PrintTreeZigzag


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <stack>
void Print(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return;
    std::stack<BinaryTreeNode*> levels[2];
    int current = 0;
    int next = 1;
    levels[current].push(pRoot);
    while(!levels[0].empty() || !levels[1].empty())
    {
        BinaryTreeNode* pNode = levels[current].top();
        levels[current].pop();
        printf("%d ", pNode->m_nValue);
        if(current == 0)
        {
            if(pNode->m_pLeft != NULL)
                levels[next].push(pNode->m_pLeft);
            if(pNode->m_pRight != NULL)
                levels[next].push(pNode->m_pRight);
        }
        else
        {
            if(pNode->m_pRight != NULL)
                levels[next].push(pNode->m_pRight);
            if(pNode->m_pLeft != NULL)
                levels[next].push(pNode->m_pLeft);
        }
        if(levels[current].empty())
        {
            printf("\n");
            current = 1 - current;
            next = 1 - next;
        }
    }
}
//            8
//        6      10
//       5 7    9  11
void Test1()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    printf("====Test1 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("8 \n");
    printf("10 6 \n");
    printf("5 7 9 11 \n\n");
    printf("Actual Result is: \n");
    Print(pNode8);
    printf("\n");
    DestroyTree(pNode8);
}
//            5
//          4
//        3
//      2
void Test2()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    printf("====Test2 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("5 \n");
    printf("4 \n");
    printf("3 \n");
    printf("2 \n\n");
    printf("Actual Result is: \n");
    Print(pNode5);
    printf("\n");
    DestroyTree(pNode5);
}
//        5
//         4
//          3
//           2
void Test3()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode2);
    printf("====Test3 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("5 \n");
    printf("4 \n");
    printf("3 \n");
    printf("2 \n\n");
    printf("Actual Result is: \n");
    Print(pNode5);
    printf("\n");
    DestroyTree(pNode5);
}
void Test4()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    printf("====Test4 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("5 \n\n");
    printf("Actual Result is: \n");
    Print(pNode5);
    printf("\n");
    DestroyTree(pNode5);
}
void Test5()
{
    printf("====Test5 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("Actual Result is: \n");
    Print(NULL);
    printf("\n");
}
//        100
//        /
//       50   
//         \
//         150
void Test6()
{
    BinaryTreeNode* pNode100 = CreateBinaryTreeNode(100);
    BinaryTreeNode* pNode50 = CreateBinaryTreeNode(50);
    BinaryTreeNode* pNode150 = CreateBinaryTreeNode(150);
    ConnectTreeNodes(pNode100, pNode50, NULL);
    ConnectTreeNodes(pNode50, NULL, pNode150);
    printf("====Test6 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("100 \n");
    printf("50 \n");
    printf("150 \n\n");
    printf("Actual Result is: \n");
    Print(pNode100);
    printf("\n");
}
//                8
//        4              12
//     2     6       10      14
//   1  3  5  7     9 11   13  15
void Test7()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(12);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode14 = CreateBinaryTreeNode(14);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    BinaryTreeNode* pNode13 = CreateBinaryTreeNode(13);
    BinaryTreeNode* pNode15 = CreateBinaryTreeNode(15);
    ConnectTreeNodes(pNode8, pNode4, pNode12);
    ConnectTreeNodes(pNode4, pNode2, pNode6);
    ConnectTreeNodes(pNode12, pNode10, pNode14);
    ConnectTreeNodes(pNode2, pNode1, pNode3);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    ConnectTreeNodes(pNode14, pNode13, pNode15);
    printf("====Test7 Begins: ====\n");
    printf("Expected Result is:\n");
    printf("8 \n");
    printf("4 12 \n");
    printf("2 6 10 14 \n");
    printf("15 13 11 9 7 5 3 1 \n\n");
    printf("Actual Result is: \n");
    Print(pNode8);
    printf("\n");
    DestroyTree(pNode8);
}
int main(int argc, char* argv[])
{
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

## 060. PathInTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <vector>
void FindPath(BinaryTreeNode* pRoot, int expectedSum, std::vector<int>& path, int currentSum);
void FindPath(BinaryTreeNode* pRoot, int expectedSum)
{
    if(pRoot == NULL)
        return;
    std::vector<int> path;
    int currentSum = 0;
    FindPath(pRoot, expectedSum, path, currentSum);
}
void FindPath
(
    BinaryTreeNode*   pRoot,        
    int               expectedSum,  
    std::vector<int>& path,         
    int               currentSum
)
{
    currentSum += pRoot->m_nValue;
    path.push_back(pRoot->m_nValue);
    // Print the path is the current node is a leaf
    // and the sum of all nodes value is same as expectedSum 
    bool isLeaf = pRoot->m_pLeft == NULL && pRoot->m_pRight == NULL;
    if(currentSum == expectedSum && isLeaf)
    {
        printf("A path is found: ");
        std::vector<int>::iterator iter = path.begin();
        for(; iter != path.end(); ++ iter)
            printf("%d\t", *iter);
        
        printf("\n");
    }
    // If it is not a leaf, continue visition its children
    if(pRoot->m_pLeft != NULL)
        FindPath(pRoot->m_pLeft, expectedSum, path, currentSum);
    if(pRoot->m_pRight != NULL)
        FindPath(pRoot->m_pRight, expectedSum, path, currentSum);
    // Before returning back to its parent, remove it from path
    path.pop_back();
} 
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot, int expectedSum)
{
    if(testName != NULL)
        printf("%s begins:\n", testName);
    FindPath(pRoot, expectedSum);
    printf("\n");
}
//            10
//         /      \
//        5        12
//       /\        
//      4  7     
// Two paths with sum 22
void Test1()
{
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(12);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNode10, pNode5, pNode12);
    ConnectTreeNodes(pNode5, pNode4, pNode7);
    printf("Two paths should be found in Test1.\n");
    Test("Test1", pNode10, 22);
    DestroyTree(pNode10);
}
//            10
//         /      \
//        5        12
//       /\        
//      4  7     
// No path with sum 15
void Test2()
{
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(12);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNode10, pNode5, pNode12);
    ConnectTreeNodes(pNode5, pNode4, pNode7);
    printf("No paths should be found in Test2.\n");
    Test("Test2", pNode10, 15);
    DestroyTree(pNode10);
}
//               5
//              /
//             4
//            /
//           3
//          /
//         2
//        /
//       1
// A path with sum 15
void Test3()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode1, NULL);
    printf("One path should be found in Test3.\n");
    Test("Test3", pNode5, 15);
    DestroyTree(pNode5);
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
// No path with sum 16
void Test4()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, NULL, pNode2);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    printf("No paths should be found in Test4.\n");
    Test("Test4", pNode1, 16);
    DestroyTree(pNode1);
}
// Only one node in a tree 
void Test5()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    printf("One path should be found in Test5.\n");
    Test("Test5", pNode1, 1);
    DestroyTree(pNode1);
}
// empty tree
void Test6()
{
    printf("No paths should be found in Test6.\n");
    Test("Test6", NULL, 0);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    return 0;
}
```

## 061. ConstructBinaryTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <exception>
BinaryTreeNode* ConstructCore(int* startPreorder, int* endPreorder, int* startInorder, int* endInorder);
BinaryTreeNode* Construct(int* preorder, int* inorder, int length)
{
    if(preorder == NULL || inorder == NULL || length <= 0)
        return NULL;
    return ConstructCore(preorder, preorder + length - 1,
        inorder, inorder + length - 1);
}
BinaryTreeNode* ConstructCore
(
    int* startPreorder, int* endPreorder, 
    int* startInorder, int* endInorder
)
{
    // The first number in the pre-order traversal sequence is the root value
    int rootValue = startPreorder[0];
    BinaryTreeNode* root = new BinaryTreeNode();
    root->m_nValue = rootValue;
    root->m_pLeft = root->m_pRight = NULL;
    if(startPreorder == endPreorder)
    {
        if(startInorder == endInorder && *startPreorder == *startInorder)
            return root;
        else
            throw std::exception("Invalid input.");
    }
    // Get the root value in the in-order traversal sequence
    int* rootInorder = startInorder;
    while(rootInorder <= endInorder && *rootInorder != rootValue)
        ++ rootInorder;
    if(rootInorder == endInorder && *rootInorder != rootValue)
        throw std::exception("Invalid input.");
    int leftLength = rootInorder - startInorder;
    int* leftPreorderEnd = startPreorder + leftLength;
    if(leftLength > 0)
    {
        // Build left subtree
        root->m_pLeft = ConstructCore(startPreorder + 1, leftPreorderEnd, 
            startInorder, rootInorder - 1);
    }
    if(leftLength < endPreorder - startPreorder)
    {
        // Build rigth subtree
        root->m_pRight = ConstructCore(leftPreorderEnd + 1, endPreorder,
            rootInorder + 1, endInorder);
    }
    return root;
}
// ==================== Test Code ====================
void Test(char* testName, int* preorder, int* inorder, int length)
{
    if(testName != NULL)
        printf("%s begins:\n", testName);
    printf("The preorder sequence is: ");
    for(int i = 0; i < length; ++ i)
        printf("%d ", preorder[i]);
    printf("\n");
    printf("The inorder sequence is: ");
    for(int i = 0; i < length; ++ i)
        printf("%d ", inorder[i]);
    printf("\n");
    try
    {
        BinaryTreeNode* root = Construct(preorder, inorder, length);
        PrintTree(root);
        DestroyTree(root);
    }
    catch(std::exception& exception)
    {
        printf("Invalid Input.\n");
    }
}
//              1
//           /     \
//          2       3  
//         /       / \
//        4       5   6
//         \         /
//          7       8
void Test1()
{
    const int length = 8;
    int preorder[length] = {1, 2, 4, 7, 3, 5, 6, 8};
    int inorder[length] = {4, 7, 2, 1, 5, 3, 8, 6};
    Test("Test1", preorder, inorder, length);
}
//            1
//           / 
//          2   
//         / 
//        3 
//       /
//      4
//     /
//    5
void Test2()
{
    const int length = 5;
    int preorder[length] = {1, 2, 3, 4, 5};
    int inorder[length] = {5, 4, 3, 2, 1};
    Test("Test2", preorder, inorder, length);
}
//            1
//             \ 
//              2   
//               \ 
//                3 
//                 \
//                  4
//                   \
//                    5
void Test3()
{
    const int length = 5;
    int preorder[length] = {1, 2, 3, 4, 5};
    int inorder[length] = {1, 2, 3, 4, 5};
    Test("Test3", preorder, inorder, length);
}
// Only one node
void Test4()
{
    const int length = 1;
    int preorder[length] = {1};
    int inorder[length] = {1};
    Test("Test4", preorder, inorder, length);
}
//              1
//           /     \
//          2       3  
//         / \     / \
//        4   5   6   7
void Test5()
{
    const int length = 7;
    int preorder[length] = {1, 2, 4, 5, 3, 6, 7};
    int inorder[length] = {4, 2, 5, 1, 6, 3, 7};
    Test("Test5", preorder, inorder, length);
}
// empty tree
void Test6()
{
    Test("Test6", NULL, NULL, 0);
}
// ������������в�ƥ��
void Test7()
{
    const int length = 7;
    int preorder[length] = {1, 2, 4, 5, 3, 6, 7};
    int inorder[length] = {4, 2, 8, 1, 6, 3, 7};
    Test("Test7: for unmatched input", preorder, inorder, length);
}
int main(int argc, char* argv[])
{
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

## 062. SerializeBinaryTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
#include <iostream>
#include <fstream>
using namespace std;
void Serialize(BinaryTreeNode* pRoot, ostream& stream)
{
    if(pRoot == NULL)
    {
        stream << "$,";
        return;
    }
    
    stream << pRoot->m_nValue << ',';
    Serialize(pRoot->m_pLeft, stream);
    Serialize(pRoot->m_pRight, stream);
}
bool ReadStream(istream& stream, int* number)
{
    if(stream.eof())
        return false;
    
    char buffer[32];
    buffer[0] = '\0';
    
    char ch;
    stream >> ch;
    int i = 0;
    while(!stream.eof() && ch != ',')
    {
        buffer[i++] = ch;
        stream >> ch;
    }
    
    bool isNumeric = false;
    if(i > 0 && buffer[0] != '$')
    {
        *number = atoi(buffer);
        isNumeric = true;
    }
        
    return isNumeric;
}
void Deserialize(BinaryTreeNode** pRoot, istream& stream)
{
    int number;
    if(ReadStream(stream, &number))
    {
        *pRoot = new BinaryTreeNode();
        (*pRoot)->m_nValue = number;
        (*pRoot)->m_pLeft = NULL;
        (*pRoot)->m_pRight = NULL;
        Deserialize(&((*pRoot)->m_pLeft), stream);
        Deserialize(&((*pRoot)->m_pRight), stream);
    }
}
// ==================== Test Code ====================
bool isSameTree(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2)
{
    if(pRoot1 == NULL && pRoot2 == NULL)
        return true;
    
    if(pRoot1 == NULL || pRoot2 == NULL)
        return false;
    
    if(pRoot1->m_nValue != pRoot2->m_nValue)
        return false;
    
    return isSameTree(pRoot1->m_pLeft, pRoot2->m_pLeft) &&
        isSameTree(pRoot1->m_pRight, pRoot2->m_pRight);
}
void Test(char* testName, BinaryTreeNode* pRoot)
{
    if(testName != NULL)
        printf("%s begins: \n", testName);
    
    PrintTree(pRoot);
    
    char* fileName = "test.txt";
    ofstream fileOut;
    fileOut.open(fileName);
    
    Serialize(pRoot, fileOut);
    fileOut.close();
    
    // print the serialized file
    ifstream fileIn1;
    char ch;
    fileIn1.open(fileName);
    while(!fileIn1.eof())
    {
        fileIn1 >> ch;
        cout << ch;       
    }
    fileIn1.close();
    cout << endl;
    
    ifstream fileIn2;
    fileIn2.open(fileName);
    BinaryTreeNode* pNewRoot = NULL;
    Deserialize(&pNewRoot, fileIn2);
    fileIn2.close();
    
    PrintTree(pNewRoot);
    
    if(isSameTree(pRoot, pNewRoot))
        printf("The deserialized tree is same as the oritinal tree.\n\n");
    else
        printf("The deserialized tree is NOT same as the oritinal tree.\n\n");
    
    DestroyTree(pNewRoot);
}
//            8
//        6      10
//       5 7    9  11
void Test1()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    Test("Test1", pNode8);
    DestroyTree(pNode8);
}
//            5
//          4
//        3
//      2
void Test2()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    Test("Test2", pNode5);
    DestroyTree(pNode5);
}
//        5
//         4
//          3
//           2
void Test3()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    ConnectTreeNodes(pNode5, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode2);
    Test("Test3", pNode5);
    DestroyTree(pNode5);
}
void Test4()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    Test("Test4", pNode5);
    DestroyTree(pNode5);
}
void Test5()
{
    Test("Test5", NULL);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    
    return 0;
}
```

## 063. SequenceOfBST


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
bool VerifySquenceOfBST(int sequence[], int length)
{
    if(sequence == NULL || length <= 0)
        return false;
    int root = sequence[length - 1];
    // nodes in left sub-tree are less than root node
    int i = 0;
    for(; i < length - 1; ++ i)
    {
        if(sequence[i] > root)
            break;
    }
    // nodes in right sub-tree are greater than root node
    int j = i;
    for(; j < length - 1; ++ j)
    {
        if(sequence[j] < root)
            return false;
    }
    // Is left sub-tree a binary search tree?
    bool left = true;
    if(i > 0)
        left = VerifySquenceOfBST(sequence, i);
    // Is right sub-tree a binary search tree? 
    bool right = true;
    if(i < length - 1)
        right = VerifySquenceOfBST(sequence + i, length - i - 1);
    return (left && right);
}
// ==================== Test Code ====================
void Test(char* testName, int sequence[], int length, bool expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(VerifySquenceOfBST(sequence, length) == expected)
        printf("passed.\n");
    else
        printf("failed.\n");
}
//            10
//         /      \
//        6        14
//       /\        /\
//      4  8     12  16
void Test1()
{
    int data[] = {4, 8, 6, 12, 16, 14, 10};
    Test("Test1", data, sizeof(data)/sizeof(int), true);
}
//           5
//          / \
//         4   7
//            /
//           6
void Test2()
{
    int data[] = {4, 6, 7, 5};
    Test("Test2", data, sizeof(data)/sizeof(int), true);
}
//               5
//              /
//             4
//            /
//           3
//          /
//         2
//        /
//       1
void Test3()
{
    int data[] = {1, 2, 3, 4, 5};
    Test("Test3", data, sizeof(data)/sizeof(int), true);
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
void Test4()
{
    int data[] = {5, 4, 3, 2, 1};
    Test("Test4", data, sizeof(data)/sizeof(int), true);
}
// Only one node
void Test5()
{
    int data[] = {5};
    Test("Test5", data, sizeof(data)/sizeof(int), true);
}
void Test6()
{
    int data[] = {7, 4, 6, 5};
    Test("Test6", data, sizeof(data)/sizeof(int), false);
}
void Test7()
{
    int data[] = {4, 6, 12, 8, 16, 14, 10};
    Test("Test7", data, sizeof(data)/sizeof(int), false);
}
// empty
void Test8()
{
    Test("Test8", NULL, 0, false);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
    return 0;
}
```

## 064. ConvertBinarySearchTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
// ==================== Solution 1 ====================
void ConvertNode(BinaryTreeNode* pNode, BinaryTreeNode** pLastNodeInList);
BinaryTreeNode* Convert_Solution1(BinaryTreeNode* pRootOfTree)
{
    BinaryTreeNode *pLastNodeInList = NULL;
    ConvertNode(pRootOfTree, &pLastNodeInList);
    // pLastNodeInList points to the last node in the doule-linked list,
    // but we are going to return the head node.
    BinaryTreeNode *pHeadOfList = pLastNodeInList;
    while(pHeadOfList != NULL && pHeadOfList->m_pLeft != NULL)
        pHeadOfList = pHeadOfList->m_pLeft;
    return pHeadOfList;
}
void ConvertNode(BinaryTreeNode* pNode, BinaryTreeNode** pLastNodeInList)
{
    if(pNode == NULL)
        return;
    BinaryTreeNode *pCurrent = pNode;
    if (pCurrent->m_pLeft != NULL)
        ConvertNode(pCurrent->m_pLeft, pLastNodeInList);
    pCurrent->m_pLeft = *pLastNodeInList; 
    if(*pLastNodeInList != NULL)
        (*pLastNodeInList)->m_pRight = pCurrent;
    *pLastNodeInList = pCurrent;
    if (pCurrent->m_pRight != NULL)
        ConvertNode(pCurrent->m_pRight, pLastNodeInList);
}
// ==================== Solution 2 ====================
BinaryTreeNode* Convert_Solution2(BinaryTreeNode* pRoot)
{
    BinaryTreeNode* pNode = pRoot;
    BinaryTreeNode* pHead = NULL;
    BinaryTreeNode* pParent = NULL;
    while(pNode != NULL)
    {
        while(pNode->m_pLeft != NULL)
        {
            // right rotation
            BinaryTreeNode* pLeft = pNode->m_pLeft;
            pNode->m_pLeft = pLeft->m_pRight;
            pLeft->m_pRight = pNode;
            pNode = pLeft;
            if(pParent != NULL)
                pParent->m_pRight = pNode;
        }
        if(pHead == NULL)
            pHead = pNode;
        pParent = pNode;
        pNode = pNode->m_pRight;
    }
    // build double-linked list
    BinaryTreeNode* pNode1 = pHead;
    if(pNode1 != NULL)
    {
        BinaryTreeNode* pNode2 = pNode1->m_pRight;
        while(pNode2 != NULL)
        {
            pNode2->m_pLeft = pNode1;
            pNode1 = pNode2;
            pNode2 = pNode2->m_pRight;
        }
    }
    return pHead;
}
// ==================== Test code ====================
void Clone(BinaryTreeNode* pRoot, BinaryTreeNode** pCloned)
{
    if(pRoot == NULL)
        return;
    
    *pCloned = CreateBinaryTreeNode(pRoot->m_nValue);
    
    BinaryTreeNode* pLeft = NULL;
    BinaryTreeNode* pRight = NULL;
    
    Clone(pRoot->m_pLeft, &pLeft);
    Clone(pRoot->m_pRight, &pRight);
    
    (*pCloned)->m_pLeft = pLeft;
    (*pCloned)->m_pRight = pRight;
}
void PrintDoubleLinkedList(BinaryTreeNode* pHeadOfList)
{
    BinaryTreeNode* pNode = pHeadOfList;
    printf("The nodes from left to right are:\n");
    while(pNode != NULL)
    {
        printf("%d\t", pNode->m_nValue);
        if(pNode->m_pRight == NULL)
            break;
        pNode = pNode->m_pRight;
    }
    printf("\nThe nodes from right to left are:\n");
    while(pNode != NULL)
    {
        printf("%d\t", pNode->m_nValue);
        if(pNode->m_pLeft == NULL)
            break;
        pNode = pNode->m_pLeft;
    }
    printf("\n");
}
void DestroyList(BinaryTreeNode* pHeadOfList)
{
    BinaryTreeNode* pNode = pHeadOfList;
    while(pNode != NULL)
    {
        BinaryTreeNode* pNext = pNode->m_pRight;
        delete pNode;
        pNode = pNext;
    }
}
void Test(char* testName, BinaryTreeNode* pRootOfTree)
{
    if(testName != NULL)
        printf("========%s begins: ========\n", testName);
        
    BinaryTreeNode* pNewRoot = NULL;
    Clone(pRootOfTree, &pNewRoot);
    printf("======== Solution 1 ========\n");
    PrintTree(pRootOfTree);
    BinaryTreeNode* pHeadOfList1 = Convert_Solution1(pRootOfTree);
    PrintDoubleLinkedList(pHeadOfList1);
    
    printf("======== Solution 2 ========\n");
    PrintTree(pNewRoot);
    BinaryTreeNode* pHeadOfList2 = Convert_Solution2(pNewRoot);
    PrintDoubleLinkedList(pHeadOfList2);
    
    DestroyList(pHeadOfList2);
    
    printf("\n\n");
}
//            10
//         /      \
//        6        14
//       /\        /\
//      4  8     12  16
void Test1()
{
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode14 = CreateBinaryTreeNode(14);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode12 = CreateBinaryTreeNode(12);
    BinaryTreeNode* pNode16 = CreateBinaryTreeNode(16);
    ConnectTreeNodes(pNode10, pNode6, pNode14);
    ConnectTreeNodes(pNode6, pNode4, pNode8);
    ConnectTreeNodes(pNode14, pNode12, pNode16);
    Test("Test1", pNode10);
    DestroyList(pNode4);
}
//               5
//              /
//             4
//            /
//           3
//          /
//         2
//        /
//       1
void Test2()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode1, NULL);
    Test("Test2", pNode5);
    DestroyList(pNode1);
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
void Test3()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, NULL, pNode2);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("Test3", pNode1);
    DestroyList(pNode1);
}
// One node in a tree
void Test4()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    Test("Test4", pNode1);
    DestroyList(pNode1);
}
// empty
void Test5()
{
    Test("Test5", NULL);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    return 0;
}
```

## 065. StringPermutation


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string.h>
void PermutationCore(char* pStr, char* pBegin);
void Permutation(char* pStr)
{
    if(pStr == NULL)
        return;
    PermutationCore(pStr, pStr);
}
void PermutationCore(char* pStr, char* pBegin)
{
    char *pCh = NULL;
    char temp;
    
    if(*pBegin == '\0')
    {
        printf("%s\n", pStr);
    }
    else
    {
        for(pCh = pBegin; *pCh != '\0'; ++ pCh)
        {
            temp = *pCh;
            *pCh = *pBegin;
            *pBegin = temp;
            PermutationCore(pStr, pBegin + 1);
            temp = *pCh;
            *pCh = *pBegin;
            *pBegin = temp;
        }
    }
}
// ==================== Test Code ====================
void Test(char* pStr)
{
    if(pStr == NULL)
        printf("Test for NULL begins:\n");
    else
        printf("Test for %s begins:\n", pStr);
    Permutation(pStr);
    printf("\n");
}
int main(int argc, char* argv[])
{
    char string1[64];
    char string2[64];
    char string3[64];
    char string4[64];
    Test(NULL);
    string1[0] = '\0';
    Test(string1);
    sprintf(string2, "%s", "a");
    Test(string2);
    sprintf(string3, "%s", "ab");
    Test(string3);
    sprintf(string4, "%s", "abc");
    Test(string4);
    return 0;
}
```

## 066. EightQueens


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
void Permutation(int columnIndex[], int length, int index, int* count);
int Check(int ColumnIndex[], int length);
int EightQueen()
{
    int columnIndex[8] = {0, 1, 2, 3, 4, 5, 6, 7};
    
    int count = 0;
    Permutation(columnIndex, 8, 0, &count);
    
    return count;
}
void Permutation(int columnIndex[], int length, int index, int* count)
{
    int i, temp;
    
    if(index == length)
    {
        if(Check(columnIndex, length) != 0)
        {
            (*count)++;
        }
    }
    else
    {
        for(i = index; i < length; ++ i)
        {
            temp = columnIndex[i];
            columnIndex[i] = columnIndex[index];
            columnIndex[index] = temp;
            Permutation(columnIndex, length, index + 1, count);
            temp = columnIndex[index];
            columnIndex[index] = columnIndex[i];
            columnIndex[i] = temp;
        }
    }
}
/* If there are two queens on the same diagonal, it returns 0,
   otherwise it returns 1. */
int Check(int columnIndex[], int length)
{
    int i, j;
    
    for(i = 0; i < length; ++ i)
    {
        for(j = i + 1; j < length; ++ j)
        {
            if((i - j == columnIndex[i] - columnIndex[j])
                || (j - i == columnIndex[i] - columnIndex[j]))
            return 0;
        }
    }
    return 1;
}
int main(int argc, char* argv[])
{
    int count = EightQueen();
    printf("The count of ways to place eight queens: %d.\n", count);
}
```

## 067. ArrayPermutation


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.*;
public class ArrayPermutation {
    static public void permute(ArrayList<int[]> arrays){
        Stack<Integer> permutation = new Stack<Integer>();
        permuteCore(arrays, permutation);
    }
    
    static private void permuteCore(ArrayList<int[]> arrays, Stack<Integer> permutation){
        if(permutation.size() == arrays.size()){
            System.out.println(permutation);
            return;
        }
        
        int[] array = arrays.get(permutation.size());
        for(int i = 0; i < array.length; ++i){
            permutation.push(array[i]);
            permuteCore(arrays, permutation);
            permutation.pop();
        }
    }
    
    //================= Test Code =================
    static private void test(String testName, ArrayList<int[]> arrays){
        System.out.println(testName + " begins: ");
        
        permute(arrays);
    }
    
    static private void test1(){
        ArrayList<int[]> arrays = new ArrayList<int[]>();
        
        int[] array1 = {1, 2};
        int[] array2 = {3, 4};
        int[] array3 = {5, 6};
        
        arrays.add(array1);
        arrays.add(array2);
        arrays.add(array3);
        
        test("Test1", arrays);
    }
    
    static private void test2(){
        ArrayList<int[]> arrays = new ArrayList<int[]>();
        
        int[] array1 = {1, 2};
        int[] array2 = {3, 4, 5};
        int[] array3 = {6};
        
        arrays.add(array1);
        arrays.add(array2);
        arrays.add(array3);
        
        test("Test2", arrays);
    }
    
    static private void test3(){
        ArrayList<int[]> arrays = new ArrayList<int[]>();
        
        int[] array1 = {1, 2, 3};
        int[] array2 = {4, 5, 6};
        int[] array3 = {7, 8};
        
        arrays.add(array1);
        arrays.add(array2);
        arrays.add(array3);
        
        test("Test3", arrays);
    }
    
    static private void test4(){
        ArrayList<int[]> arrays = new ArrayList<int[]>();
        
        int[] array1 = {1, 2, 3, 4, 5};
        
        arrays.add(array1);
        
        test("Test4", arrays);
    }
    
    static private void test5(){
        ArrayList<int[]> arrays = new ArrayList<int[]>();
        
        int[] array1 = {6};
        int[] array2 = {7};
        int[] array3 = {8};
        int[] array4 = {9};
        
        arrays.add(array1);
        arrays.add(array2);
        arrays.add(array3);
        arrays.add(array4);
        
        test("Test4", arrays);
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

## 068. StringCombination


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.BitSet;
import java.util.Stack;
public class StringCombination {
    // ==================== solution 1 ====================
    public static void combination_solution1(String str) {
        Stack<Character> result = new Stack<Character>();
        for(int i = 1; i <= str.length(); ++ i) {
            combination(str, 0, i, result);
        }
    }
    private static void combination(String str, int index, int number, Stack<Character> result) {
        if(number == 0) {
            System.out.println(result);
            return;
        }
        
        if(index == str.length())
            return;
     
        // select the character str[index]
        result.push(str.charAt(index));
        combination(str, index + 1, number - 1, result);
        result.pop();
        // ignore the character str[index]
        combination(str, index + 1, number, result);
    }
    
    // ==================== solution 2 ====================
    public static void combination_solution2(String str) {
        BitSet bits = new BitSet(str.length());
        while(increment(bits, str.length()))
            print(str, bits);
    }
    
    private static boolean increment(BitSet bits, int length) {
        int index = length - 1;
        
        while(index >= 0 && bits.get(index)) {
            bits.clear(index);
            --index;
        }
        
        if(index < 0)
            return false;
        
        bits.set(index);
        return true;
    }
    
    private static void print(String str, BitSet bits) {
        for(int i = 0; i < str.length(); ++i) {
            if(bits.get(i))
                System.out.print(str.charAt(i));
        }
        
        System.out.println();
    }
    
    // ==================== test code ====================
    private static void test(String str) {
        System.out.println("Test Begins for " + str);
        combination_solution1(str);
        combination_solution2(str);
    }
    public static void main(String args[]) {
        test("");
        test("a");
        test("ab");
        test("abc");
        test("abcd");
    }
}
```

## 069. MedianStream


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <algorithm>
#include <vector>
#include <functional>
using namespace std;
template<typename T> class DynamicArray
{
public:
    void Insert(T num)
    {
        if(((minHeap.size() + maxHeap.size()) & 1) == 0)
        {
            if(maxHeap.size() > 0 && num < maxHeap[0])
            {
                maxHeap.push_back(num);
                push_heap(maxHeap.begin(), maxHeap.end(), less<T>());
                num = maxHeap[0];
                pop_heap(maxHeap.begin(), maxHeap.end(), less<T>());
                maxHeap.pop_back();
            }
            minHeap.push_back(num);
            push_heap(minHeap.begin(), minHeap.end(), greater<T>());
        }
        else
        {
            if(minHeap.size() > 0 && minHeap[0] < num)
            {
                minHeap.push_back(num);
                push_heap(minHeap.begin(), minHeap.end(), greater<T>());
                num = minHeap[0];
                pop_heap(minHeap.begin(), minHeap.end(), greater<T>());
                minHeap.pop_back();
            }
            maxHeap.push_back(num);
            push_heap(maxHeap.begin(), maxHeap.end(), less<T>());
        }
    }
    int GetMedian()
    { 
        int size = minHeap.size() + maxHeap.size();
        if(size == 0)
            throw exception("No numbers are available");
        T median = 0;
        if(size & 1 == 1)
            median = minHeap[0];
        else
            median = (minHeap[0] + maxHeap[0]) / 2;
        return median;
    }
private:
    vector<T> minHeap;
    vector<T> maxHeap;
};
// ==================== Test Code ====================
void Test(char* testName, DynamicArray<double>& numbers, double expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(abs(numbers.GetMedian() - expected) < 0.0000001)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
int main(int argc, char* argv[])
{
    DynamicArray<double> numbers;
    printf("Test1 begins: ");
    try
    {
        numbers.GetMedian();
        printf("FAILED.\n");
    }
    catch(exception e)
    {
        printf("Passed.\n");
    }
    numbers.Insert(5);
    Test("Test2", numbers, 5);
    numbers.Insert(2);
    Test("Test3", numbers, 3.5);
    numbers.Insert(3);
    Test("Test4", numbers, 3);
    numbers.Insert(4);
    Test("Test6", numbers, 3.5);
    numbers.Insert(1);
    Test("Test5", numbers, 3);
    numbers.Insert(6);
    Test("Test7", numbers, 3.5);
    numbers.Insert(7);
    Test("Test8", numbers, 4);
    numbers.Insert(0);
    Test("Test9", numbers, 3.5);
    numbers.Insert(8);
    Test("Test10", numbers, 4);
	return 0;
}
```

## 070. KLeastNumbers


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.Arrays;
import java.util.Iterator;
import java.util.PriorityQueue;
import java.util.Random;
import java.util.Comparator;
public class KLeastNumbers {
    
    //================= Solution 1 =================
    public class ReversedComparator implements Comparator<Integer> {
        public int compare(Integer int1, Integer int2){
            int num1 = int1.intValue();
            int num2 = int2.intValue();
            
            if(num1 < num2)
                return 1;
            else if (num1 == num2)
                return 0;
            else
                return -1;
        }
    }
    public static void getLeastNumbers_1(int[] input, int[] output) {
        KLeastNumbers leastNumbers = new KLeastNumbers();
        ReversedComparator comparator = leastNumbers.new ReversedComparator();
        PriorityQueue<Integer> maxQueue = null;
        maxQueue = new PriorityQueue<Integer>(1, comparator);
        
        getLeastNumbers(input, maxQueue, output.length);
        
        Iterator<Integer> iter = maxQueue.iterator();
        for(int i = 0; i < output.length; ++i) {
            output[i] = iter.next();
        }
    }
    
    private static void getLeastNumbers(int[] input, PriorityQueue<Integer> output, int k) {
        output.clear();
        
        for(int i = 0; i < input.length; ++i) {
            if(output.size() < k) 
                output.add(new Integer(input[i]));
            else {
                Integer max = output.peek();
                Integer number = new Integer(input[i]);
                if(output.comparator().compare(number, max) > 0) {
                    output.poll();
                    output.add(number);
                }
            }
        }
    }
    //================= Solution 2 =================
    public static void getLeastNumbers_2(int[] input, int[] output) {
        int start = 0;
        int end = input.length - 1;
        int k = output.length;
        int index = partition(input, start, end);
        while(index != k - 1) {
            if(index > k - 1) {
                end = index - 1;
                index = partition(input, start, end);
            }
            else {
                start = index + 1;
                index = partition(input, start, end);
            }
        }
        
        for(int i = 0; i < k; ++i)
            output[i] = input[i];
    }
    
    private static int partition(int[] numbers, int start, int end) {
        int index = randomInRange(start, end);
        swap(numbers, index, end);
        
        int small = start - 1;
        for(index = start; index < end; ++index) {
            if(numbers[index] < numbers[end]){
                ++small;
                if(small != index)
                    swap(numbers, index, small);
            }
        }
        
        ++small;
        swap(numbers, small, end);
        
        return small;
    }
    
    private static int randomInRange(int start, int end){
        Random random = new Random();
        return random.nextInt(end - start + 1) + start;
    }
    
    private static void swap(int[] numbers, int index1, int index2){
        int temp = numbers[index1];
        numbers[index1] = numbers[index2];
        numbers[index2] = temp;
    }
    
    //================= Test Code =================
    
    private static boolean equals(int[] array1, int[] array2) {
        if(array1.length != array2.length)
            return false;
        
        Arrays.sort(array1);
        Arrays.sort(array2);
        
        int i = 0;
        for(; i < array1.length; ++i) {
            if(array1[i] != array2[i])
                break;
        }
        
        return (i == array1.length);
    }
    
    private static void test(String testName, int[] numbers, int[] expected) {
        System.out.print(testName + " begins: ");
        
        int[] output1 = new int[expected.length];
        getLeastNumbers_1(numbers, output1);
        if(equals(output1, expected))
            System.out.print("Solution1 Passed; ");
        else
            System.out.print("Solution1 FAILED; ");
        
        int[] output2 = new int[expected.length];
        getLeastNumbers_2(numbers, output2);
        if(equals(output2, expected))
            System.out.print("Solution2 Passed.\n");
        else
            System.out.print("Solution2 FAILED.\n");
    }
    
    private static void test1() {
        int[] numbers = {4, 5, 1, 6, 2, 7, 3, 8};
        int[] expected = {1, 2, 3, 4};
        test("Test1", numbers, expected);
    }
    
    private static void test2() {
        int[] numbers = {4, 5, 1, 6, 2, 7, 3, 8};
        int[] expected = {1, 2, 3, 4, 5, 6, 7, 8};
        test("Test2", numbers, expected);
    }
    
    private static void test3() {
        int[] numbers = {4, 5, 1, 6, 2, 7, 3, 8};
        int[] expected = {1};
        test("Test3", numbers, expected);
    }
    
    // duplicated numbers in the array
    private static void test4() {
        int[] numbers = {4, 5, 1, 6, 2, 7, 2, 8};
        int[] expected = {1, 2};
        test("Test4", numbers, expected);
    }
    
    public static void main(String[] argv) {
        test1();
        test2();
        test3();
        test4();
    }
}
```

## 071. ArrayIntersection


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <vector>
#include <algorithm>
using namespace std;
// ==================== Solution 1 ====================
void GetIntersection_solution1(const vector<int>& array1, 
                     const vector<int>& array2, 
                     vector<int>& intersection)
{
    vector<int>::const_iterator iter1 = array1.begin();
    vector<int>::const_iterator iter2 = array2.begin();
    intersection.clear();
    while(iter1 != array1.end() && iter2 != array2.end())
    {
        if(*iter1 == *iter2)
        {
            intersection.push_back(*iter1);
            ++ iter1;
            ++ iter2;
        }
        else if(*iter1 < *iter2)
            ++ iter1;
        else
            ++ iter2;
    }
}
// ==================== Solution 2 ====================
/* === Supposing array1 is much longer than array2 === */
void GetIntersection_solution2(const vector<int>& array1, 
                     const vector<int>& array2, 
                     vector<int>& intersection)
{
    intersection.clear();
    
    vector<int>::const_iterator iter1 = array1.begin();
    while(iter1 != array1.end())
    {
        if(binary_search(array2.begin(), array2.end(), *iter1))
            intersection.push_back(*iter1);
    }
}
// ==================== Test Code ====================
bool CheckIdenticalArray(const vector<int>& array1, 
          const vector<int>& array2)
{
    vector<int>::const_iterator iter1 = array1.begin();
    vector<int>::const_iterator iter2 = array2.begin();
    bool identical = true;
    while(iter1 != array1.end() && iter2 != array2.end())
    {
        if(*iter1 != *iter2)
        {
            identical = false;
            break;
        }
        ++ iter1;
        ++ iter2;
    }
    if(iter1 != array1.end() || iter2 != array2.end())
        identical = false;
    return identical;
}    
void Test(char* testName, 
          const vector<int>& array1, 
          const vector<int>& array2, 
          vector<int>& expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    vector<int> result1;
    GetIntersection_solution1(array1, array2, result1);
    if(CheckIdenticalArray(result1, expected))
        printf("solution1 passed; ");
    else
        printf("solution1 FAILED; ");
    vector<int> result2;
    GetIntersection_solution1(array1, array2, result2);
    if(CheckIdenticalArray(result2, expected))
        printf("solution2 passed.\n");
    else
        printf("solution2 FAILED.\n");
}
void Test1()
{
    int numbers1[] = {1, 4, 7, 10, 13, 16};
    vector<int> array1(numbers1, numbers1 + sizeof(numbers1) / sizeof(int));
    int numbers2[] = {1, 3, 5, 7, 9, 11};
    vector<int> array2(numbers2, numbers2 + sizeof(numbers2) / sizeof(int));
    int exp[] = {1, 7};
    vector<int> expected(exp, exp + sizeof(exp) / sizeof(int));
    Test("Test1", array1, array2, expected);
}
void Test2()
{
    int numbers1[] = {4, 7, 10, 13};
    vector<int> array1(numbers1, numbers1 + sizeof(numbers1) / sizeof(int));
    int numbers2[] = {1, 3, 5, 7, 9, 11, 13};
    vector<int> array2(numbers2, numbers2 + sizeof(numbers2) / sizeof(int));
    int exp[] = {7, 13};
    vector<int> expected(exp, exp + sizeof(exp) / sizeof(int));
    Test("Test2", array1, array2, expected);
}
// no intersection
void Test3()
{
    int numbers1[] = {1, 3, 5, 7};
    vector<int> array1(numbers1, numbers1 + sizeof(numbers1) / sizeof(int));
    int numbers2[] = {2, 4, 6, 8};
    vector<int> array2(numbers2, numbers2 + sizeof(numbers2) / sizeof(int));
    vector<int> expected;
    Test("Test3", array1, array2, expected);
}
// one array only has one element
void Test4()
{
    int numbers1[] = {-2, 1, 4, 7};
    vector<int> array1(numbers1, numbers1 + sizeof(numbers1) / sizeof(int));
    vector<int> array2;
    array2.push_back(1);
    vector<int> expected;
    expected.push_back(1);
    Test("Test4", array1, array2, expected);
}
// one array is empty
void Test5()
{
    int numbers1[] = {-2, 1, 4, 7};
    vector<int> array1(numbers1, numbers1 + sizeof(numbers1) / sizeof(int));
    vector<int> array2;
    vector<int> expected;
    Test("Test5", array1, array2, expected);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
	return 0;
}
```

## 072. GreatestSumOfSubarrays


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class GreatestSumOfSubarrays {
    public static int getGreatestSumOfSubArray(int[] numbers) {
        int curSum = 0;
        int greatestSum = Integer.MIN_VALUE;
        for(int i = 0; i < numbers.length; ++i) {
            if(curSum <= 0)
                curSum = numbers[i];
            else
                curSum += numbers[i];
            if(curSum > greatestSum)
                greatestSum = curSum;
        }
        return greatestSum;
    } 
    
    //================= Test Code =================
    
    private static void test(String testName, int[] numbers, int expected) {
        System.out.print(testName + " begins: ");
        if(getGreatestSumOfSubArray(numbers) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    
    private static void test1() {
        int[] numbers = {1, -2, 3, 10, -4, 7, 2, -5};
        int expected = 18;
        test("Test1", numbers, expected);
    }
    
    // all numbers are negative    
    private static void test2() {
        int[] numbers = {-2, -8, -1, -5, -9};
        int expected = -1;
        test("Test2", numbers, expected);
    }
    
    // all numbers are positive    
    private static void test3() {
        int[] numbers = {2, 8, 1, 5, 9};
        int expected = 25;
        test("Test3", numbers, expected);
    }
    
    // only one number    
    private static void test4() {
        int[] numbers = {2};
        int expected = 2;
        test("Test4", numbers, expected);
    }
    
    public static void main(String[] argv) {
        test1();
        test2();
        test3();
        test4();
    }
}
```

## 073. NumberOf1


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
// ==================== Solution 1 ====================
int NumberOf1(unsigned int n);
int NumberOf1Between1AndN_Solution1(unsigned int n)
{
    int number = 0;
    unsigned int i;
    for(i = 0; i <= n; ++ i)
        number += NumberOf1(i);
    return number;
}
int NumberOf1(unsigned int n)
{
    int number = 0;
    while(n)
    {
        if(n % 10 == 1)
            number ++;
        n = n / 10;
    }
    return number;
}
// ==================== Solution 2 ====================
int NumberOf1InString(const char* strN);
int PowerBase10(unsigned int n);
int NumberOf1Between1AndN_Solution2(int n)
{
    char strN[50];
    if(n <= 0)
        return 0;
    sprintf(strN, "%d", n);
    return NumberOf1InString(strN);
}
int NumberOf1InString(const char* strN)
{
    int first, length;
    int numOtherDigits, numRecursive, numFirstDigit;
    
    if(!strN || *strN < '0' || *strN > '9' || *strN == '\0')
        return 0;
    first = *strN - '0';
    length = (unsigned int)(strlen(strN));
    if(length == 1 && first == 0)
        return 0;
    if(length == 1 && first > 0)
        return 1;
    // If strN is 21345, numFirstDigit is the number of digit 1 
    // in the most signification digit in numbers from 10000 to 19999
    numFirstDigit = 0;
    if(first > 1)
        numFirstDigit = PowerBase10(length - 1);
    else if(first == 1)
        numFirstDigit = atoi(strN + 1) + 1;
    // numOtherDigits is the number of digit 1 in digits except
    // the most significant digit in numbers from 01346 to 21345
    numOtherDigits = first * (length - 1) * PowerBase10(length - 2);
    // numRecursive is the number of digit 1 in numbers from 1 to 1345    
    numRecursive = NumberOf1InString(strN + 1);
    return numFirstDigit + numOtherDigits + numRecursive;
}
int PowerBase10(unsigned int n)
{
    int i, result = 1;
    for(i = 0; i < n; ++ i)
        result *= 10;
    return result;
}
// ==================== Test Code ====================
void Test(char* testName, int n, int expected)
{
    if(testName != NULL)
        printf("%s begins: \n", testName);
    
    if(NumberOf1Between1AndN_Solution1(n) == expected)
        printf("Solution1 passed.\n");
    else
        printf("Solution1 failed.\n"); 
    
    if(NumberOf1Between1AndN_Solution2(n) == expected)
        printf("Solution2 passed.\n");
    else
        printf("Solution2 failed.\n"); 
    printf("\n");
}
int main(int argc, char* argv[])
{
    Test("Test1", 1, 1);
    Test("Test2", 5, 1);
    Test("Test3", 10, 2);
    Test("Test4", 55, 16);
    Test("Test5", 99, 20);
    Test("Test6", 10000, 4001);
    Test("Test7", 21345, 18821);
    Test("Test8", 0, 0);
    return 0;
}
```

## 074. SortArrayForMinNumber


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.Arrays;
import java.util.Comparator;
public class SortArrayForMinNumber {    
    public static void PrintMinNumber(int numbers[]) {
        String strNumbers[] = new String[numbers.length];
        for(int i = 0; i < numbers.length; ++i) {
            strNumbers[i] = String.valueOf(numbers[i]);
        }
     
        Arrays.sort(strNumbers, (new SortArrayForMinNumber()).new NumericComparator());
     
        for(int i = 0; i < numbers.length; ++i)
            System.out.print(strNumbers[i]);
        System.out.print("\n");
    }
    public class NumericComparator implements Comparator<String>{
        public int compare(String num1, String num2) {
            String str1 = num1 + num2;
            String str2 = num2 + num1;
            return str1.compareTo(str2);
        }
    }
    
     // ====================Test code====================
    private static void Test(String testName, int numbers[], String expectedResult)
    {
        System.out.println(testName + " begins:");
        System.out.println("Expected result is: \t" + expectedResult);
        System.out.print("Actual result is: \t");
        PrintMinNumber(numbers);
        System.out.print("\n");
    }
    private static void Test1() {
        int numbers[] = {3, 5, 1, 4, 2};
        Test("Test1", numbers, "12345");
    }
    private static void Test2() {
        int numbers[] = {3, 32, 321};
        Test("Test2", numbers, "321323");
    }
    private static void Test3() {
        int numbers[] = {3, 323, 32123};
        Test("Test3", numbers, "321233233");
    }
    private static void Test4() {
        int numbers[] = {1, 11, 111};
        Test("Test4", numbers, "111111");
    }
    // There is only one element in an array
    private static void Test5(){
        int numbers[] = {321};
        Test("Test5", numbers, "321");
    }
    
    public static void main(String[] args) {
        Test1();
        Test2();
        Test3();
        Test4();
        Test5();
    }
}
```

## 076. FirstNotRepeatingChar


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string>
char FirstNotRepeatingChar(char* pString)
{
    if(pString == NULL)
        return '\0';
    const int tableSize = 256;
    unsigned int hashTable[tableSize];
    for(unsigned int i = 0; i<tableSize; ++ i)
        hashTable[i] = 0;
    char* pHashKey = pString;
    while(*(pHashKey) != '\0')
        hashTable[*(pHashKey++)] ++;
    pHashKey = pString;
    while(*pHashKey != '\0')
    {
        if(hashTable[*pHashKey] == 1)
            return *pHashKey;
        pHashKey++;
    }
    return '\0';
} 
// ==================== Test Code ====================
void Test(char* testName, char* pString, char expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    if(FirstNotRepeatingChar(pString) == expected)
        printf("Test passed.\n");
    else
        printf("Test failed.\n");
}
int main(int argc, char* argv[])
{
    // There is a character appearing once
    Test("Test1", "google", 'l');
    // There is not a character appearing once
    Test("Test2", "aabccdbd", '\0');
    // All characters appear only once
    Test("Test3", "abcdefg", 'a');
    // NULL
    Test("Test4", NULL, '\0');
    
    // Empty string
    Test("Test5", "", '\0');
    return 0;
}
```

## 077. FirstCharAppearingOnce


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <vector>
#include <limits>
using namespace std;
class CharStatistics
{
public:
    CharStatistics() : index (0)
    {
        for(int i = 0; i < 256; ++i)
            occurrence[i] = -1;
    }
    
    void Insert(char ch)
    {
        if(occurrence[ch] == -1)
            occurrence[ch] = index;
        else if(occurrence[ch] >= 0)
            occurrence[ch] = -2;  
        
        index++;  
    }
    
    char FirstAppearingOnce()
    {
        char ch = '\0';
        int minIndex = numeric_limits<int>::max();
        for(int i = 0; i < 256; ++i)
        {
            if(occurrence[i] >= 0 && occurrence[i] < minIndex)
            {
                ch = (char)i;
                minIndex = occurrence[i];
            }
        }
        
        return ch;
    }
    
private:
    // occurrence[i]: A character with ASCII value i;
    // occurrence[i] = -1: The character has not found;
    // occurrence[i] = -2: The character has been found for mutlple times
    // occurrence[i] >= 0: The character has been found only once
    int occurrence[256];
    int index;
};
void Test(char* testName, CharStatistics chars, char expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(chars.FirstAppearingOnce() == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
int main(int argc, char* argv[])
{
    CharStatistics chars;
    Test("Test1", chars, '\0');
    chars.Insert('g');
    Test("Test2", chars, 'g');
    chars.Insert('o');
    Test("Test3", chars, 'g');
    chars.Insert('o');
    Test("Test4", chars, 'g');
    chars.Insert('g');
    Test("Test5", chars, '\0');
    chars.Insert('l');
    Test("Test6", chars, 'l');
    chars.Insert('e');
    Test("Test7", chars, 'l');
	return 0;
}
```

## 078. DelelteCharacters


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string.h>
void DeleteCharacters(char* pString, char* pCharsToBeDeleted)
{
    int hashTable[256];
    const char* pTemp = pCharsToBeDeleted;
    char* pSlow = pString;
    char* pFast = pString;
    if(pString == NULL || pCharsToBeDeleted == NULL)
        return;
    memset(hashTable, 0, sizeof(hashTable));
    while (*pTemp != '\0')
    {
        hashTable[*pTemp] = 1;
        ++ pTemp;
    }
    while (*pFast != '\0')
    {
        if(hashTable[*pFast] != 1)
        {
            *pSlow = *pFast;
            ++ pSlow;
        }
        ++pFast;
    }
    *pSlow = '\0';
}
// ==================== Test Code ====================
void Test(char* testName, char* pString, char* pCharsToBeDeleted, char* pExpected)
{
    if(NULL != testName)
        printf("%s begins: ", testName);
    DeleteCharacters(pString, pCharsToBeDeleted);
    if((pString == NULL && pExpected == NULL) || (0 == strcmp(pString, pExpected)))
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
void Test1()
{
    char pString[] = "We are students";
    char* pCharsToBeDeleted = "aeiou";
    char* pExpected = "W r stdnts";
    Test("Test1", pString, pCharsToBeDeleted, pExpected);
}
void Test2()
{
    char pString[] = "We are students";
    char* pCharsToBeDeleted = "wxyz";
    char* pExpected = "We are students";
    Test("Test2", pString, pCharsToBeDeleted, pExpected);
}
void Test3()
{
    char pString[] = "8613761352066";
    char* pCharsToBeDeleted = "1234567890";
    char* pExpected = "";
    Test("Test3", pString, pCharsToBeDeleted, pExpected);
}
void Test4()
{
    char pString[] = "119911";
    char* pCharsToBeDeleted = "";
    char* pExpected = "119911";
    Test("Test4", pString, pCharsToBeDeleted, pExpected);
}
void Test5()
{
    char pString[] = "119911";
    char* pCharsToBeDeleted = "aeiou";
    char* pExpected = "119911";
    Test("Test5", pString, pCharsToBeDeleted, pExpected);
}
void Test6()
{
    char pString[] = "";
    char* pCharsToBeDeleted = "aeiou";
    char* pExpected = "";
    Test("Test6", pString, pCharsToBeDeleted, pExpected);
}
void Test7()
{
    char* pString = NULL;
    char* pCharsToBeDeleted = "aeiou";
    char* pExpected = NULL;
    Test("Test7", pString, pCharsToBeDeleted, pExpected);
}
void Test8()
{
    char pString[] = "abcdefg";
    char* pCharsToBeDeleted = NULL;
    char* pExpected = "abcdefg";
    Test("Test8", pString, pCharsToBeDeleted, pExpected);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
}
```

## 079. DelelteDuplicatedCharacters


```c

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string.h>
void DeletedDuplication(char* pString)
{
    int hashTable[256];
    char* pSlow = pString;
    char* pFast = pString;
    if(pString == NULL)
        return;
    memset(hashTable, 0, sizeof(hashTable));
    while (*pFast != '\0')
    {
        *pSlow = *pFast;
        
        if(hashTable[*pFast] == 0)
        {
            ++ pSlow;
            hashTable[*pFast] = 1;
        }
        
        ++pFast;
    }
    *pSlow = '\0';
}
// ==================== Test Code ====================
void Test(char* testName, char* pString, char* pExpected)
{
    if(NULL != testName)
        printf("%s begins: ", testName);
    DeletedDuplication(pString);
    if((pString == NULL && pExpected == NULL) || (0 == strcmp(pString, pExpected)))
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
void Test1()
{
    char pString[] = "google";
    char* pExpected = "gole";
    Test("Test1", pString, pExpected);
}
void Test2()
{
    char pString[] = "aaaaaaaaaaaaaaaaaaaaa";
    char* pExpected = "a";
    Test("Test2", pString, pExpected);
}
void Test3()
{
    char pString[] = "";
    char* pExpected = "";
    Test("Test3", pString, pExpected);
}
void Test4()
{
    char pString[] = "abcacbbacbcacabcba";
    char* pExpected = "abc";
    Test("Test4", pString, pExpected);
}
void Test5()
{
    char pString[] = "abcdefg";
    char* pExpected = "abcdefg";
    Test("Test5", pString, pExpected);
}
void Test6()
{
    char* pString = NULL;
    char* pExpected = NULL;
    Test("Test6", pString, pExpected);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
}
```

## 080. Anagram


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.HashMap;
import java.util.Map;
public class Anagram {
    public static boolean areAnagrams(String str1, String str2){
        if(str1.length() != str2.length())
            return false;
        
        Map<Character, Integer> times = new HashMap<Character, Integer>();
        for(int i = 0; i < str1.length(); ++i){
            Character ch = str1.charAt(i);
            if(times.containsKey(ch))
                times.put(ch, times.get(ch) + 1);
            else
                times.put(ch, 1);
        }
        
        for(int i = 0; i < str2.length(); ++i){
            Character ch = str2.charAt(i);
            if(!times.containsKey(ch))
                return false;
            
            if(times.get(ch) == 0)
                return false;
            
            times.put(ch, times.get(ch) - 1);
        }
        
        // If two strings with same length and containing
        // same set of characters are not anagrams,
        // it returns false before, because values of some
        // characters reach 0 in the second for loop above.
        // Therefore, it is safe to return true here.
        return true;
    }
    //================= Test Code =================
    private static void test(String testName, String str1, String str2, boolean expected){
        System.out.print(testName + " begins: ");
        if(areAnagrams(str1, str2) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    
    private static void test1(){
        test("Test1", "silent", "listen", true);
    }
    
    private static void test2(){
        test("Test2", "evil", "live", true);
    }
    
    private static void test3(){
        test("Test3", "eleven plus two", "twelve plus one", true);
    }
    
    private static void test4(){
        test("Test4", "there", "their", false);
    }
    
    private static void test5(){
        test("Test5", "an", "and", false);
    }
    
    static public void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
    }
}
```

## 081. ReversedPairs


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class ReversedPairs {
    public static int countReversedPairs(int[] numbers) {
        int[] buffer = new int[numbers.length];
        return countReversedPairs(numbers, buffer, 0, numbers.length - 1);
    }
    
    private static int countReversedPairs(int[] numbers, int[] buffer, int start, int end) {
        if(start >= end)
            return 0;
        
        int middle = start + (end - start) / 2;
        
        int left = countReversedPairs(numbers, buffer, start, middle);
        int right = countReversedPairs(numbers, buffer, middle + 1, end);
        int between = merge(numbers, buffer, start, middle, end);
        
        return left + right + between; 
    }
    
    private static int merge(int[] numbers, int[] buffer, int start, int middle, int end) {
        int i = middle; // the end of the first sub-array
        int j = end;    // the end of the second sub-array
        int k = end;    // the end of the merged array
        int count = 0;
        while(i >= start && j >= middle + 1) {
            if(numbers[i] > numbers[j]) {
                buffer[k--] = numbers[i--];
                count += (j - middle);
            }
            else {
                buffer[k--] = numbers[j--];
            }   
        }
        
        while(i >= start) {
            buffer[k--] = numbers[i--];
        }
        
        while(j >= middle + 1) {
            buffer[k--] = numbers[j--];
        }
        
        // copy elements from buffer[] to numbers[]
        for(i = start; i <= end; ++i) {
            numbers[i] = buffer[i];
        }
        
        return count;       
    }
    //================= Test Code =================
    
    private static void test(String testName, int[] numbers, int expected) {
        System.out.print(testName + " begins: ");
        if(countReversedPairs(numbers) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    
    private static void test1() {
        int[] numbers = {1, 2, 3, 4, 7, 6, 5};
        int expected = 3;
        test("Test1", numbers, expected);
    }
    
    // decreasingly sorted array
    private static void test2() {
        int[] numbers = {6, 5, 4, 3, 2, 1};
        int expected = 15;
        test("Test2", numbers, expected);
    }
    
    // increasingly sorted array
    private static void test3() {
        int[] numbers = {1, 2, 3, 4, 5, 6};
        int expected = 0;
        test("Test3", numbers, expected);
    }
    
    // only one number
    private static void test4() {
        int[] numbers = {1};
        int expected = 0;
        test("Test4", numbers, expected);
    }
    
    // two numbers
    private static void test5() {
        int[] numbers = {1, 2};
        int expected = 0;
        test("Test5", numbers, expected);
    }
    
    // two numbers
    private static void test6() {
        int[] numbers = {2, 1};
        int expected = 1;
        test("Test5", numbers, expected);
    }
    
    // duplicated numbers
    private static void test7() {
        int[] numbers = {1, 2, 1, 2, 1};
        int expected = 3;
        test("Test7", numbers, expected);
    }
    
    // duplicated numbers
    private static void test8() {
        int[] numbers = {7, 5, 4, 5, 3, 5, 6, 5};
        int expected = 12;
        test("Test8", numbers, expected);
    }
    
    public static void main(String[] argv) {
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

## 082. FirstCommonNodesInLists


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\List.h"
unsigned int GetListLength(ListNode* pHead);
ListNode* FindFirstCommonNode( ListNode *pHead1, ListNode *pHead2)
{
    // Get length of two lists
    unsigned int nLength1 = GetListLength(pHead1);
    unsigned int nLength2 = GetListLength(pHead2);
    int nLengthDif = nLength1 - nLength2;
 
    ListNode* pListHeadLong = pHead1;
    ListNode* pListHeadShort = pHead2;
    if(nLength2 > nLength1)
    {
        pListHeadLong = pHead2;
        pListHeadShort = pHead1;
        nLengthDif = nLength2 - nLength1;
    }
 
    // Move d steps on the longer list if the length difference is d
    for(int i = 0; i < nLengthDif; ++ i)
        pListHeadLong = pListHeadLong->m_pNext;
 
    while((pListHeadLong != NULL) && 
        (pListHeadShort != NULL) &&
        (pListHeadLong != pListHeadShort))
    {
        pListHeadLong = pListHeadLong->m_pNext;
        pListHeadShort = pListHeadShort->m_pNext;
    }
 
    // Traverse two lists
    ListNode* pFisrtCommonNode = pListHeadLong;
 
    return pFisrtCommonNode;
}
unsigned int GetListLength(ListNode* pHead)
{
    unsigned int nLength = 0;
    ListNode* pNode = pHead;
    while(pNode != NULL)
    {
        ++ nLength;
        pNode = pNode->m_pNext;
    }
 
    return nLength;
}
// ==================== Test Code ====================
void DestroyNode(ListNode* pNode);
void Test(char* testName, ListNode* pHead1, ListNode* pHead2, ListNode* pExpected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    ListNode* pResult = FindFirstCommonNode(pHead1, pHead2);
    if(pResult == pExpected)
        printf("Passed.\n");
    else
        printf("Failed.\n");
}
// 1 - 2 - 3 \
//            6 - 7
//     4 - 5 /
void Test1()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ListNode* pNode7 = CreateListNode(7);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode6);
    ConnectListNodes(pNode4, pNode5);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    Test("Test1", pNode1, pNode4, pNode6);
    DestroyNode(pNode1);
    DestroyNode(pNode2);
    DestroyNode(pNode3);
    DestroyNode(pNode4);
    DestroyNode(pNode5);
    DestroyNode(pNode6);
    DestroyNode(pNode7);
}
// No intersection
// 1 - 2 - 3 - 4
//            
// 5 - 6 - 7
void Test2()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ListNode* pNode7 = CreateListNode(7);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    Test("Test2", pNode1, pNode5, NULL);
    DestroyList(pNode1);
    DestroyList(pNode5);
}
// 1 - 2 - 3 - 4 \
//                7
//         5 - 6 /
void Test3()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ListNode* pNode6 = CreateListNode(6);
    ListNode* pNode7 = CreateListNode(7);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode7);
    ConnectListNodes(pNode5, pNode6);
    ConnectListNodes(pNode6, pNode7);
    Test("Test3", pNode1, pNode5, pNode7);
    DestroyNode(pNode1);
    DestroyNode(pNode2);
    DestroyNode(pNode3);
    DestroyNode(pNode4);
    DestroyNode(pNode5);
    DestroyNode(pNode6);
    DestroyNode(pNode7);
}
// Two lists are same
// 1 - 2 - 3 - 4 - 5
void Test4()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test4", pNode1, pNode1, pNode1);
    DestroyList(pNode1);
}
// One of two lists is empty
void Test5()
{
    ListNode* pNode1 = CreateListNode(1);
    ListNode* pNode2 = CreateListNode(2);
    ListNode* pNode3 = CreateListNode(3);
    ListNode* pNode4 = CreateListNode(4);
    ListNode* pNode5 = CreateListNode(5);
    ConnectListNodes(pNode1, pNode2);
    ConnectListNodes(pNode2, pNode3);
    ConnectListNodes(pNode3, pNode4);
    ConnectListNodes(pNode4, pNode5);
    Test("Test5", NULL, pNode1, NULL);
    DestroyList(pNode1);
}
// Two lists are empty
void Test6()
{
    Test("Test6", NULL, NULL, NULL);
}
void DestroyNode(ListNode* pNode)
{
    delete pNode;
    pNode = NULL;
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    return 0;
}
```

## 083. Occurrence


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class Occurrence {
    static public int countOccurrence(int[] numbers, int k) {
        int first = getFirst(numbers, 0, numbers.length - 1, k);
        int last = getLast(numbers, 0, numbers.length - 1, k);
        
        int occurrence = 0;
        if(first > -1 && last > -1)
            occurrence = last - first + 1;
        
        return occurrence;
    }
    
    // Get the index of the first k. It returns -1 if no k exists.
    static private int getFirst(int[] numbers, int start, int end, int k) {
        if(start > end)
            return -1;
        
        int middle = start + (end - start) / 2;
        if(numbers[middle] == k) {
            if((middle > 0 && numbers[middle - 1] != k)
                    || (middle == 0))
                return middle;
            
            end = middle - 1;
        }
        else if(numbers[middle] > k){
            end = middle - 1;
        }
        else {
            start = middle + 1;
        }
        
        return getFirst(numbers, start, end, k);
    }
    
    // Get the index of the last k. It returns -1 if no k exists.
    static private int getLast(int[] numbers, int start, int end, int k) {
        if(start > end)
            return -1;
        
        int middle = start + (end - start) / 2;
        if(numbers[middle] == k) {
            if((middle < numbers.length - 1 && numbers[middle + 1] != k)
                    || (middle == numbers.length - 1))
                return middle;
            
            start = middle + 1;
        }
        else if(numbers[middle] > k){
            end = middle - 1;
        }
        else {
            start = middle + 1;
        }
        
        return getLast(numbers, start, end, k);
    }
    
    //================= Test Code =================
    
    private static void test(String testName, int[] numbers, int k, int expected) {
        System.out.print(testName + " begins: ");
        if(countOccurrence(numbers, k) == expected)
            System.out.print("passed.\n");
        else
            System.out.print("FAILED.\n");
    }
    
    // target is in the array
    private static void test1()
    {
        int[] data = {1, 2, 3, 3, 3, 3, 4, 5};
        int k = 3;
        int expected = 4;
        test("Test1", data, k, expected);
    }
    // target is at the beginning of the array
    private static void test2()
    {
        int[] data = {3, 3, 3, 3, 4, 5};
        int k = 3;
        int expected = 4;
        test("Test2", data, k, expected);
    }
    
    // target is at the end of the array
    private static void test3()
    {
        int[] data = {1, 2, 3, 3, 3, 3};
        int k = 3;
        int expected = 4;
        test("Test2", data, k, expected);
    }   
    
    // target does not exist in the array
    private static void test4()
    {
        int[] data = {1, 3, 3, 3, 3, 4, 5};
        int k = 2;
        int expected = 0;
        test("Test4", data, k, expected);
    }
    
    // target is less than the first element in the array
    private static void test5()
    {
        int[] data = {1, 3, 3, 3, 3, 4, 5};
        int k = 0;
        int expected = 0;
        test("Test5", data, k, expected);
    }
    
    // target is greater than the first element in the array
    private static void test6()
    {
        int[] data = {1, 3, 3, 3, 3, 4, 5};
        int k = 6;
        int expected = 0;
        test("Test6", data, k, expected);
    }
    
    // all numbers are duplicated, and target is in the array
    private static void test7()
    {
        int[] data = {3, 3, 3, 3};
        int k = 3;
        int expected = 4;
        test("Test7", data, k, expected);
    }
    
    // all numbers are duplicated, and target is not in the array
    private static void test8()
    {
        int[] data = {3, 3, 3, 3};
        int k = 4;
        int expected = 0;
        test("Test8", data, k, expected);
    }
    
    // only one number exists in the array, and target is in the array
    private static void test9()
    {
        int[] data = {3};
        int k = 3;
        int expected = 1;
        test("Test9", data, k, expected);
    }
        
    // only one number exists in the array, and target is not in the array
    private static void test10()
    {
        int[] data = {3};
        int k = 4;
        int expected = 0;
        test("Test10", data, k, expected);
    }
    
    public static void main(String[] argv) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
        test7();
        test8();
        test9();
        test10();
    }
}
```

## 084. KthNodeInBST


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdlib.h>
#include <stdio.h>
#include "../Utility/BinaryTree.h"
BinaryTreeNode* KthNodeCore(BinaryTreeNode* pRoot, unsigned int& k);
BinaryTreeNode* KthNode(BinaryTreeNode* pRoot, unsigned int k)
{
    if(pRoot == NULL || k == 0)
        return NULL;
    
    return KthNodeCore(pRoot, k);
}
BinaryTreeNode* KthNodeCore(BinaryTreeNode* pRoot, unsigned int& k)
{
    BinaryTreeNode* target = NULL;
    
    if(pRoot->m_pLeft != NULL)
        target = KthNodeCore(pRoot->m_pLeft, k);
    
    if(target == NULL)
    {
        if(k == 1)
            target = pRoot;
        
        k--;
    }
    
    if(target == NULL && pRoot->m_pRight != NULL)
        target = KthNodeCore(pRoot->m_pRight, k);
    
    return target;
}
void Test(char* testName, BinaryTreeNode* pRoot, unsigned int k, bool isNull, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    BinaryTreeNode* pTarget = KthNode(pRoot, k);
    if((isNull && pTarget == NULL) || (!isNull && pTarget->m_nValue == expected))
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
//            8
//        6      10
//       5 7    9  11
void TestA()
{
    BinaryTreeNode* pNode8 = CreateBinaryTreeNode(8);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode10 = CreateBinaryTreeNode(10);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    BinaryTreeNode* pNode9 = CreateBinaryTreeNode(9);
    BinaryTreeNode* pNode11 = CreateBinaryTreeNode(11);
    ConnectTreeNodes(pNode8, pNode6, pNode10);
    ConnectTreeNodes(pNode6, pNode5, pNode7);
    ConnectTreeNodes(pNode10, pNode9, pNode11);
    Test("TestA0", pNode8, 0, true, -1);
    Test("TestA1", pNode8, 1, false, 5);
    Test("TestA2", pNode8, 2, false, 6);
    Test("TestA3", pNode8, 3, false, 7);
    Test("TestA4", pNode8, 4, false, 8);
    Test("TestA5", pNode8, 5, false, 9);
    Test("TestA6", pNode8, 6, false, 10);
    Test("TestA7", pNode8, 7, false, 11);
    Test("TestA8", pNode8, 8, true, -1);
    DestroyTree(pNode8);
    
    printf("\n\n");
}
//               5
//              /
//             4
//            /
//           3
//          /
//         2
//        /
//       1
void TestB()
{
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    ConnectTreeNodes(pNode5, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode1, NULL);
    Test("TestB0", pNode5, 0, true, -1);
    Test("TestB1", pNode5, 1, false, 1);
    Test("TestB2", pNode5, 2, false, 2);
    Test("TestB3", pNode5, 3, false, 3);
    Test("TestB4", pNode5, 4, false, 4);
    Test("TestB5", pNode5, 5, false, 5);
    Test("TestB6", pNode5, 6, true, -1);
    DestroyTree(pNode5);
    printf("\n\n");
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
void TestC()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, NULL, pNode2);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("TestC0", pNode1, 0, true, -1);
    Test("TestC1", pNode1, 1, false, 1);
    Test("TestC2", pNode1, 2, false, 2);
    Test("TestC3", pNode1, 3, false, 3);
    Test("TestC4", pNode1, 4, false, 4);
    Test("TestC5", pNode1, 5, false, 5);
    Test("TestC6", pNode1, 6, true, -1);
    DestroyTree(pNode1);
    printf("\n\n");
}
// There is only one node in a tree
void TestD()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    Test("TestD0", pNode1, 0, true, -1);
    Test("TestD1", pNode1, 1, false, 1);
    Test("TestD2", pNode1, 2, true, -1);
    DestroyTree(pNode1);
    printf("\n\n");
}
// empty tree
void TestE()
{
    Test("TestE0", NULL, 0, true, -1);
    Test("TestE1", NULL, 1, true, -1);
    printf("\n\n");
}
int main(int argc, char* argv[])
{
    TestA();
    TestB();
    TestC();
    TestD();
    TestE();
}
```

## 085. TreeDepth


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
int TreeDepth(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return 0;
    int nLeft = TreeDepth(pRoot->m_pLeft);
    int nRight = TreeDepth(pRoot->m_pRight);
    return (nLeft > nRight) ? (nLeft + 1) : (nRight + 1);
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    
    int result = TreeDepth(pRoot);
    if(expected == result)
        printf("Test passed.\n");
    else
        printf("Test failed.\n");
}
//            1
//         /      \
//        2        3
//       /\         \
//      4  5         6
//        /
//       7
void Test1()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNode1, pNode2, pNode3);
    ConnectTreeNodes(pNode2, pNode4, pNode5);
    ConnectTreeNodes(pNode3, NULL, pNode6);
    ConnectTreeNodes(pNode5, pNode7, NULL);
    Test("Test1", pNode1, 4);
    DestroyTree(pNode1);
}
//               1
//              /
//             2
//            /
//           3
//          /
//         4
//        /
//       5
void Test2()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode5, NULL);
    Test("Test2", pNode1, 5);
    DestroyTree(pNode1);
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
void Test3()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, NULL, pNode2);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("Test3", pNode1, 5);
    DestroyTree(pNode1);
}
// Only one node
void Test4()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    Test("Test4", pNode1, 1);
    DestroyTree(pNode1);
}
// Empty tree
void Test5()
{
    Test("Test5", NULL, 0);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    return 0;
}
```

## 086. BalancedBinaryTree


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include "..\Utility\BinaryTree.h"
// ==================== Solution 1 ====================
int TreeDepth(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return 0;
    int nLeft = TreeDepth(pRoot->m_pLeft);
    int nRight = TreeDepth(pRoot->m_pRight);
    return (nLeft > nRight) ? (nLeft + 1) : (nRight + 1);
}
bool IsBalanced_Solution1(BinaryTreeNode* pRoot)
{
    if(pRoot == NULL)
        return true;
    int left = TreeDepth(pRoot->m_pLeft);
    int right = TreeDepth(pRoot->m_pRight);
    int diff = left - right;
    if(diff > 1 || diff < -1)
        return false;
    return IsBalanced_Solution1(pRoot->m_pLeft) 
        && IsBalanced_Solution1(pRoot->m_pRight);
}
// ==================== Solution 2 ====================
bool IsBalanced(BinaryTreeNode* pRoot, int* pDepth);
bool IsBalanced_Solution2(BinaryTreeNode* pRoot)
{
    int depth = 0;
    return IsBalanced(pRoot, &depth);
}
bool IsBalanced(BinaryTreeNode* pRoot, int* pDepth)
{
    if(pRoot == NULL)
    {
        *pDepth = 0;
        return true;
    }
    int left, right;
    if(IsBalanced(pRoot->m_pLeft, &left) 
        && IsBalanced(pRoot->m_pRight, &right))
    {
        int diff = left - right;
        if(diff <= 1 && diff >= -1)
        {
            *pDepth = 1 + (left > right ? left : right);
            return true;
        }
    }
    return false;
}
// ==================== Test Code ====================
void Test(char* testName, BinaryTreeNode* pRoot, bool expected)
{
    if(testName != NULL)
        printf("%s begins:\n", testName);
    printf("Solution1 begins: ");
    if(IsBalanced_Solution1(pRoot) == expected)
        printf("Passed.\n");
    else
        printf("Failed.\n");
    printf("Solution2 begins: ");
    if(IsBalanced_Solution2(pRoot) == expected)
        printf("Passed.\n");
    else
        printf("Failed.\n");
}
//             1
//         /      \
//        2        3
//       /\       / \
//      4  5     6   7
void Test1()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNode1, pNode2, pNode3);
    ConnectTreeNodes(pNode2, pNode4, pNode5);
    ConnectTreeNodes(pNode3, pNode6, pNode7);
    Test("Test1", pNode1, true);
    DestroyTree(pNode1);
}
//             1
//         /      \
//        2        3
//       /\         \
//      4  5         6
//        /
//       7
void Test2()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    BinaryTreeNode* pNode7 = CreateBinaryTreeNode(7);
    ConnectTreeNodes(pNode1, pNode2, pNode3);
    ConnectTreeNodes(pNode2, pNode4, pNode5);
    ConnectTreeNodes(pNode3, NULL, pNode6);
    ConnectTreeNodes(pNode5, pNode7, NULL);
    Test("Test2", pNode1, true);
    DestroyTree(pNode1);
}
//             1
//         /      \
//        2        3
//       /\         
//      4  5        
//        /
//       6
void Test3()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    BinaryTreeNode* pNode6 = CreateBinaryTreeNode(6);
    ConnectTreeNodes(pNode1, pNode2, pNode3);
    ConnectTreeNodes(pNode2, pNode4, pNode5);
    ConnectTreeNodes(pNode5, pNode6, NULL);
    Test("Test3", pNode1, false);
    DestroyTree(pNode1);
}
//               1
//              /
//             2
//            /
//           3
//          /
//         4
//        /
//       5
void Test4()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, pNode2, NULL);
    ConnectTreeNodes(pNode2, pNode3, NULL);
    ConnectTreeNodes(pNode3, pNode4, NULL);
    ConnectTreeNodes(pNode4, pNode5, NULL);
    Test("Test4", pNode1, false);
    DestroyTree(pNode1);
}
// 1
//  \
//   2
//    \
//     3
//      \
//       4
//        \
//         5
void Test5()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    BinaryTreeNode* pNode2 = CreateBinaryTreeNode(2);
    BinaryTreeNode* pNode3 = CreateBinaryTreeNode(3);
    BinaryTreeNode* pNode4 = CreateBinaryTreeNode(4);
    BinaryTreeNode* pNode5 = CreateBinaryTreeNode(5);
    ConnectTreeNodes(pNode1, NULL, pNode2);
    ConnectTreeNodes(pNode2, NULL, pNode3);
    ConnectTreeNodes(pNode3, NULL, pNode4);
    ConnectTreeNodes(pNode4, NULL, pNode5);
    Test("Test5", pNode1, false);
    DestroyTree(pNode1);
}
// Only one node
void Test6()
{
    BinaryTreeNode* pNode1 = CreateBinaryTreeNode(1);
    Test("Test6", pNode1, true);
    DestroyTree(pNode1);
}
// Empty tree
void Test7()
{
    Test("Test7", NULL, true);
}
int main(int argc, char* argv[])
{
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

## 087. TwoNumbersWithSum


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class TwoNumbersWithSum {
    public static boolean hasPairWithSum(int numbers[], int sum) {
        boolean found = false;
    
        int ahead = numbers.length - 1;
        int behind = 0;
    
        while(ahead > behind) {
            int curSum = numbers[ahead] + numbers[behind];
        
            if(curSum == sum) {
                found = true;
                break;
            }
            else if(curSum > sum)
                ahead --;
            else
                behind ++;
        }
    
        return found;
    }
    //================= Test Code =================
    private static void test(String testName, int numbers[], int sum, boolean expected) {
        System.out.print(testName + " Begins: ");
        
        if(hasPairWithSum(numbers, sum) == expected)
            System.out.println("Passed.");
        else
            System.out.println("FAILED.");
    }
    
    // The array has two numbers with the given sum,
    // and two numbers are inside the array
    private static void test1() {
        int numbers[] = {1, 2, 4, 7, 11, 15};
        test("test1", numbers, 15, true);
    }
    // The array has two numbers with the given sum,
    // and two numbers are the first and last elements
    private static void test2() {
        int numbers[] = {1, 2, 4, 7, 11, 16};
        test("test2", numbers, 17, true);
    }
    // The array does not have two numbers with the given sum
    private static void test3() {
        int numbers[] = {1, 2, 4, 7, 11, 16};
        test("test3", numbers, 10, false);
    }
    // There is only one number in the array
    private static void test4() {
        int numbers[] = {2};
        test("test4", numbers, 2, false);
    }
    
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
    }
}
```

## 088. ThreeNumbersWithSum


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.Arrays;
public class ThreeNumbersWithSum {
    public static boolean hasTripleWithSum0(int numbers[]) {
        boolean found = false;
        if(numbers.length < 3)
            return found;
        Arrays.sort(numbers);
        for(int i = 0; i < numbers.length; ++i) {
            int sum = -numbers[i];
            found = hasPairWithSum(numbers, sum, i);
            if(found)
                break;
        }
        return found;
    }
    private static boolean hasPairWithSum(int numbers[], int sum, int excludeIndex) {
        boolean found = false;
    
        int ahead = numbers.length - 1;
        int behind = 0;
    
        while(ahead > behind) {
            if(ahead == excludeIndex)
                ahead--;
            if(behind == excludeIndex)
                behind++;
            
            int curSum = numbers[ahead] + numbers[behind];
        
            if(curSum == sum) {
                found = true;
                break;
            }
            else if(curSum > sum)
                ahead --;
            else
                behind ++;
        }
    
        return found;
    }
    
    //================= Test Code =================
    private static void test(String testName, int numbers[], boolean expected) {
        System.out.print(testName + " Begins: ");
        
        if(hasTripleWithSum0(numbers) == expected)
            System.out.println("Passed.");
        else
            System.out.println("FAILED.");
    }
    
    // The array has three numbers with the sum 0
    private static void test1() {
        int numbers[] = {1, 2, -4, 7, 11, 3};
        test("test1", numbers, true);
    }
    
    // The array does not have three numbers with the sum 0
    private static void test2() {
        int numbers[] = {1, 2, 4, 7, 11, 5};
        test("test2", numbers, false);
    }    
    
    // The array with three numbers,
    // and the sum of three numbers is 0
    private static void test3() {
        int numbers[] = {1, 2, -3};
        test("test3", numbers, true);
    }
    
    // The array with three numbers,
    // and the sum of three numbers is not 0
    private static void test4() {
        int numbers[] = {1, 2, 3};
        test("test4", numbers, false);
    }
    
    // The length of the array is less than 3,
    private static void test5() {
        int numbers[] = {1, 2};
        test("test5", numbers, false);
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

## 089. SubsetWithSum


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
import java.util.BitSet;
public class SubsetWithSum {
    public static boolean hasSubsetWithSum0(int numbers[]) {
        BitSet bits = new BitSet(numbers.length);
        while(increment(bits, numbers.length)) {
            int sum = 0;
            boolean oneBitAtLeast = false;
            for(int i = 0; i < numbers.length; ++i) {
                if(bits.get(i)) {
                    if(!oneBitAtLeast)
                        oneBitAtLeast = true;
                    
                    sum += numbers[i];
                }
            }
            
            if(oneBitAtLeast && sum == 0)
                return true;
        }
        
        return false;
    }
    
    private static boolean increment(BitSet bits, int length) {
        int index = length - 1;
        
        while(index >= 0 && bits.get(index)) {
            bits.clear(index);
            --index;
        }
        
        if(index < 0)
            return false;
        
        bits.set(index);
        return true;
    }
    // ==================== test code ====================
    private static void test(String testName, int numbers[], boolean expected) {
        System.out.print(testName + " Begins: ");
        
        if(hasSubsetWithSum0(numbers) == expected)
            System.out.println("Passed.");
        else
            System.out.println("FAILED.");
    }
    // The array has a zero
    private static void test1() {
        int numbers[] = {1, 2, 4, 0, 11, 15};
        test("test1", numbers, true);
    }
    
    // The array has two numbers whose sum is 0,
    private static void test2() {
        int numbers[] = {1, 2, -11, 7, 11, 15};
        test("test2", numbers, true);
    }
    
    // The array has three numbers whose sum is 0,
    private static void test3() {
        int numbers[] = {1, 2, -11, 7, 11, 15};
        test("test3", numbers, true);
    }
    
    // The array has four numbers whose sum is 0,
    private static void test4() {
        int numbers[] = {1, 2, -11, 7, 8, 15};
        test("test4", numbers, true);
    }
    
    // The array has five numbers whose sum is 0,
    private static void test5() {
        int numbers[] = {1, 2, -11, 4, 5, 15};
        test("test5", numbers, true);
    }
    
    // The array has six numbers whose sum is 0,
    private static void test6() {
        int numbers[] = {1, 3, 5, 4, 7, -20};
        test("test6", numbers, true);
    }
    
    // The array does not have a subset with sum is 0,
    private static void test7() {
        int numbers[] = {1, -3, 5, 4, 7, -20};
        test("test7", numbers, false);
    }
    
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
        test7();
    }
}
```

## 090. ContinousSequenceWithSum


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class ContinousSequenceWithSum {
    public static void findContinuousSequence(int sum) {
        if(sum < 3)
            return;
        int small = 1; 
        int big = 2;
        int middle = (1 + sum) / 2;
        int curSum = small + big;
        while(small < middle) {
            if(curSum == sum)
                printContinuousSequence(small, big);
            while(curSum > sum && small < middle) {
                curSum -= small;
                ++small;
                if(curSum == sum)
                    printContinuousSequence(small, big);
            }
            ++big;
            curSum += big;
        }
    }
    
    private static void printContinuousSequence(int small, int big){
        for(int i = small; i <= big; ++i)
            System.out.print(String.valueOf(i) + " ");
        System.out.println("");
    }
    
    // ==================== test code ====================
    private static void test(String testName, int sum) {
        System.out.println(testName + " for " + String.valueOf(sum) + " begins: ");
        
        findContinuousSequence(sum);
    }
    
    public static void main(String[] args) {
        test("test1", 1);
        test("test2", 3);
        test("test3", 4);
        test("test4", 9);
        test("test5", 15);
        test("test6", 100);
    }
}
```

## 091. ReverseWordsInSentence


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string>
void Reverse(char *pBegin, char *pEnd) 
{
    if(pBegin == NULL || pEnd == NULL)
        return;
    while(pBegin < pEnd) 
    {
        char temp = *pBegin;
        *pBegin = *pEnd;
        *pEnd = temp;
        pBegin ++, pEnd --;
    }
}	
char* ReverseSentence(char *pData)
{
    if(pData == NULL)
        return NULL;
    char *pBegin = pData;
    char *pEnd = pData;
    while(*pEnd != '\0')
        pEnd ++;
    pEnd--;
    // Reverse the whole sentence
    Reverse(pBegin, pEnd);
    // Reverse every word in the sentence
    pBegin = pEnd = pData;
    while(*pBegin != '\0')
    {
        if(*pBegin == ' ')
        {
            pBegin ++;
            pEnd ++;
        }
        else if(*pEnd == ' ' || *pEnd == '\0')
        {
            Reverse(pBegin, --pEnd);
            pBegin = ++pEnd;
        }
        else
        {
            pEnd ++;
        }
    }
    return pData;
}
// ==================== Test Code ====================
void Test(char* testName, char* input, char* expectedResult)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    ReverseSentence(input);
    if((input == NULL && expectedResult == NULL)
        || (input != NULL && strcmp(input, expectedResult) == 0))
        printf("Passed.\n\n");
    else
        printf("Failed.\n\n");
}
// Multiple words
void Test1()
{
    char input[] = "I am a student.";
    char expected[] = "student. a am I";
    Test("Test1", input, expected);
}
// Only one word
void Test2()
{
    char input[] = "Wonderful";
    char expected[] = "Wonderful";
    Test("Test2", input, expected);
}
// Null pointer
void Test3()
{
    Test("Test3", NULL, NULL);
}
// empty string
void Test4()
{
    Test("Test4", "", "");
}
// Only blanks
void Test5()
{
    char input[] = "   ";
    char expected[] = "   ";
    Test("Test5", input, expected);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    return 0;
}
```

## 092. LeftRotateString


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <string.h>
void Reverse(char *pBegin, char *pEnd) 
{
    if(pBegin == NULL || pEnd == NULL)
        return;
    while(pBegin < pEnd) 
    {
        char temp = *pBegin;
        *pBegin = *pEnd;
        *pEnd = temp;
        pBegin ++, pEnd --;
    }
}	
char* LeftRotateString(char* pStr, int n)
{
    if(pStr != NULL)
    {
        int nLength = static_cast<int>(strlen(pStr));
        if(nLength > 0 && n > 0 && n < nLength)
        {
            char* pFirstStart = pStr;
            char* pFirstEnd = pStr + n - 1;
            char* pSecondStart = pStr + n;
            char* pSecondEnd = pStr + nLength - 1;
            // Reverse the n leading characters
            Reverse(pFirstStart, pFirstEnd);
            // Reverse other characters
            Reverse(pSecondStart, pSecondEnd);
            // Reverse the whole string
            Reverse(pFirstStart, pSecondEnd);
        }
    }
    return pStr;
}
// ==================== Test Code ====================
void Test(char* testName, char* input, int num, char* expectedResult)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    char* result = LeftRotateString(input, num);
    if((input == NULL && expectedResult == NULL)
        || (input != NULL && strcmp(result, expectedResult) == 0))
        printf("Passed.\n\n");
    else
        printf("Failed.\n\n");
}
void Test1()
{
    char input[] = "abcdefg";
    char expected[] = "cdefgab";
    Test("Test1", input, 2, expected);
}
void Test2()
{
    char input[] = "abcdefg";
    char expected[] = "bcdefga";
    Test("Test2", input, 1, expected);
}
void Test3()
{
    char input[] = "abcdefg";
    char expected[] = "gabcdef";
    Test("Test3", input, 6, expected);
}
void Test4()
{
    Test("Test4", NULL, 6, NULL);
}
void Test5()
{
    char input[] = "abcdefg";
    char expected[] = "abcdefg";
    Test("Test5", input, 0, expected);
}
void Test6()
{
    char input[] = "abcdefg";
    char expected[] = "abcdefg";
    Test("Test6", input, 7, expected);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    return 0;
}
```

## 093. MaxInSlidingWindows


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <vector>
#include <deque>
using namespace std;
vector<int> maxInWindows(const vector<int>& numbers, int windowSize)
{
    vector<int> maxInSlidingWindows;
    if(numbers.size() >= windowSize && windowSize >= 1)
    {
        deque<int> indices;
        for(int i = 0; i < windowSize; ++i)
        {
            while(!indices.empty() && numbers[i] >= numbers[indices.back()])
                indices.pop_back();
            indices.push_back(i);
        }
        for(int i = windowSize; i < numbers.size(); ++i)
        {
            maxInSlidingWindows.push_back(numbers[indices.front()]);
            while(!indices.empty() && numbers[i] >= numbers[indices.back()])
                indices.pop_back();
            if(!indices.empty() && indices.front() <= i - windowSize)
                indices.pop_front();
            indices.push_back(i);
        }
        maxInSlidingWindows.push_back(numbers[indices.front()]);
    }
    return maxInSlidingWindows;
}
// ==================== Test Code ====================
void Test(char* testName, const vector<int>& numbers, int windowSize, const vector<int>& expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    vector<int> result = maxInWindows(numbers, windowSize);
    vector<int>::const_iterator iterResult = result.begin();
    vector<int>::const_iterator iterExpected = expected.begin();
    while(iterResult < result.end() && iterExpected < expected.end())
    {
        if(*iterResult != *iterExpected)
            break;
        ++iterResult;
        ++iterExpected;
    }
    if(iterResult == result.end() && iterExpected == expected.end())
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
void Test1()
{
    int numbers[] = {2, 3, 4, 2, 6, 2, 5, 1};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    int expected[] = {4, 4, 6, 6, 6, 5};
    vector<int> vecExpected(expected, expected + sizeof(expected) / sizeof(int));
    int windowSize = 3;
    Test("Test1", vecNumbers, windowSize, vecExpected);
}
void Test2()
{
    int numbers[] = {1, 3, -1, -3, 5, 3, 6, 7};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    int expected[] = {3, 3, 5, 5, 6, 7};
    vector<int> vecExpected(expected, expected + sizeof(expected) / sizeof(int));
    int windowSize = 3;
    Test("Test2", vecNumbers, windowSize, vecExpected);
}
// increasingly sorted
void Test3()
{
    int numbers[] = {1, 3, 5, 7, 9, 11, 13, 15};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    int expected[] = {7, 9, 11, 13, 15};
    vector<int> vecExpected(expected, expected + sizeof(expected) / sizeof(int));
    int windowSize = 4;
    Test("Test3", vecNumbers, windowSize, vecExpected);
}
// decreasingly sorted
void Test4()
{
    int numbers[] = {16, 14, 12, 10, 8, 6, 4};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    int expected[] = {16, 14, 12};
    vector<int> vecExpected(expected, expected + sizeof(expected) / sizeof(int));
    int windowSize = 5;
    Test("Test4", vecNumbers, windowSize, vecExpected);
}
// size of sliding windows is 1
void Test5()
{
    int numbers[] = {10, 14, 12, 11};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    int expected[] = {10, 14, 12, 11};
    vector<int> vecExpected(expected, expected + sizeof(expected) / sizeof(int));
    int windowSize = 1;
    Test("Test5", vecNumbers, windowSize, vecExpected);
}
// size of sliding windows is same as the array length
void Test6()
{
    int numbers[] = {10, 14, 12, 11};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    int expected[] = {14};
    vector<int> vecExpected(expected, expected + sizeof(expected) / sizeof(int));
    int windowSize = 4;
    Test("Test6", vecNumbers, windowSize, vecExpected);
}
// size of sliding windows is 0
void Test7()
{
    int numbers[] = {10, 14, 12, 11};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    vector<int> vecExpected;
    int windowSize = 0;
    Test("Test7", vecNumbers, windowSize, vecExpected);
}
// size of sliding windows is greater than the array length
void Test8()
{
    int numbers[] = {10, 14, 12, 11};
    vector<int> vecNumbers(numbers, numbers + sizeof(numbers) / sizeof(int));
    vector<int> vecExpected;
    int windowSize = 5;
    Test("Test8", vecNumbers, windowSize, vecExpected);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    Test7();
    Test8();
	return 0;
}
```

## 094. QueueWithMax


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <deque>
#include <exception>
using namespace std;
template<typename T> class QueueWithMax
{
public:
    QueueWithMax(): currentIndex(0)
    {
    }
    void push_back(T number)
    {
        while(!maximums.empty() && number >= maximums.back().number)
            maximums.pop_back();
        InternalData internalData = {number, currentIndex};
        data.push_back(internalData);
        maximums.push_back(internalData);
        ++currentIndex;
    }
    void pop_front()
    {
        if(maximums.empty())
            throw new exception("queue is empty");
        if(maximums.front().index == data.front().index)
            maximums.pop_front();
        data.pop_front();
    }
    T max() const
    {
        if(maximums.empty())
            throw new exception("queue is empty");
        return maximums.front().number;
    }
private:
    struct InternalData
    {
        T number;
        int index;
    };
    deque<InternalData> data;
    deque<InternalData> maximums;
    int currentIndex;
};
// ==================== Test Code ====================
void Test(char* testName, const QueueWithMax<int>& queue, int expected)
{
    if(testName != NULL)
        printf("%s begins: ", testName);
    if(queue.max() == expected)
        printf("Passed.\n");
    else
        printf("FAILED.\n");
}
int main(int argc, char* argv[])
{
    // 2, 3, 4, 2, 6, 2, 5, 1
    QueueWithMax<int> queue;
    // {2}
    queue.push_back(2);
    Test("Test1", queue, 2);
    // {2, 3}
    queue.push_back(3);
    Test("Test2", queue, 3);
    // {2, 3, 4}
    queue.push_back(4);
    Test("Test3", queue, 4);
    // {2, 3, 4, 2}
    queue.push_back(2);
    Test("Test4", queue, 4);
    // {3, 4, 2}
    queue.pop_front();
    Test("Test5", queue, 4);
    // {4, 2}
    queue.pop_front();
    Test("Test6", queue, 4);
    // {2}
    queue.pop_front();
    Test("Test7", queue, 2);
    // {2, 6}
    queue.push_back(6);
    Test("Test8", queue, 6);
    // {2, 6, 2}
    queue.push_back(2);
    Test("Test9", queue, 6);
    // {2, 6, 2, 5}
    queue.push_back(5);
    Test("Test9", queue, 6);
    // {6, 2, 5}
    queue.pop_front();
    Test("Test10", queue, 6);
    // {2, 5}
    queue.pop_front();
    Test("Test11", queue, 5);
    // {5}
    queue.pop_front();
    Test("Test12", queue, 5);
    // {5, 1}
    queue.push_back(1);
    Test("Test13", queue, 5);
	return 0;
}
```

## 095. DicesProbability


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <math.h>
void PrintProbability(int number)
{
    if(number < 1)
        return;
    const int maxValue = 6;
    int* pProbabilities[2];
    pProbabilities[0] = new int[maxValue * number + 1];
    pProbabilities[1] = new int[maxValue * number + 1];
    for(int i = 0; i < maxValue * number + 1; ++i)
    {
        pProbabilities[0][i] = 0;
        pProbabilities[1][i] = 0;
    }
 
    int flag = 0;
    for (int i = 1; i <= maxValue; ++i) 
        pProbabilities[flag][i] = 1; 
    
    for (int k = 2; k <= number; ++k) 
    {
        for(int i = 0; i < k; ++i)
            pProbabilities[1 - flag][i] = 0;
        for (int i = k; i <= maxValue * k; ++i) 
        {
            pProbabilities[1 - flag][i] = 0;
            for(int j = 1; j <= i && j <= maxValue; ++j) 
                pProbabilities[1 - flag][i] += pProbabilities[flag][i - j];
        }
 
        flag = 1 - flag;
    }
 
    double total = pow((double)maxValue, number);
    for(int i = number; i <= maxValue * number; ++i)
    {
        double ratio = (double)pProbabilities[flag][i] / total;
        printf("%d: %e\n", i, ratio);
    }
 
    delete[] pProbabilities[0];
    delete[] pProbabilities[1];
}
// ==================== Test Code ====================
void Test(int n)
{
    printf("Test for %d begins:\n", n);
    PrintProbability(n);
    printf("\n");
}
int main(int argc, char* argv[])
{
    Test(1);
    Test(2);
    Test(3);
    Test(4);
    
    Test(11);
    Test(0);
    return 0;
}
```

## 096. LastNumberInCircle


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <list>
using namespace std;
// ==================== Solution 1====================
int LastRemaining_Solution1(unsigned int n, unsigned int m)
{
    if(n < 1 || m < 1)
        return -1;
    list<int> numbers;
    for(unsigned int i = 0; i < n; ++ i)
        numbers.push_back(i);
    list<int>::iterator current = numbers.begin();
    while(numbers.size() > 1)
    {
        for(int i = 1; i < m; ++ i)
        {
            current ++;
            if(current == numbers.end())
                current = numbers.begin();
        }
        list<int>::iterator next = ++ current;
        if(next == numbers.end())
            next = numbers.begin();
        -- current;
        numbers.erase(current);
        current = next;
    }
    return *(current);
}
// ==================== Solution 2 ====================
int LastRemaining_Solution2(unsigned int n, unsigned int m)
{
    if(n < 1 || m < 1)
        return -1;
    int last = 0;
    for (int i = 2; i <= n; i ++) 
        last = (last + m) % i;
    return last;
}
// ==================== Test Code ====================
void Test(char* testName, unsigned int n, unsigned int m, int expected)
{
    if(testName != NULL)
        printf("%s begins: \n", testName);
    if(LastRemaining_Solution1(n, m) == expected)
        printf("Solution1 passed.\n");
    else
        printf("Solution1 failed.\n");
    if(LastRemaining_Solution2(n, m) == expected)
        printf("Solution2 passed.\n");
    else
        printf("Solution2 failed.\n");
    printf("\n");
}
void Test1()
{
    Test("Test1", 5, 3, 3);
}
void Test2()
{
    Test("Test2", 5, 2, 2);
}
void Test3()
{
    Test("Test3", 6, 7, 4);
}
void Test4()
{
    Test("Test4", 6, 6, 3);
}
void Test5()
{
    Test("Test5", 0, 0, -1);
}
void Test6()
{
    Test("Test6", 4000, 997, 1027);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    return 0;
}
```

## 097. MinimalMoves


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class MinimalMoves {
    // ==================== Solution 1 ====================
    public static int minMoveCount_solution1(int[] seq) {
        return seq.length - longestIncreasingLength_solution1(seq);
    }
    
    private static int longestIncreasingLength_solution1(int[] seq){
        int len = seq.length;
        int[] lookup = new int[len];
        
        for(int i = 0; i < len; ++i) {
            int longestSoFar = 0;
            for(int j= 0; j < i; ++j) {
                if(seq[i] > seq[j] && lookup[j] > longestSoFar)
                    longestSoFar = lookup[j];
            }
            lookup[i] = longestSoFar + 1;
        }
        
        int longestLength = 0;
        for(int i = 0; i < len; ++i) {
            if(lookup[i] > longestLength)
                longestLength = lookup[i];
        }
        
        return longestLength;
    }
    
    // ==================== Solution 2 ====================
    public static int minMoveCount_solution2(int[] seq) {
        return seq.length - longestIncreasingLength_solution2(seq);
    }
    
    private static int longestIncreasingLength_solution2(int[] seq){
        int len = seq.length;
        int[] lookup = new int[len];
        
        lookup[0] = seq[0];
        int longestLength = 1;
        for(int i = 1; i < len; ++i) {
            if(seq[i] > lookup[longestLength - 1]) {
                longestLength++;
                lookup[longestLength - 1] = seq[i];
            }
            else {
                int low = 0;
                int high = longestLength - 1;
                while(low != high) {
                    int mid = (low + high) / 2;
                    if(lookup[mid] < seq[i]) {
                        low = mid + 1;
                    }
                    else {
                        high = mid;
                    }
                }
                
                lookup[low] = seq[i];
            }
        }
        return longestLength;
    }
    
    // ==================== Test Code ====================
    private static void test(String testName, int cards[], int expected){
        System.out.print(testName + " begins: ");
        if(minMoveCount_solution1(cards) == expected)
            System.out.print("Passed; ");
        else
            System.out.print("FAILED; ");
        
        if(minMoveCount_solution2(cards) == expected)
            System.out.println("Passed.");
        else
            System.out.println("FAILED.");
    }
    
    private static void test1(){
        int cards[] = {2, 4, 1, 5, 6, 3};
        test("Test1", cards, 2);
    }
    
    private static void test2(){
        int cards[] = {1, 2, 5, 3, 4, 7, 6};
        test("Test2", cards, 2);
    }
    
    private static void test3(){
        int cards[] = {7, 2, 3, 1, 4, 5, 8, 9, 6};
        test("Test3", cards, 3);
    }
    
    private static void test4(){
        int cards[] = {8, 4, 12, 2, 10, 6, 14, 1, 9, 5, 13, 3, 11, 7};
        test("Test4", cards, 10);
    }
    
    private static void test5(){
        int cards[] = {1, 2, 3, 4, 5, 6, 7};
        test("Test5", cards, 0);
    }
    
    private static void test6(){
        int cards[] = {7, 6, 5, 4, 3, 2, 1};
        test("Test6", cards, 6);
    }
    
    private static void test7(){
        int cards[] = {7};
        test("Test7", cards, 0);
    }
    
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
        test7();
    }
}
```

## 098. MaximalProfitBuyingSellingStock


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
// ==================== Solution 1 ====================
int MaxDiffCore(int* start, int* end, int* max, int* min);
int MaxDiff_Solution1(int numbers[], unsigned length)
{
    if(numbers == NULL && length < 2)
        return 0;
    int max, min;
    return MaxDiffCore(numbers, numbers + length - 1, &max, &min);
}
int MaxDiffCore(int* start, int* end, int* max, int* min)
{
    if(end == start)
    {
        *max = *min = *start;
        return 0x80000000;
    }
    int* middle = start + (end - start) / 2;
    int maxLeft, minLeft;
    int leftDiff = MaxDiffCore(start, middle, &maxLeft, &minLeft);
    int maxRight, minRight;
    int rightDiff = MaxDiffCore(middle + 1, end, &maxRight, &minRight);
    int crossDiff = maxRight - minLeft;
    *max = (maxLeft > maxRight) ? maxLeft : maxRight;
    *min = (minLeft < minRight) ? minLeft : minRight;
    int maxDiff = (leftDiff > rightDiff) ? leftDiff : rightDiff;
    maxDiff = (maxDiff > crossDiff) ? maxDiff : crossDiff;
    return maxDiff;
}
// ==================== Solution 2 ====================
int MaxDiff_Solution2(int numbers[], unsigned length)
{
    if(numbers == NULL && length < 2)
        return 0;
    int min = numbers[0];
    int maxDiff =  numbers[1] - min;
    for(int i = 2; i < length; ++i)
    {
        if(numbers[i - 1] < min)
            min = numbers[i - 1];
        int currentDiff = numbers[i] - min;
        if(currentDiff > maxDiff)
            maxDiff = currentDiff;
    }
    return maxDiff;
}
// ==================== Test Code ====================
void Test(char* testName, int* numbers, unsigned int length, int expected)
{
    if(testName != NULL)
        printf("====%s begins: ==== \n", testName);
    if(MaxDiff_Solution1(numbers, length) == expected)
        printf("Solution 1 Passed.\n");
    else
        printf("Solution 1 FAILED.\n");
    if(MaxDiff_Solution2(numbers, length) == expected)
        printf("Solution 2 Passed.\n");
    else
        printf("Solution 2 FAILED.\n");
}
void Test1()
{
    int numbers[] = {4, 1, 3, 2, 5};
    Test("Test1", numbers, sizeof(numbers) / sizeof(int), 4);
}
void Test2()
{
    int numbers[] = {1, 2, 4, 7, 11, 16};
    Test("Test2", numbers, sizeof(numbers) / sizeof(int), 15);
}
void Test3()
{
    int numbers[] = {16, 11, 7, 4, 2, 1};
    Test("Test3", numbers, sizeof(numbers) / sizeof(int), -1);
}
void Test4()
{
    int numbers[] = {9, 11, 5, 7, 16, 1, 4, 2};
    Test("Test4", numbers, sizeof(numbers) / sizeof(int), 11);
}
void Test5()
{
    int numbers[] = {2, 4};
    Test("Test5", numbers, sizeof(numbers) / sizeof(int), 2);
}
void Test6()
{
    int numbers[] = {4, 2};
    Test("Test6", numbers, sizeof(numbers) / sizeof(int), -2);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
	return 0;
}
```

## 099. Accumulate


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
// ==================== Solution 1 ====================
class Temp
{
public:
    Temp() { ++ N; Sum += N; }
    static void Reset() { N = 0; Sum = 0; }
    static unsigned int GetSum() { return Sum; }
private:
    static unsigned int N;
    static unsigned int Sum;
};
unsigned int Temp::N = 0;
unsigned int Temp::Sum = 0;
unsigned int Sum_Solution1(unsigned int n)
{
    Temp::Reset();
    Temp *a = new Temp[n];
    delete []a;
    a = NULL;
    return Temp::GetSum();
}
// ==================== Solution 2 ====================
class A;
A* Array[2];
class A
{
public:
    virtual unsigned int Sum (unsigned int n) 
    { 
        return 0; 
    }
};
class B: public A
{
public:
    virtual unsigned int Sum (unsigned int n) 
    { 
        return Array[!!n]->Sum(n-1) + n; 
    }
};
int Sum_Solution2(int n)
{
    A a;
    B b;
    Array[0] = &a;
    Array[1] = &b;
    int value = Array[1]->Sum(n);
    return value;
}
// ==================== Solution 3 ====================
typedef unsigned int (*fun)(unsigned int);
unsigned int Solution3_Teminator(unsigned int n) 
{
    return 0;
}
unsigned int Sum_Solution3(unsigned int n)
{
    static fun f[2] = {Solution3_Teminator, Sum_Solution3}; 
    return n + f[!!n](n - 1);
}
// ==================== Solution 4 ====================
template <unsigned int n> struct Sum_Solution4
{
    enum Value { N = Sum_Solution4<n - 1>::N + n};
};
template <> struct Sum_Solution4<1>
{
    enum Value { N = 1};
};
template <> struct Sum_Solution4<0>
{
    enum Value { N = 0};
};
// ==================== Test Code ====================
void Test(int n, int expected)
{
    printf("Test for %d begins:\n", n);
    if(Sum_Solution1(n) == expected)
        printf("Solution1 passed.\n");
    else
        printf("Solution1 failed.\n");
    if(Sum_Solution2(n) == expected)
        printf("Solution2 passed.\n");
    else
        printf("Solution2 failed.\n");
    if(Sum_Solution3(n) == expected)
        printf("Solution3 passed.\n");
    else
        printf("Solution3 failed.\n");
}
void Test1()
{
    const unsigned int number = 1;
    int expected = 1;
    Test(number, expected);
    if(Sum_Solution4<number>::N == expected)
        printf("Solution4 passed.\n");
    else
        printf("Solution4 failed.\n");
}
void Test2()
{
    const unsigned int number = 5;
    int expected = 15;
    Test(number, expected);
    if(Sum_Solution4<number>::N == expected)
        printf("Solution4 passed.\n");
    else
        printf("Solution4 failed.\n");
}
void Test3()
{
    const unsigned int number = 10;
    int expected = 55;
    Test(number, expected);
    if(Sum_Solution4<number>::N == expected)
        printf("Solution4 passed.\n");
    else
        printf("Solution4 failed.\n");
}
void Test4()
{
    const unsigned int number = 0;
    int expected = 0;
    Test(number, expected);
    if(Sum_Solution4<number>::N == expected)
        printf("Solution4 passed.\n");
    else
        printf("Solution4 failed.\n");
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    return 0;
}
```

## 100. 103. ArithmeticOperations


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class ArithmeticOperations {
    public static int add(int num1, int num2) {
        int sum, carry;
        do {
            sum = num1 ^ num2;
            carry = (num1 & num2) << 1;
            num1 = sum;
            num2 = carry;
        } while (num2 != 0);
        return num1;
    }
    public static int subtract(int num1, int num2) {
        num2 = add(~num2, 1);
        return add(num1, num2);
    }
    public static int multiply(int num1, int num2) {
        boolean minus = false;
        if ((num1 < 0 && num2 > 0) || (num1 > 0 && num2 < 0))
            minus = true;
        if (num1 < 0)
            num1 = add(~num1, 1);
        if (num2 < 0)
            num2 = add(~num2, 1);
        int result = 0;
        while (num1 > 0) {
            if ((num1 & 0x1) != 0) {
                result = add(result, num2);
            }
            num2 = num2 << 1;
            num1 = num1 >> 1;
        }
        if (minus)
            result = add(~result, 1);
        return result;
    }
    public static int divide(int num1, int num2) {
        if (num2 == 0)
            throw new ArithmeticException("num2 is zero.");
        boolean minus = false;
        if ((num1 < 0 && num2 > 0) || (num1 > 0 && num2 < 0))
            minus = true;
        if (num1 < 0)
            num1 = add(~num1, 1);
        if (num2 < 0)
            num2 = add(~num2, 1);
        int result = 0;
        for (int i = 0; i < 32; i=add(i, 1)) {
            result = result << 1;
            if ((num1 >> (31 - i)) >= num2) {
                num1 = subtract(num1, num2 << (31 - i));
                result = add(result, 1);
            }
        }
        if (minus)
            result = add(~result, 1);
        return result;
    }
    public static void test(String testName, int num1, int num2, int res1,
            int res2, int res3, int res4) {
        System.out.print(testName + " begins: ");
        if (add(num1, num2) == res1)
            System.out.print("add passed, ");
        else
            System.out.print("add FAILED, ");
        if (subtract(num1, num2) == res2)
            System.out.print("subtract passed, ");
        else
            System.out.print("subtract FAILED, ");
        if (multiply(num1, num2) == res3)
            System.out.print("multiply passed, ");
        else
            System.out.print("multiply FAILED, ");
        try {
            int result = divide(num1, num2);
            if (num2 != 0 && result == res4)
                System.out.print("divide passed.");
            else
                System.out.print("divide FAILED.");
        } catch (ArithmeticException e) {
            if (num2 == 0)
                System.out.print("divide passed.");
            else
                System.out.print("divide FAILED.");
        }
        System.out.print("\n");
    }
    public static void main(String[] args) {
        test("Test1", 3, 2, 5, 1, 6, 1);
        test("Test2", -3, 2, -1, -5, -6, -1);
        test("Test3", -3, -2, -5, -1, 6, 1);
        test("Test4", -3, 0, -3, -3, 0, 0);
        test("Test5", 100, -5, 95, 105, -500, -20);
        test("Test6", 98, 5, 103, 93, 490, 19);
        test("Test7", 6, 12, 18, -6, 72, 0);
    }
}
```

## 104. SealedClass


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
// ==================== Solution 1 ====================
class SealedClass1
{
public:
    static SealedClass1* GetInstance() 
    {
        return new SealedClass1();
    }
 
    static void DeleteInstance( SealedClass1* pInstance)
    {
        delete pInstance;
    }
 
private:
    SealedClass1() {}
    ~SealedClass1() {}
};
// It will cause compiling errors when we try to
// define new classes deriving from SealedClass
/*
class Try1 : public SealedClass1
{
public:
    Try1() {}
    ~Try1() {}
};
*/
// ==================== Solution 2 ====================
// NOTE: It only works in Visual Studio, but does not
// work in G++
template <typename T> class MakeSealed
{
    friend T;
 
private:
    MakeSealed() {}
    ~MakeSealed() {}
};
 
class SealedClass2 : virtual public MakeSealed<SealedClass2>
{
public:
    SealedClass2() {}
    ~SealedClass2() {}
};
// It will cause compiling errors when we try to
// define new classes deriving from SealedClass
/*
class Try2 : public SealedClass2
{
public:
    Try2() {}
    ~Try2() {}
};
*/
int main(int argc, char* argv[])
{
    return 0;
}
```

## 105. ConstuctArray


```java

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
public class ConstuctArray {
    public static void multiply(double array1[], double array2[]){
        if(array1.length == array2.length && array1.length > 0){
            array2[0] = 1;
            for(int i = 1; i < array1.length; ++i){
                array2[i] = array2[i - 1] * array1[i - 1];
            }
            
            int temp = 1;
            for(int i = array1.length - 2; i >= 0; --i){
                temp *= array1[i + 1];
                array2[i] *= temp;
            }
        }
    }
    
    //================= Test Code =================
    private static boolean equalArrays(double array1[], double array2[]){
        if(array1.length != array2.length)
            return false;
        
        for(int i = 0; i < array1.length; ++i){
            if(abs(array1[i] - array2[i]) > 0.0000001)
                return false;
        }
        
        return true;
    }
    
    private static double abs(double d) {
        return (d > 0) ? d : -d;
    }
    private static void test(String testName, double array1[], double array2[], double expected[]){
        System.out.print(testName + " Begins: ");
        
        multiply(array1, array2);
        if(equalArrays(array2, expected))
            System.out.println("Passed.");
        else
            System.out.println("FAILED.");
    }
    
    private static void test1(){
        double array1[] = {1, 2, 3, 4, 5};
        double array2[] = new double[5];
        double expected[] = {120, 60, 40, 30, 24};
        
        test("Test1", array1, array2, expected);
    }
    
    private static void test2(){
        double array1[] = {1, 2, 0, 4, 5};
        double array2[] = new double[5];
        double expected[] = {0, 0, 40, 0, 0};
        
        test("Test2", array1, array2, expected);
    }
    
    private static void test3(){
        double array1[] = {1, 2, 0, 4, 0};
        double array2[] = new double[5];
        double expected[] = {0, 0, 0, 0, 0};
        
        test("Test3", array1, array2, expected);
    }
    
    private static void test4(){
        double array1[] = {1, -2, 3, -4, 5};
        double array2[] = new double[5];
        double expected[] = {120, -60, 40, -30, 24};
        
        test("Test4", array1, array2, expected);
    }
    
    public static void main(String[] args){
        test1();
        test2();
        test3();
        test4();
    }
}
```

## 106. StringToInt


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <stdlib.h>
long long StrToIntCore(const char* str, bool minus);
enum Status {kValid = 0, kInvalid};
int g_nStatus = kValid;
int StrToInt(const char* str)
{
    g_nStatus = kInvalid;
    long long num = 0;
    if(str != NULL && *str != '\0') 
    {
        bool minus = false;
        if(*str == '+')
            str ++;
        else if(*str == '-') 
        {
            str ++;
            minus = true;
        }
        if(*str != '\0') 
        {
            num = StrToIntCore(str, minus);
        }
    }
    return (int)num;
}
long long StrToIntCore(const char* digit, bool minus)
{
    long long num = 0;
    while(*digit != '\0') 
    {
        if(*digit >= '0' && *digit <= '9') 
        {
            int flag = minus ? -1 : 1;
            num = num * 10 + flag * (*digit - '0');
            if((!minus && num > 0x7FFFFFFF) 
                || (minus && num < (signed int)0x80000000))
            {
                num = 0;
                break;
            }
            digit++;
        }
        else 
        {
            num = 0;
            break;
        }
    }
    if(*digit == '\0') 
    {
        g_nStatus = kValid;
    }
    return num;
}
// ==================== Test Code ====================
void Test(char* string)
{
    int result = StrToInt(string);
    if(result == 0 && g_nStatus == kInvalid)
        printf("the input %s is invalid.\n", string);
    else
        printf("number for %s is: %d.\n", string, result);
}
int main(int argc, char* argv[])
{
    Test(NULL);
    Test("");
    Test("123");
    Test("+123");
    
    Test("-123");
    Test("1a33");
    Test("+0");
    Test("-0");
    //Maximal positive integer, 0x7FFFFFFF
    Test("+2147483647");    
    Test("-2147483647");
    Test("+2147483648");
    //Minimal negative integer, 0x80000000
    Test("-2147483648");    
    Test("+2147483649");
    Test("-2147483649");
    Test("+");
    Test("-");
    return 0;
}
```

## 107. LowestAncestorInTrees


```cpp

// Coding Interviews: Questions, Analysis & Solutions
// Harry He
#include <stdio.h>
#include <list>
#include <vector>
using namespace std;
struct TreeNode 
{
    int                       m_nValue;    
    std::vector<TreeNode*>    m_vChildren;    
};
bool GetNodePath(TreeNode* pRoot, TreeNode* pNode, list<TreeNode*>& path)
{
    if(pRoot == pNode)
        return true;
 
    path.push_back(pRoot);
 
    bool found = false;
    vector<TreeNode*>::iterator i = pRoot->m_vChildren.begin();
    while(!found && i < pRoot->m_vChildren.end())
    {
        found = GetNodePath(*i, pNode, path);
        ++i;
    }
 
    if(!found)
        path.pop_back();
 
    return found;
}
TreeNode* GetLastCommonNode
(
    const list<TreeNode*>& path1, 
    const list<TreeNode*>& path2
)
{
    list<TreeNode*>::const_iterator iterator1 = path1.begin();
    list<TreeNode*>::const_iterator iterator2 = path2.begin();
    
    TreeNode* pLast = NULL;
 
    while(iterator1 != path1.end() && iterator2 != path2.end())
    {
        if(*iterator1 == *iterator2)
            pLast = *iterator1;
 
        iterator1++;
        iterator2++;
    }
 
    return pLast;
}
TreeNode* GetLastCommonParent(TreeNode* pRoot, TreeNode* pNode1, TreeNode* pNode2)
{
    if(pRoot == NULL || pNode1 == NULL || pNode2 == NULL)
        return NULL;
 
    list<TreeNode*> path1;
    GetNodePath(pRoot, pNode1, path1);
 
    list<TreeNode*> path2;
    GetNodePath(pRoot, pNode2, path2);
 
    return GetLastCommonNode(path1, path2);
}
// ==================== Code for Trees ====================
TreeNode* CreateTreeNode(int value)
{
    TreeNode* pNode = new TreeNode();
    pNode->m_nValue = value;
    return pNode;
}
void ConnectTreeNodes(TreeNode* pParent, TreeNode* pChild)
{
    if(pParent != NULL)
    {
        pParent->m_vChildren.push_back(pChild);
    }
}
void PrintTreeNode(TreeNode* pNode)
{
    if(pNode != NULL)
    {
        printf("value of this node is: %d.\n", pNode->m_nValue);
        printf("its children is as the following:\n");
        std::vector<TreeNode*>::iterator i = pNode->m_vChildren.begin();
        while(i < pNode->m_vChildren.end())
        {
            if(*i != NULL)
                printf("%d\t", (*i)->m_nValue);
        }
        printf("\n");
    }
    else
    {
        printf("this node is null.\n");
    }
    printf("\n");
}
void PrintTree(TreeNode* pRoot)
{
    PrintTreeNode(pRoot);
    if(pRoot != NULL)
    {
        std::vector<TreeNode*>::iterator i = pRoot->m_vChildren.begin();
        while(i < pRoot->m_vChildren.end())
        {
            PrintTree(*i);
            ++i;
        }
    }
}
void DestroyTree(TreeNode* pRoot)
{
    if(pRoot != NULL)
    {
        std::vector<TreeNode*>::iterator i = pRoot->m_vChildren.begin();
        while(i < pRoot->m_vChildren.end())
        {
            DestroyTree(*i);
            ++i;
        }
        delete pRoot;
    }
}
// ==================== Test Code ====================
void Test(char* testName, TreeNode* pRoot, TreeNode* pNode1, TreeNode* pNode2, TreeNode* pExpected)
{
    if(testName != NULL)
        printf("%s begins: \n", testName);
    TreeNode* pResult = GetLastCommonParent(pRoot, pNode1, pNode2);
    if((pExpected == NULL && pResult == NULL) || 
        (pExpected != NULL && pResult != NULL && pResult->m_nValue == pExpected->m_nValue))
        printf("Passed.\n");
    else
        printf("Failed.\n");
}
//              1
//            /   \
//           2     3
//       /       \
//      4         5
//     / \      / |  \
//    6   7    8  9  10
void Test1()
{
    TreeNode* pNode1 = CreateTreeNode(1);
    TreeNode* pNode2 = CreateTreeNode(2);
    TreeNode* pNode3 = CreateTreeNode(3);
    TreeNode* pNode4 = CreateTreeNode(4);
    TreeNode* pNode5 = CreateTreeNode(5);
    TreeNode* pNode6 = CreateTreeNode(6);
    TreeNode* pNode7 = CreateTreeNode(7);
    TreeNode* pNode8 = CreateTreeNode(8);
    TreeNode* pNode9 = CreateTreeNode(9);
    TreeNode* pNode10 = CreateTreeNode(10);
    ConnectTreeNodes(pNode1, pNode2);
    ConnectTreeNodes(pNode1, pNode3);
    ConnectTreeNodes(pNode2, pNode4);
    ConnectTreeNodes(pNode2, pNode5);
    ConnectTreeNodes(pNode4, pNode6);
    ConnectTreeNodes(pNode4, pNode7);
    ConnectTreeNodes(pNode5, pNode8);
    ConnectTreeNodes(pNode5, pNode9);
    ConnectTreeNodes(pNode5, pNode10);
    Test("Test1", pNode1, pNode6, pNode8, pNode2);
    
    DestroyTree(pNode1);
}
//              1
//            /   \
//           2     3
//       /       \
//      4         5
//     / \      / |  \
//    6   7    8  9  10
void Test2()
{
    TreeNode* pNode1 = CreateTreeNode(1);
    TreeNode* pNode2 = CreateTreeNode(2);
    TreeNode* pNode3 = CreateTreeNode(3);
    TreeNode* pNode4 = CreateTreeNode(4);
    TreeNode* pNode5 = CreateTreeNode(5);
    TreeNode* pNode6 = CreateTreeNode(6);
    TreeNode* pNode7 = CreateTreeNode(7);
    TreeNode* pNode8 = CreateTreeNode(8);
    TreeNode* pNode9 = CreateTreeNode(9);
    TreeNode* pNode10 = CreateTreeNode(10);
    ConnectTreeNodes(pNode1, pNode2);
    ConnectTreeNodes(pNode1, pNode3);
    ConnectTreeNodes(pNode2, pNode4);
    ConnectTreeNodes(pNode2, pNode5);
    ConnectTreeNodes(pNode4, pNode6);
    ConnectTreeNodes(pNode4, pNode7);
    ConnectTreeNodes(pNode5, pNode8);
    ConnectTreeNodes(pNode5, pNode9);
    ConnectTreeNodes(pNode5, pNode10);
    Test("Test2", pNode1, pNode7, pNode3, pNode1);
    
    DestroyTree(pNode1);
}
//               1
//              /
//             2
//            /
//           3
//          /
//         4
//        /
//       5
void Test3()
{
    TreeNode* pNode1 = CreateTreeNode(1);
    TreeNode* pNode2 = CreateTreeNode(2);
    TreeNode* pNode3 = CreateTreeNode(3);
    TreeNode* pNode4 = CreateTreeNode(4);
    TreeNode* pNode5 = CreateTreeNode(5);
    ConnectTreeNodes(pNode1, pNode2);
    ConnectTreeNodes(pNode2, pNode3);
    ConnectTreeNodes(pNode3, pNode4);
    ConnectTreeNodes(pNode4, pNode5);
    Test("Test3", pNode1, pNode5, pNode4, pNode3);
    DestroyTree(pNode1);
}
//               1
//              /
//             2
//            /
//           3
//          /
//         4
//        /
//       5
void Test4()
{
    TreeNode* pNode1 = CreateTreeNode(1);
    TreeNode* pNode2 = CreateTreeNode(2);
    TreeNode* pNode3 = CreateTreeNode(3);
    TreeNode* pNode4 = CreateTreeNode(4);
    TreeNode* pNode5 = CreateTreeNode(5);
    ConnectTreeNodes(pNode1, pNode2);
    ConnectTreeNodes(pNode2, pNode3);
    ConnectTreeNodes(pNode3, pNode4);
    ConnectTreeNodes(pNode4, pNode5);
    // pNode6 is not in the tree
    TreeNode* pNode6 = CreateTreeNode(6);
    Test("Test4", pNode1, pNode5, pNode6, NULL);
    DestroyTree(pNode1);
    DestroyTree(pNode6);
}
// Empty Tree
void Test5()
{
    Test("Test5", NULL, NULL, NULL, NULL);
}
//               1
//              /
//             2
//            /
//           3
//          /
//         4
//        /
//       5
void Test6()
{
    TreeNode* pNode1 = CreateTreeNode(1);
    TreeNode* pNode2 = CreateTreeNode(2);
    TreeNode* pNode3 = CreateTreeNode(3);
    TreeNode* pNode4 = CreateTreeNode(4);
    TreeNode* pNode5 = CreateTreeNode(5);
    ConnectTreeNodes(pNode1, pNode2);
    ConnectTreeNodes(pNode2, pNode3);
    ConnectTreeNodes(pNode3, pNode4);
    ConnectTreeNodes(pNode4, pNode5);
    Test("Test6", pNode1, pNode5, pNode1, NULL);
    DestroyTree(pNode1);
}
int main(int argc, char* argv[])
{
    Test1();
    Test2();
    Test3();
    Test4();
    Test5();
    Test6();
    return 0;
}
```

## BinaryTree


```cpp

#include <stdio.h>
#include "BinaryTree.h"
BinaryTreeNode* CreateBinaryTreeNode(int value)
{
    BinaryTreeNode* pNode = new BinaryTreeNode();
    pNode->m_nValue = value;
    pNode->m_pLeft = NULL;
    pNode->m_pRight = NULL;
    return pNode;
}
void ConnectTreeNodes(BinaryTreeNode* pParent, BinaryTreeNode* pLeft, BinaryTreeNode* pRight)
{
    if(pParent != NULL)
    {
        pParent->m_pLeft = pLeft;
        pParent->m_pRight = pRight;
    }
}
void PrintTreeNode(BinaryTreeNode* pNode)
{
    if(pNode != NULL)
    {
        printf("value of this node is: %d\n", pNode->m_nValue);
        if(pNode->m_pLeft != NULL)
            printf("value of its left child is: %d.\n", pNode->m_pLeft->m_nValue);
        else
            printf("left child is null.\n");
        if(pNode->m_pRight != NULL)
            printf("value of its right child is: %d.\n", pNode->m_pRight->m_nValue);
        else
            printf("right child is null.\n");
    }
    else
    {
        printf("this node is null.\n");
    }
    printf("\n");
}
void PrintTree(BinaryTreeNode* pRoot)
{
    PrintTreeNode(pRoot);
    if(pRoot != NULL)
    {
        if(pRoot->m_pLeft != NULL)
            PrintTree(pRoot->m_pLeft);
        if(pRoot->m_pRight != NULL)
            PrintTree(pRoot->m_pRight);
    }
}
void DestroyTree(BinaryTreeNode* pRoot)
{
    if(pRoot != NULL)
    {
        BinaryTreeNode* pLeft = pRoot->m_pLeft;
        BinaryTreeNode* pRight = pRoot->m_pRight;
        delete pRoot;
        pRoot = NULL;
        DestroyTree(pLeft);
        DestroyTree(pRight);
    }
}
```

## BinaryTree


```h

#pragma once
struct BinaryTreeNode 
{
    int                    m_nValue; 
    BinaryTreeNode*        m_pLeft;  
    BinaryTreeNode*        m_pRight; 
};
__declspec( dllexport ) BinaryTreeNode* CreateBinaryTreeNode(int value);
__declspec( dllexport ) void ConnectTreeNodes(BinaryTreeNode* pParent, BinaryTreeNode* pLeft, BinaryTreeNode* pRight);
__declspec( dllexport ) void PrintTreeNode(BinaryTreeNode* pNode);
__declspec( dllexport ) void PrintTree(BinaryTreeNode* pRoot);
__declspec( dllexport ) void DestroyTree(BinaryTreeNode* pRoot);
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

