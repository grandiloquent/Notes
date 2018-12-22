# C Data Files

- [1. Read a Text File Character by Character](#1-read-a-text-file-character-by-character)
- [2. Handle Errors When File Opening Fails](#2-handle-errors-when-file-opening-fails)
- [3. Write to a Text File in Batch Mode](#3-write-to-a-text-file-in-batch-mode)
- [4. Write to a Text File in Interactive Mode](#4-write-to-a-text-file-in-interactive-mode)
- [5. Read a Text File String by String](#5-read-a-text-file-string-by-string)
- [6. Write to a Text File Character by Character](#6-write-to-a-text-file-character-by-character)
- [7. Write Integers to a Text File](#7-write-integers-to-a-text-file)
- [8. Write Structures to a Text File](#8-write-structures-to-a-text-file)
- [9. Read Integers Stored in a Text File](#9-read-integers-stored-in-a-text-file)
- [10. Read Structures Stored in a Text File](#10-read-structures-stored-in-a-text-file)
- [11. Write Integers to a Binary File](#11-write-integers-to-a-binary-file)
- [12. Write Structures to a Binary File](#12-write-structures-to-a-binary-file)
- [13. Read Integers Written to a Binary File](#13-read-integers-written-to-a-binary-file)
- [14. Read Structures Written to a Binary File](#14-read-structures-written-to-a-binary-file)
- [15. Rename a File](#15-rename-a-file)
- [16. Delete a File](#16-delete-a-file)
- [17. Copy a Text File](#17-copy-a-text-file)
- [18. Copy a Binary File](#18-copy-a-binary-file)
- [19. Write to a File and Then Read from That File](#19-write-to-a-file-and-then-read-from-that-file)
- [20. Position a Text File to a Desired Character](#20-position-a-text-file-to-a-desired-character)
- [21. Read from the Device File Keyboard](#21-read-from-the-device-file-keyboard)
- [22. Write Text to the Device File Monitor](#22-write-text-to-the-device-file-monitor)
- [23. Read Text from the Device File Keyboard and Write It to the Device File Monitor](#23-read-text-from-the-device-file-keyboard-and-write-it-to-the-device-file-monitor)

## 1. Read a Text File Character by Character


```c

#include <stdio.h>
int main(int argc, char *argv[]) {
  int num;
  FILE *fptr;
  fptr = fopen("test.txt", "r");
  num = fgetc(fptr);
  while (num != EOF) {
    putchar(num);
    num = fgetc(fptr);
  }
  fclose(fptr);
  return 0;
}
```

## 2. Handle Errors When File Opening Fails


```c

#include <stdio.h>
int main(int argc, char *argv[]) {
  int num, k = 0;
  FILE *fptr;
  fptr = fopen("test.txt","r");
  if (fptr != NULL) {
    puts("File test.txt is opened successfully");
    puts("Contents of file test.txt");
    num = fgetc(fptr);
    while (!feof(fptr)) {
      putchar(num);
      num = fgetc(fptr);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    else
      puts("File test.txt is closed successfully");
  }
  return 0;
}
```

## 3. Write to a Text File in Batch Mode


```c

#include <stdio.h>
int main(int argc, char *argv[]) {
  int k = 0;
  FILE *fptr;
  fptr = fopen("kolkata.txt", "w");
  if (fptr != NULL) {
    puts("File kolkata.txt is opened successfully.");
    fputs("Kolkata is very big city.\n", fptr);
    fputs("It also very nice city.\n", fptr);
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening-failed");
  return 0;
}
```

## 4. Write to a Text File in Interactive Mode


```c

#include <stdio.h>
#include <string.h>
int main(int argc, char *argv[]) {
  int k = 0, n = 0;
  char filename[40], temp[15], store[80];
  FILE *fptr;
  printf("Enter filename (AAAAAAA.AAA) extension not optional: ");
  scanf("%s", temp);
  strcpy(filename, "\\bin\\");
  strcat(filename, temp);
  fptr = fopen(filename, "w");
  if (fptr != NULL) {
    printf("File %s is opened successfully.\n", filename);
    puts("Enter the lines of text to store in the file.");
    puts("Strike Enter key twice to end the line-entry-session.");
    fflush(stdin);
    gets(store);
    n = strlen(store);
    while (n != 0) {
      fputs(store, fptr);
      fputs("\n", fptr);
      gets(store);
      n = strlen(store);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return 0;
}
```

## 5. Read a Text File String by String


```c

#include <stdio.h>
int main(int argc, char *argv[]) {
  int k = 0;
  char *cptr;
  char store[80];
  FILE *fptr;
  fptr = fopen("test.txt", "r");
  if (fptr != NULL) {
    puts("File test.txt is opened successfully.");
    puts("Contents of this file:");
    cptr = fgets(store, 80, fptr);
    while (cptr != NULL) {
      printf("%s", store);
      cptr = fgets(store, 80, fptr);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("\nFile is closed successfully.");
  } else
    puts("File-opening failed");
  return 0;
}
```

## 6. Write to a Text File Character by Character


```c

#include <stdio.h>
int main(int argc, char *argv[]) {
  int k = 0, n = 0;
  FILE *fptr;
  fptr = fopen("jaipur.txt", "w");
  if (fptr != NULL) {
    puts("File jaipur.txt is opened successfully.");
    puts("Enter text to be written to file. Enter * to");
    puts("terminate the text-entry-session.");
    n = getchar();
    while (n != '*') {
      fputc(n, fptr);
      n = getchar();
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return 0;
}
```

## 7. Write Integers to a Text File


```c

#include <stdio.h>
int main() {
  int i, k = 0;
  FILE *fptr;
  fptr = fopen("numbers.dat", "w");
  if (fptr != NULL) {
    puts("File numbers.dat is opened successfully.");
    for (i = 0; i < 10; i++)
      fprintf(fptr, "%d ", i + 1);
    puts("Data written to file numbers.dat successfully.");
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 8. Write Structures to a Text File


```c

#include <stdio.h>
int main() {
  int k = 0;
  char flag = 'y';
  FILE *fptr;
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
  };
  struct biodata sa;
  fptr = fopen("agents.dat", "w");
  if (fptr != NULL) {
    printf("File agents.dat is opened successfully.\n");
    while (flag == 'y') {
      printf("Enter name, roll no, age, and weight of agent: ");
      scanf("%s %d %d %f", sa.name, &sa.rollno, &sa.age,
            &sa.weight);
      fprintf(fptr, "%s %d %d %.1f", sa.name, sa.rollno, sa.age, sa.weight);
      fflush(stdin);
      printf("Any more records(y/n): ");
      scanf(" %c", &flag);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 9. Read Integers Stored in a Text File


```c

#include <stdio.h>
int main() {
  int m = 0, n, k = 0;
  FILE *fptr;
  fptr = fopen("numbers.dat", "r");
  if (fptr != NULL) {
    puts("File numbers.dat is opened successfully.");
    puts("Contents of file numbers.dat:");
    m = fscanf(fptr, "%d", &n);
    while (m != EOF) {
      printf("%d ", n);
      m = fscanf(fptr, "%d", &n);
    }
    printf("\n");
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```
## 10. Read Structures Stored in a Text File


```c

#include <stdio.h>
int main() {
  int k = 0, m = 0;
  FILE *fptr;
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
  };
  struct biodata sa;
  fptr = fopen("agents.dat", "r");
  if (fptr != NULL) {
    printf("File agents.dat is opened successfully.\n");
    m = fscanf(fptr, "%s %d %d %f", sa.name, &sa.rollno, &sa.age, &sa.weight);
    while (m != EOF) {
      printf("Name: %s, Roll no: %d, Age: %d, Weight: %.1f\n", sa.name,
             sa.rollno, sa.age, sa.weight);
      m = fscanf(fptr, "%s %d %d %f", sa.name, &sa.rollno, &sa.age, &sa.weight);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 11. Write Integers to a Binary File


```c

#include <stdio.h>
int main() {
  int i, k, m, a[20];
  FILE *fptr;
  for (i = 0; i < 20; i++)
    a[i] = 30000 + i;
  fptr = fopen("num.dat", "wb");
  if (fptr != NULL) {
    puts("File num.dat is opened successfully.");
    m = fwrite(a, sizeof(int), 10, fptr);
    if (m == 10)
      puts("Data written to the file successfully.");
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 12. Write Structures to a Binary File


```c

#include <stdio.h>
int main() {
  int k = 0;
  char flag = 'y';
  FILE *fptr;
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
  };
  struct biodata sa;
  fptr = fopen("agents2.dat", "wb");
  if (fptr != NULL) {
    printf("File agents2.dat is opened successfully.\n");
    while (flag == 'y') {
      printf("Enter name, roll no, age, and weight of agent: ");
      scanf("%s %d %d %f", sa.name, &sa.rollno, &sa.age, &sa.weight);
      fwrite(&sa, sizeof(sa), 1, fptr);
      fflush(stdin);
      printf("Any more records(y/n): ");
      scanf(" %c", &flag);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 13. Read Integers Written to a Binary File


```c

#include <stdio.h>
int main() {
  int i, k;
  int a[10];
  FILE *fptr;
  fptr = fopen("num.dat", "rb");
  if (fptr != NULL) {
    puts("File num.dat is opened successfully.");
    fread(a, sizeof(int), 10, fptr);
    puts("Contents of file num.dat:");
    for (i = 0; i < 10; i++)
      printf("%d\n", a[i]);
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 14. Read Structures Written to a Binary File


```c

#include <stdio.h>
int main() {
  int k = 0, m = 0;
  FILE *fptr;
  struct biodata {
    char name[15];
    int rollno;
    int age;
    float weight;
  };
  struct biodata sa;
  fptr = fopen("agents2.dat", "rb");
  if (fptr != NULL) {
    printf("File agents2.dat is opened successfully.\n");
    m = fread(&sa, sizeof(sa), 1, fptr);
    while (m != 0) {
      printf("Name: %s, Roll no: %d, Age: %d, Weight: %.1f\n", sa.name,
             sa.rollno, sa.age, sa.weight);
      m = fread(&sa, sizeof(sa), 1, fptr);
    }
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File is closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 15. Rename a File


```c

#include <stdio.h>
int main() {
  int m;
  m = rename("C:\\Code\\kolkata.txt", "C:\\Code\\city.dat");
  if (m == 0)
    puts("Operation of renaming a file is successful.");
  if (m != 0)
    puts("Operation of renaming a file failed.");
  return (0);
}
```

## 16. Delete a File


```c

#include <stdio.h>
int main() {
  int m;
  m = remove("C:\\Code\\city.dat");
  if (m == 0)
    puts("Operation of deletion of file is successful.");
  if (m != 0)
    puts("Operation of deletion of file failed.");
  return (0);
}
```

## 17. Copy a Text File


```c

#include <stdio.h>
int main() {
  FILE *fptrSource, *fptrTarget;
  int m, n, p;
  fptrSource = fopen("C:\\Code\\satara.txt", "r");
  if (fptrSource == NULL) {
    puts("Source-file-opening failed");
    exit(1);
  }
  puts("Source-file satara.txt opened successfully");
  fptrTarget = fopen("C:\\Code\\town.dat", "w");
  if (fptrTarget == NULL) {
    puts("Target-file-opening failed");
    exit(2);
  }
  puts("Target-file town.dat opened successfully");
  m = fgetc(fptrSource);
  while (m != EOF) {
    fputc(m, fptrTarget);
    m = fgetc(fptrSource);
  }
  puts("File copied successfully");
  n = fclose(fptrSource);
  if (n == -1)
    puts("Source-file-closing failed");
  if (n == 0)
    puts("Source-file closed successfully");
  p = fclose(fptrTarget);
  if (p == -1)
    puts("Target-file-closing failed");
  if (p == 0)
    puts("Target-file closed successfully");
  return (0);
}
```

## 18. Copy a Binary File


```c

#include <stdio.h>
int main() {
  FILE *fptrSource, *fptrTarget;
  int m, n, p;
  fptrSource = fopen("C:\\Output\\hello.exe", "rb");
  if (fptrSource == NULL) {
    puts("Source-file-opening failed");
    exit(1);
  }
  puts("Source-file Hello.exe opened successfully");
  fptrTarget = fopen("C:\\Output\\world.exe", "wb");
  if (fptrTarget == NULL) {
    puts("Target-file-opening failed");
    exit(2);
  }
  puts("Target-file World.exe opened successfully");
  m = fgetc(fptrSource);
  while (m != EOF) {
    fputc(m, fptrTarget);
    m = fgetc(fptrSource);
  }
  puts("File copied successfully");
  n = fclose(fptrSource);
  if (n == -1)
    puts("Source-file-closing failed");
  if (n == 0)
    puts("Source-file closed successfully");
  p = fclose(fptrTarget);
  if (p == -1)
    puts("Target-file-closing failed");
  if (p == 0)
    puts("Target-file closed successfully");
  return (0);
}
```

## 19. Write to a File and Then Read from That File


```c

#include <stdio.h>
int main() {
  FILE *fptr;
  char store[80];
  int k;
  fptr = fopen("C:\\Code\\pune.txt", "w+");
  if (fptr != NULL) {
    puts("File pune.txt opened successfully");
    fputs("Pune is very nice city.", fptr);
    puts("Text written to file pune.txt successfully");
    rewind(fptr);
    fgets(store, 80, fptr);
    puts("Contents of file pune.txt:");
    puts(store);
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File closed successfully");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 20. Position a Text File to a Desired Character


```c

#include <stdio.h>
int main() {
  FILE *fptr;
  int m, n, k, p;
  fptr = fopen("C:\\Code\\pune.txt", "r");
  if (fptr != NULL) {
    puts("File pune.txt opened successfully");
    puts("Let n denotes current file position");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    printf("Let us read a single character and it is: ");
    m = fgetc(fptr);
    putchar(m);
    printf("\n");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    fseek(fptr, 8, 0);
    puts("Statement \"fseek(fptr, 8, 0);\" executed");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    fseek(fptr, 3, 1);
    puts("Statement \"fseek(fptr, 3, 1);\" executed");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    fseek(fptr, -5, 1);
    puts("Statement \"fseek(fptr, -5, 1);\" executed");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    fseek(fptr, -3, 2);
    puts("Statement \"fseek(fptr, -3, 2);\" executed");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    fseek(fptr, 0, 2);
    puts("Statement \"fseek(fptr, 0, 2);\" executed");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    puts("Now let us perform a read operation");
    m = fgetc(fptr);
    printf("Value read is %d\n", m);
    n = ftell(fptr);
    printf("Now value of n is still %d\n", n);
    fseek(fptr, 0, 0);
    puts("Statement \"fseek(fptr, 0, 0);\" executed");
    n = ftell(fptr);
    printf("Now value of n is %d\n", n);
    puts("That's all.");
    k = fclose(fptr);
    if (k == -1)
      puts("File-closing failed");
    if (k == 0)
      puts("File closed successfully.");
  } else
    puts("File-opening failed");
  return (0);
}
```

## 21. Read from the Device File Keyboard


```c

#include <stdio.h>
int main() {
  char text[500];
  int m, n = 0, p;
  puts("Type the text. The text you type form the contents");
  puts("of the device-file keyboard. Strike the function");
  puts("key F6 to signify the end of this file.");
  m = fgetc(stdin);
  while (m != EOF) {
    text[n] = m;
    n = n + 1;
    m = fgetc(stdin);
  }
  puts("Contents of device file \"keyboard\":");
  for (p = 0; p < n; p++)
    putchar(text[p]);
  return (0);
}
```

## 22. Write Text to the Device File Monitor


```c

#include <stdio.h>
main() {
  int m, k;
  FILE *fptr;
  fptr = fopen("C:\\Code\\satara.txt", "r");
  if (fptr != NULL) {
    puts("Disk-file kolkata.txt opened successfully.");
    puts("`Its contents are now written to device file monitor:");
    m = fgetc(fptr);
    while (m != EOF) {
      fputc(m, stdout);
      m = fgetc(fptr);
    }
    k = fclose(fptr);
    if (k == -1)
      fprintf(stderr, "Disk-file closing failed\n");
    if (k == 0)
      puts("Disk-file closed successfully.");
  } else
    fprintf(stderr, "Disk-file opening failed\n");
  return (0);
}
```

## 23. Read Text from the Device File Keyboard and Write It to the Device File Monitor


```c

#include <stdio.h>
main() {
  char text[500];
  int m, n = 0, p;
  puts("Type the text. The text you type form the contents");
  puts("of the device-file keyboard. Strike the function");
  puts("key F6 to signify the end of this file.");
  m = fgetc(stdin);
  while (m != EOF) {
    text[n] = m;
    n = n + 1;
    m = fgetc(stdin);
  }
  puts("Contents of the device-file keyboard are now");
  puts("written to the device-file monitor.");
  for (p = 0; p < n; p++)
    fputc(text[p], stdout);
  return (0);
}
```
