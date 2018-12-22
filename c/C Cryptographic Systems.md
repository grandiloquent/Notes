# C Cryptographic Systems

- [1. Use the Reverse Cipher Method](#1-use-the-reverse-cipher-method)
- [2. Use the Caesar Cipher Method](#2-use-the-caesar-cipher-method)
- [3. Use the Transposition Cipher Method](#3-use-the-transposition-cipher-method)
- [4. Use the Multiplicative Cipher Method](#4-use-the-multiplicative-cipher-method)
- [5. Use the Affine Cipher Method](#5-use-the-affine-cipher-method)
- [6. Use the Simple Substitution Cipher Method](#6-use-the-simple-substitution-cipher-method)
- [7. Use the Vigenère Cipher Method](#7-use-the-vigenère-cipher-method)
- [8. Use the One-Time Pad Cipher Method](#8-use-the-one-time-pad-cipher-method)
- [9. Use the RSA Cipher Method](#9-use-the-rsa-cipher-method)

## 1. Use the Reverse Cipher Method


```c

#include <stdio.h>
#include <string.h>
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
int intChoice, length;
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int i, j;
  fflush(stdin);
  printf("Enter the Message to be Encrypted (upto 100 characters): \n");
  gets(msgOriginal);
  length = strlen(msgOriginal);
  j = length - 1;
  for (i = 0; i < length; i++) {
    msgEncrypt[j] = msgOriginal[i];
    j--;
  }
  msgEncrypt[length] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  int i, j;
  fflush(stdin);
  printf("Enter the Message to be Decrypted (upto 100 characters): \n");
  gets(msgEncrypt);
  length = strlen(msgEncrypt);
  j = length - 1;
  for (i = 0; i < length; i++) {
    msgDecrypt[j] = msgEncrypt[i];
    j--;
  }
  msgDecrypt[length] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main()
{
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 2. Use the Caesar Cipher Method


```c

#include <stdio.h>
#include <string.h>
#define KEY 5
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
int intChoice, length;
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int i, ch;
  fflush(stdin);
  printf("Enter the Message to Encrypt, Do Not Include Spaces and \n");
  printf("Punctuation Symbols (upto 100 alphabets): \n");
  gets(msgOriginal);
  length = strlen(msgOriginal);
  for (i = 0; i < length; i++) {
    ch = msgOriginal[i];
    if (ch >= 'a' && ch <= 'z') {
      ch = ch + KEY;
      if (ch > 'z')
        ch = ch - 'z' + 'a' - 1;
      msgEncrypt[i] = ch;
    } else if (ch >= 'A' && ch <= 'Z') {
      ch = ch + KEY;
      if (ch > 'Z')
        ch = ch - 'Z' + 'A' - 1;
      msgEncrypt[i] = ch;
    }
  }
  msgEncrypt[length] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  int i, ch;
  fflush(stdin);
  printf("Enter the Message to Decrypt (upto 100 alphabets):\n");
  gets(msgEncrypt);
  length = strlen(msgEncrypt);
  for (i = 0; i < length; i++) {
    ch = msgEncrypt[i];
    if (ch >= 'a' && ch <= 'z') {
      ch = ch - KEY;
      if (ch < 'a')
        ch = ch + 'z' - 'a' + 1;
      msgDecrypt[i] = ch;
    } else if (ch >= 'A' && ch <= 'Z') {
      ch = ch - KEY;
      if (ch < 'A')
        ch = ch + 'Z' - 'A' + 1;
      msgDecrypt[i] = ch;
    }
  }
  msgDecrypt[length] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main() {
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 3. Use the Transposition Cipher Method


```c

#include <stdio.h>
#include <string.h>
#define KEY 7
char msgToEncrypt[110];
char msgToDecrypt[110];
char msgEncrypt[20][KEY];
char msgDecrypt[20][KEY];
int intChoice, length;
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int row, col, rows, cilr, length, k = 0;
  printf("Enter the Message (20 to 110 letters) to be Encrypted: \n");
  fflush(stdin);
  gets(msgToEncrypt);
  length = strlen(msgToEncrypt);
  rows = (length / KEY) + 1;
  cilr = length % KEY;
  for (row = 0; row < rows; row++) {
    for (col = 0; col < KEY; col++) {
      msgEncrypt[row][col] = msgToEncrypt[k++];
      if (k == length)
        break;
    }
  }
  printf("\nEncrypted Message: \n");
  for (col = 0; col < KEY; col++) {
    for (row = 0; row < rows; row++) {
      if ((col >= cilr) && (row == (rows - 1)))
        continue;
      printf("%c", msgEncrypt[row][col]);
    }
  }
}
void decryptMsg() {
  int row, col, rows, cilr, length, k = 0;
  printf("Enter the Message (20 to 110 letters) to be Decrypted: \n");
  fflush(stdin);
  gets(msgToDecrypt);
  length = strlen(msgToDecrypt);
  rows = (length / KEY) + 1;
  cilr = length % KEY;
  for (col = 0; col < KEY; col++) {
    for (row = 0; row < rows; row++) {
      if ((col >= cilr) && (row == (rows - 1)))
        continue;
      msgDecrypt[row][col] = msgToDecrypt[k++];
      if (k == length)
        break;
    }
  }
  printf("\nDecrypted Message: \n");
  for (row = 0; row < rows; row++) {
    for (col = 0; col < KEY; col++) {
      printf("%c", msgDecrypt[row][col]);
    }
  }
}
void main()
{
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 4. Use the Multiplicative Cipher Method


```c

#include <stdio.h>
#include <string.h>
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
int length, intChoice, a = 3;
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int i;
  printf("Enter the Message to Encrypt in FULL CAPS, Do Not Include \n");
  printf("Spaces and Punctuation Symbols (upto 100 characters): \n");
  fflush(stdin);
  gets(msgOriginal);
  length = strlen(msgOriginal);
  for (i = 0; i < length; i++)
    msgEncrypt[i] = (((a * msgOriginal[i]) % 26) + 65);
  msgEncrypt[length] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  int i;
  int aInv = 0;
  int flag = 0;
  printf("Enter the Message to Decrypt (upto 100 characters): \n");
  fflush(stdin);
  gets(msgEncrypt);
  length = strlen(msgEncrypt);
  for (i = 0; i < 26; i++) {
    flag = (a * i) % 26;
    if (flag == 1)
      aInv = i;
  }
  for (i = 0; i < length; i++)
    msgDecrypt[i] = (((aInv * msgEncrypt[i]) % 26) + 65);
  msgDecrypt[length] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main() {
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 5. Use the Affine Cipher Method


```c

#include <stdio.h>
#include <string.h>
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
int intChoice, length, a = 3, b = 5;
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int i;
  printf("Enter the Message to Encrypt in FULL CAPS, Do Not Include \n");
  printf("Spaces and Punctuation Symbols (upto 100 characters): \n");
  fflush(stdin);
  gets(msgOriginal);
  length = strlen(msgOriginal);
  for (i = 0; i < length; i++)
    msgEncrypt[i] = ((((a * msgOriginal[i]) + b) % 26) + 65);
  msgEncrypt[length] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  int i;
  int aInv = 0;
  int flag = 0;
  printf("Enter the Message to Decrypt (upto 100 chars): \n");
  fflush(stdin);
  gets(msgEncrypt);
  length = strlen(msgEncrypt);
  for (i = 0; i < 26; i++) {
    flag = (a * i) % 26;
    if (flag == 1)
      aInv = i;
  }
  for (i = 0; i < length; i++)
    msgDecrypt[i] = (((aInv * ((msgEncrypt[i] - b)) % 26)) + 65);
  msgDecrypt[length] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main() {
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 6. Use the Simple Substitution Cipher Method


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
int intEncryptKey[26], intDecryptKey[26];
int intChoice, i, j, seed, length, randNum, num, flag = 1, tag = 0;
void generateKey() {
  printf("\nEnter seed S (1 <= S <= 30000): ");
  scanf("%d", &seed);
  srand(seed);
  for (i = 0; i < 26; i++)
    intEncryptKey[i] = -1;
  for (i = 0; i < 26; i++) {
    do {
      randNum = rand();
      num = randNum % 26;
      flag = 1;
      for (j = 0; j < 26; j++)
        if (intEncryptKey[j] == num)
          flag = 0;
      if (flag == 1) {
        intEncryptKey[i] = num;
        tag = tag + 1;
      }
    } while ((!flag) && (tag < 26));
  }
  printf("\nEncryption KEY = ");
  for (i = 0; i < 26; i++)
    printf("%c", intEncryptKey[i] + 65);
  for (i = 0; i < 26; i++) {
    for (j = 0; j < 26; j++) {
      if (i == intEncryptKey[j]) {
        intDecryptKey[i] = j;
        break;
      }
    }
  }
  printf("\nDecryption KEY = ");
  for (i = 0; i < 26; i++)
    printf("%c", intDecryptKey[i] + 65);
}
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  printf("Enter the Message to Encrypt in FULL CAPS, Do Not Include \n");
  printf("Spaces and Punctuation Symbols (upto 100 characters): \n");
  fflush(stdin);
  gets(msgOriginal);
  length = strlen(msgOriginal);
  for (i = 0; i < length; i++)
    msgEncrypt[i] = (intEncryptKey[(msgOriginal[i]) - 65]) + 65;
  msgEncrypt[length] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  printf("Enter the Message to Decrypt (upto 100 chars): \n");
  fflush(stdin);
  gets(msgEncrypt);
  length = strlen(msgEncrypt);
  for (i = 0; i < length; i++)
    msgDecrypt[i] = (intDecryptKey[(msgEncrypt[i]) - 65]) + 65;
  msgDecrypt[length] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main() {
  generateKey();
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 7. Use the Vigenère Cipher Method


```c

#include <stdio.h>
#include <string.h>
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
char msgKey[15];
int intChoice, lenKey, lenMsg, intKey[15];
void getKey() {
  int i, j;
  fflush(stdin);
  printf("\nEnter TEXT KEY in FULL CAPS, Do Not Include Spaces and \n");
  printf("Punctuation Symbols (upto 15 characters): \n");
  gets(msgKey);
  lenKey = strlen(msgKey);
  for (i = 0; i < lenKey; i++)
    intKey[i] = msgKey[i] - 65;
}
void menu()
{
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Decrypt an Encrypted Message.");
  printf("\nEnter 3 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1, 2 or 3) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int i, j, ch;
  fflush(stdin);
  printf("Enter the Message to be Encrypted (upto 100 alphabets), ");
  printf("do not include \nspaces and punctuation symbols:\n");
  gets(msgOriginal);
  lenMsg = strlen(msgOriginal);
  for (i = 0; i < lenMsg; i++) {
    j = i % lenKey;
    ch = msgOriginal[i];
    if (ch >= 'a' && ch <= 'z') {
      ch = ch + intKey[j];
      if (ch > 'z')
        ch = ch - 'z' + 'a' - 1;
      msgEncrypt[i] = ch;
    } else if (ch >= 'A' && ch <= 'Z') {
      ch = ch + intKey[j];
      if (ch > 'Z')
        ch = ch - 'Z' + 'A' - 1;
      msgEncrypt[i] = ch;
    }
  }
  msgEncrypt[lenMsg] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  int i, j, ch;
  fflush(stdin);
  printf("Enter the Message to be Decrypted (upto 100 alphabets):\n");
  gets(msgEncrypt);
  lenMsg = strlen(msgEncrypt);
  for (i = 0; i < lenMsg; i++) {
    j = i % lenKey;
    ch = msgEncrypt[i];
    if (ch >= 'a' && ch <= 'z') {
      ch = ch - intKey[j];
      if (ch < 'a')
        ch = ch + 'z' - 'a' + 1;
      msgDecrypt[i] = ch;
    } else if (ch >= 'A' && ch <= 'Z') {
      ch = ch - intKey[j];
      if (ch < 'A')
        ch = ch + 'Z' - 'A' + 1;
      msgDecrypt[i] = ch;
    }
  }
  msgDecrypt[lenMsg] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main() {
  getKey();
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      break;
    case 2:
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 8. Use the One-Time Pad Cipher Method


```c

#include <stdio.h>
#include <string.h>
char msgOriginal[100];
char msgEncrypt[100];
char msgDecrypt[100];
char msgKey[100];
int intChoice, lenKey, lenMsg, intKey[100];
void generateKey() {
  int i, randNum, num, seed;
  lenKey = lenMsg;
  printf("\nEnter seed S (1 <= S <= 30000): ");
  scanf("%d", &seed);
  srand(seed);
  for (i = 0; i < lenKey; i++) {
    randNum = rand();
    num = randNum % 26;
    msgKey[i] = num + 65;
    intKey[i] = num;
  }
  msgKey[lenKey] = '\0';
  printf("\nKey: %s", msgKey);
}
void menu() {
  printf("\nEnter 1 to Encrypt a Message.");
  printf("\nEnter 2 to Stop the Execution of Program.");
  printf("\nNow Enter Your Choice (1 or 2) and Strike Enter Key: ");
  scanf("%d", &intChoice);
}
void encryptMsg() {
  int i, j, ch;
  fflush(stdin);
  printf("Enter the Message to be Encrypted (upto 100 alphabets), ");
  printf("Do Not Include \nSpaces and Punctuation Symbols:\n");
  gets(msgOriginal);
  lenMsg = strlen(msgOriginal);
  generateKey();
  for (i = 0; i < lenMsg; i++) {
    ch = msgOriginal[i];
    if (ch >= 'a' && ch <= 'z') {
      ch = ch + intKey[i];
      if (ch > 'z')
        ch = ch - 'z' + 'a' - 1;
      msgEncrypt[i] = ch;
    } else if (ch >= 'A' && ch <= 'Z') {
      ch = ch + intKey[i];
      if (ch > 'Z')
        ch = ch - 'Z' + 'A' - 1;
      msgEncrypt[i] = ch;
    }
  }
  msgEncrypt[lenMsg] = '\0';
  printf("\nEncrypted Message: %s", msgEncrypt);
}
void decryptMsg() {
  int i, j, ch;
  fflush(stdin);
  printf("\nEnter the Message to be Decrypted (upto 100 alphabets):\n");
  gets(msgEncrypt);
  lenMsg = strlen(msgEncrypt);
  for (i = 0; i < lenMsg; i++) {
    ch = msgEncrypt[i];
    if (ch >= 'a' && ch <= 'z') {
      ch = ch - intKey[i];
      if (ch < 'a')
        ch = ch + 'z' - 'a' + 1;
      msgDecrypt[i] = ch;
    } else if (ch >= 'A' && ch <= 'Z') {
      ch = ch - intKey[i];
      if (ch < 'A')
        ch = ch + 'Z' - 'A' + 1;
      msgDecrypt[i] = ch;
    }
  }
  msgDecrypt[lenMsg] = '\0';
  printf("\nDecrypted Message: %s", msgDecrypt);
}
void main() {
  do {
    menu();
    switch (intChoice) {
    case 1:
      encryptMsg();
      decryptMsg();
      break;
    default:
      printf("\nThank you.\n");
      exit(0);
    }
  } while (1);
}
```

## 9. Use the RSA Cipher Method


```c

#include <stdio.h>
#include <math.h>
#include <string.h>
long int i, j, p, q, n, t, flag;
long int e[100], d[100], temp[100], msgDecrypt[100], msgEncrypt[100];
char msgOriginal[100];
int prime(long int);
int findPrime(long int s);
void computeKeys();
long int cd(long int);
void encryptMsg();
void decryptMsg();
void main() {
  long int s;
  do {
    printf("Enter the serial number S of 1st prime number (10 <= S <= 40): ");
    scanf("%ld", &s);
  } while ((s < 10) || (s > 40));
  p = findPrime(s);
  printf("First prime number p is: %d \n", p);
  do {
    printf("Enter the serial number S of 2nd prime number (10 <= S <= 40): ");
    scanf("%ld", &s);
  } while ((s < 10) || (s > 40));
  q = findPrime(s);
  printf("Second prime number q is: %d \n", q);
  printf("\nEnter the Message to be Encrypted, Do Not Include Spaces:\n");
  fflush(stdin);
  scanf("%s", msgOriginal);
  for (i = 0; msgOriginal[i] != NULL; i++)
    msgDecrypt[i] = msgOriginal[i];
  n = p * q;
  t = (p - 1) * (q - 1);
  computeKeys();
  printf("\nPossible Values of e and d Are:\n");
  for (i = 0; i < j - 1; i++)
    printf("\n %ld \t %ld", e[i], d[i]);
  printf("\nSample Public Key: (%ld,  %ld)", n, e[i - 1]);
  printf("\nSample Private Key: (%ld,  %ld)", n, d[i - 1]);
  encryptMsg();
  decryptMsg();
}
int findPrime(long int s) {
  int f, d, tag;
  f = 2;
  i = 1;
  while (i <= s) {
    tag = 1;
    for (d = 2; d <= f - 1; d++) {
      if (f % d == 0) {
        tag = 0;
        break;
      }
    }
    if (tag == 1) {
      if (i == s)
        return (f);
      i++;
    }
    f++;
  }
  return (0);
}
int prime(long int pr) {
  int i;
  j = sqrt(pr);
  for (i = 2; i <= j; i++) {
    if (pr % i == 0)
      return 0;
  }
  return 1;
}
void computeKeys() {
  int k;
  k = 0;
  for (i = 2; i < t; i++) {
    if (t % i == 0)
      continue;
    flag = prime(i);
    if (flag == 1 && i != p && i != q) {
      e[k] = i;
      flag = cd(e[k]);
      if (flag > 0) {
        d[k] = flag;
        k++;
      }
      if (k == 99)
        break;
    }
  }
}
long int cd(long int x) {
  long int k = 1;
  while (1) {
    k = k + t;
    if (k % x == 0)
      return (k / x);
  }
}
void encryptMsg()
{
  long int pt, ct, key = e[0], k, length;
  i = 0;
  length = strlen(msgOriginal);
  while (i != length) {
    pt = msgDecrypt[i];
    pt = pt - 96;
    k = 1;
    for (j = 0; j < key; j++) {
      k = k * pt;
      k = k % n;
    }
    temp[i] = k;
    ct = k + 96;
    msgEncrypt[i] = ct;
    i++;
  }
  msgEncrypt[i] = -1;
  printf("\nThe Encrypted Message:\n");
  for (i = 0; msgEncrypt[i] != -1; i++)
    printf("%c", msgEncrypt[i]);
}
void decryptMsg() {
  long int pt, ct, key = d[0], k;
  i = 0;
  while (msgEncrypt[i] != -1) {
    ct = temp[i];
    k = 1;
    for (j = 0; j < key; j++) {
      k = k * ct;
      k = k % n;
    }
    pt = k + 96;
    msgDecrypt[i] = pt;
    i++;
  }
  msgDecrypt[i] = -1;
  printf("\nThe Decrypted Message:\n");
  for (i = 0; msgDecrypt[i] != -1; i++)
    printf("%c", msgDecrypt[i]);
  printf("\nThank you. \n ");
}
```


