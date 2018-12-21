# Command Line

## gcc

```
gcc main.c -o m && m
```

## clang-format


```
clang-format -help
clang-format -sort-includes=false -style=Google -i fuzzy_match.h
```

## ebook-convert


```
for /r %i in (*.epub) do "C:\Calibre Portable\Calibre\ebook-convert.exe" "%i" "%~ni.azw3"
```
 
## git

```
echo "# Notes" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/grandiloquent/Notes.git
git push -u origin master
```

```
git add .
git commit -m "changed"
git push
```
