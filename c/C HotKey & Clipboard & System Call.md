# C HotKey & Clipboard & System Call

```c
#include <windows.h>

#include <stdio.h>
#include <tchar.h>
#include <strsafe.h>
#include <sys/stat.h>
#include <string.h>

// https://docs.microsoft.com/zh-cn/windows/desktop/Debug/retrieving-the-last-error-code
void ErrorExit(LPTSTR lpszFunction) {
  // Retrieve the system error message for the last-error code

  LPVOID lpMsgBuf;
  LPVOID lpDisplayBuf;
  DWORD dw = GetLastError();

  FormatMessage(FORMAT_MESSAGE_ALLOCATE_BUFFER | FORMAT_MESSAGE_FROM_SYSTEM |
                    FORMAT_MESSAGE_IGNORE_INSERTS,
                NULL, dw, MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT),
                (LPTSTR)&lpMsgBuf, 0, NULL);

  // Display the error message and exit the process

  lpDisplayBuf =
      (LPVOID)LocalAlloc(LMEM_ZEROINIT, (lstrlen((LPCTSTR)lpMsgBuf) +
                                         lstrlen((LPCTSTR)lpszFunction) + 40) *
                                            sizeof(TCHAR));
  StringCchPrintf((LPTSTR)lpDisplayBuf, LocalSize(lpDisplayBuf) / sizeof(TCHAR),
                  TEXT("%s failed with error %d: %s"), lpszFunction, dw,
                  lpMsgBuf);
  MessageBox(NULL, (LPCTSTR)lpDisplayBuf, TEXT("Error"), MB_OK);

  LocalFree(lpMsgBuf);
  LocalFree(lpDisplayBuf);
  ExitProcess(dw);
}
BOOL IsFile(const char *fileName) {
  struct stat buffer;
  return (stat(fileName, &buffer) == 0);
}
BOOL GetClipboardText(LPTSTR buff, UINT iBuffSize) {
  HANDLE hClip;
  char *pszClipboard;

  if (OpenClipboard(NULL)) // Open the clipboard so we can read its contents
  {
    hClip = GetClipboardData(CF_TEXT); // Get a handle to the clipboad contents
    if (hClip == NULL) {
      CloseClipboard();
      return FALSE;
    }
    pszClipboard =
        (char *)GlobalLock(hClip); // Point pClipboard to the clipboard contents
    if (!pszClipboard) {
      GlobalUnlock(hClip);
      return FALSE;
    }
    strncpy(buff, pszClipboard,
            iBuffSize); // Copy the contents to a local variable
    buff[iBuffSize - 1] = 0;
    GlobalUnlock(hClip); // Release the lock on the clipboard contents
    CloseClipboard();    // Close the clipboard.
  } else {
    return FALSE;
  }

  return TRUE;
}
#define BUFFER_SIZE 512

int main() {
  //   printf("%s", rstrstr("C:\\Codes\\Codes\\C\\x.c", "\\"));
  //   return 0;
  // https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-registerhotkey
  RegisterHotKey(NULL, 0, 0, 0x78);
  // https://docs.microsoft.com/en-us/windows/desktop/api/winuser/ns-winuser-tagmsg
  MSG msg;
  // https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-getmessage
  while (GetMessage(&msg, 0, 0, 0)) {
    if (msg.message == WM_HOTKEY) {
      char lpszDynamic[MAX_PATH];
      GetClipboardText(lpszDynamic, MAX_PATH);
      if (IsFile(lpszDynamic)) {
        char buffer[BUFFER_SIZE];
        size_t len = strlen(lpszDynamic);
        char *fileName = lpszDynamic + len - 1;
        while (*fileName != '\\') {
          if (*fileName-- == 0)
            break;
        }
        fileName = fileName + 1;
        len = strlen(fileName);
        char name[len];
        size_t i = 0;
        for (i = 0; i < len; i++) {
          if (*fileName != '.') {
            name[i] = *fileName;
          } else {
            name[i] = '\0';
            break;
          }
          fileName++;
        }

        snprintf(buffer, BUFFER_SIZE, "cmd /K g++ \"%s\" -o %s && %s",
                 lpszDynamic, name, name);

        printf("%s\n", buffer);
        // free(fileName);

        STARTUPINFO si;
        PROCESS_INFORMATION pi;

        ZeroMemory(&si, sizeof(si));
        si.cb = sizeof(si);
        ZeroMemory(&pi, sizeof(pi));

        // Start the child process.
        if (!CreateProcess(NULL,   // No module name (use command line)
                           buffer, // Command line
                           NULL,   // Process handle not inheritable
                           NULL,   // Thread handle not inheritable
                           FALSE,  // Set handle inheritance to FALSE
                           0,      // No creation flags
                           NULL,   // Use parent's environment block
                           NULL,   // Use parent's starting directory
                           &si,    // Pointer to STARTUPINFO structure
                           &pi)    // Pointer to PROCESS_INFORMATION structure
        ) {
          printf("CreateProcess failed (%d).\n", GetLastError());
          return 1;
        }

        // Wait until child process exits.
        WaitForSingleObject(pi.hProcess, INFINITE);

        // Close process and thread handles.
        CloseHandle(pi.hProcess);
        CloseHandle(pi.hThread);
      }
    }
  }
}
// gcc x.c -o m && m
```

