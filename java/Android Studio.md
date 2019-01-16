#Java: Android Studio

移动静态方法

鼠标放在方法中
F6

加载历史项目时删除.idea目录
https://stackoverflow.com/questions/12486846/intellij-generate-switch-case
## adb

```java
C:\Users\psycho\AppData\Local\Android\Sdk\platform-tools\adb shell
cd /storage/19E7-1704/Books/Safari/EPUB && ls -R >/storage/19E7-1704/books.txt
C:\Users\psycho\AppData\Local\Android\Sdk\platform-tools\adb  pull /storage/19E7-1704/books.txt
```


```java
Log\.e\(TAG,\s*\"===>[^\"]*\"\);\n
System.out.println\("===> \[[^\]]*\]"\);\n
(?<= +)(private)
Log\.e\(TAG,[^\)]*\);\n
(?<!// )Log\.[e|d]\([^\(]*\);\n
// $0
```

## 快捷键

https://developer.android.com/studio/intro/keyboard-shortcuts

|ShortCut|MenuItem|
|---|---|
|F5|Run > Run|
|F1|Code > Reformat Code|
|Alt + 6|View > Tool Windows > Logcat|
||Refactor > Find and Replace Code Duplicates|
||Refactor > Extract > Method|
|Control + F12|在 Studio 内导航和搜索 > 打开文件结构弹出式菜单|
|Control + Shift + U|Toggle Case|

|主菜单|菜单|快捷键|
|---|---|---|
|Analyze|Code Cleanup||
|Code|Collapse All|Ctrl+1|
|Code|Expand All|Ctrl+2|
|Code|Optimize Imports|Ctrl+Alt+O|
|Code|Generate|Alt+Insert|
|Code|Rearrange Code|F2|
|Code|Reformat Code|F1|

## 禁止 Debug C++

工具栏 - app - Run/Debug Configurations - Debugger - Debug type - Java

- https://stackoverflow.com/questions/146576/why-is-the-java-main-method-static
