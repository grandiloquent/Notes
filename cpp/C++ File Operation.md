# C++ File Operation


```c++
#include <algorithm>
#include <dirent.h>
#include <functional>
#include <iostream>
#include <sys/stat.h>
#include <vector>

using namespace std;
int streq(const char *s1, const char *s2) {
  while (*s1 == *s2++)
    if (*s1++ == 0)
      return 0;
  return -1;
}
inline bool endsWith(std::string const &value, std::string const &ending) {
  if (ending.size() > value.size())
    return false;
  return std::equal(ending.rbegin(), ending.rend(), value.rbegin());
}
void traverseDirectory(const string &path, vector<string> &list) {
  // cout << "Traversing the directory: " << path << endl;

  DIR *dir = opendir(path.c_str());
  if (dir == nullptr) {
    return;
  }

  struct dirent *entry;
  struct stat stats;
  while ((entry = readdir(dir))) {
    if (streq(entry->d_name, ".") == 0 || streq(entry->d_name, "..") == 0)
      continue;
    string fullName = path + "/" + entry->d_name;
    stat(fullName.c_str(), &stats);
    if ((stats.st_mode & S_IFMT) == S_IFDIR) {
      // https://devdocs.io/cpp/container/vector/push_back
      list.push_back(fullName);
      traverseDirectory(fullName, list);
    }
  }
  closedir(dir);
}
void deleteDirectory(const string &path) {
  DIR *dir = opendir(path.c_str());
  if (dir == NULL) {
    cout << "Directory does not exist or an unknown error has occurred" << endl;
    return;
  }
  struct dirent *entry;
  struct stat stats;
  // http://man7.org/linux/man-pages/man3/readdir.3.html
  while ((entry = readdir(dir))) {
    if (streq(entry->d_name, ".") == 0 || streq(entry->d_name, "..") == 0)
      continue;
    string fullName = path + "/" + entry->d_name;
    // http://man7.org/linux/man-pages/man2/stat.2.html
    stat(fullName.c_str(), &stats);
    if ((stats.st_mode & S_IFMT) == S_IFDIR) {
      deleteDirectory(fullName);
    } else {
      // http://man7.org/linux/man-pages/man2/unlink.2.html
      unlink(fullName.c_str());
    }
  }
  closedir(dir);
  // cout << "Try to delete the folder: " << path << endl;
  // http://man7.org/linux/man-pages/man2/rmdir.2.html
  rmdir(path.c_str());
}

int main() {
  string path("C:/Codes/Codes/C++");
  vector<string> list;
  traverseDirectory(path, list);
  std::sort(list.begin(), list.end());
  string filter("/bin");
  for (auto &&i : list) {
    if (endsWith(i, filter)) {
      deleteDirectory(i);
      cout << i << endl;
    }
  }
  return 0;
}
```

