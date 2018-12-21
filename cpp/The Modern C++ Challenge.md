# The Modern C++ Challenge

- https://github.com/PacktPublishing/Modern-C-plus-plus-Efficient-and-Scalable-Application-Development

- [Math Problems](#math-problems)
	- [1. Sum of naturals divisible by 3 and 5](#1-sum-of-naturals-divisible-by-3-and-5)
	- [2. Greatest common divisor](#2-greatest-common-divisor)
	- [3. Least common multiple](#3-least-common-multiple)
	- [4. Largest prime smaller than given number](#4-largest-prime-smaller-than-given-number)
	- [5. Sexy prime pairs](#5-sexy-prime-pairs)
	- [6. Abundant numbers](#6-abundant-numbers)
	- [7. Amicable numbers](#7-amicable-numbers)
	- [8. Armstrong numbers](#8-armstrong-numbers)
	- [9. Prime factors of a number](#9-prime-factors-of-a-number)
	- [10. Gray code](#10-gray-code)
	- [11. Converting numerical values to Roman](#11-converting-numerical-values-to-roman)
	- [12. Largest Collatz sequence](#12-largest-collatz-sequence)
	- [13. Computing the value of Pi](#13-computing-the-value-of-pi)
	- [14. Validating ISBNs](#14-validating-isbns)
- [Language Features](#language-features)
	- [15. IPv4 data type](#15-ipv4-data-type)
	- [16. Enumerating IPv4 addresses in a range](#16-enumerating-ipv4-addresses-in-a-range)
	- [17. Creating a 2D array with basic operations](#17-creating-a-2d-array-with-basic-operations)
	- [18. Minimum function with any number of arguments](#18-minimum-function-with-any-number-of-arguments)
	- [19. Adding a range of values to a container](#19-adding-a-range-of-values-to-a-container)
	- [20. Container any, all, none](#20-container-any-all-none)
	- [21. System handle wrapper](#21-system-handle-wrapper)
	- [22. Literals of various temperature scales](#22-literals-of-various-temperature-scales)
- [Strings and Regular Expressions](#strings-and-regular-expressions)
	- [23. Binary to string conversion](#23-binary-to-string-conversion)
	- [24. String to binary conversion](#24-string-to-binary-conversion)
	- [25. Capitalizing an article title](#25-capitalizing-an-article-title)
	- [26. Joining strings together separated by a delimiter](#26-joining-strings-together-separated-by-a-delimiter)
	- [27. Splitting a string into tokens with a list of possible delimiters](#27-splitting-a-string-into-tokens-with-a-list-of-possible-delimiters)
	- [28. Longest palindromic substring](#28-longest-palindromic-substring)
	- [29. License plate validation](#29-license-plate-validation)
	- [30. Extracting URL parts](#30-extracting-url-parts)
	- [31. Transforming dates in strings](#31-transforming-dates-in-strings)
- [Streams and Filesystems](#streams-and-filesystems)
	- [32. Pascal's triangle](#32-pascals-triangle)
	- [33. Tabular printing of a list of processes](#33-tabular-printing-of-a-list-of-processes)
	- [34. Removing empty lines from a text file](#34-removing-empty-lines-from-a-text-file)
	- [35. Computing the size of a directory](#35-computing-the-size-of-a-directory)
	- [36. Deleting files older than a given date](#36-deleting-files-older-than-a-given-date)
	- [37. Finding files in a directory that match a regular expression](#37-finding-files-in-a-directory-that-match-a-regular-expression)
	- [38. Temporary log files](#38-temporary-log-files)
- [Date and Time](#date-and-time)
	- [39. Measuring function execution time](#39-measuring-function-execution-time)
	- [40. Number of days between two dates](#40-number-of-days-between-two-dates)
	- [41. Day of the week](#41-day-of-the-week)
	- [42. Day and week of the year](#42-day-and-week-of-the-year)
	- [43. Meeting time for multiple time zones](#43-meeting-time-for-multiple-time-zones)
	- [44. Monthly calendar](#44-monthly-calendar)
- [Algorithms and Data Structures](#algorithms-and-data-structures)
	- [45. Priority queue](#45-priority-queue)
	- [46. Circular buffer](#46-circular-buffer)
	- [47. Double buffer](#47-double-buffer)
	- [48. The most frequent element in a range](#48-the-most-frequent-element-in-a-range)
	- [49. Text histogram](#49-text-histogram)
	- [50. Filtering a list of phone numbers](#50-filtering-a-list-of-phone-numbers)
	- [51. Transforming a list of phone numbers](#51-transforming-a-list-of-phone-numbers)
	- [52. Generating all the permutations of a string](#52-generating-all-the-permutations-of-a-string)
	- [53. Average rating of movies](#53-average-rating-of-movies)
	- [54. Pairwise algorithm](#54-pairwise-algorithm)
	- [55. Zip algorithm](#55-zip-algorithm)
	- [56. Select algorithm](#56-select-algorithm)
	- [57. Sort algorithm](#57-sort-algorithm)
	- [58. The shortest path between nodes](#58-the-shortest-path-between-nodes)
	- [59. The Weasel program](#59-the-weasel-program)
	- [60. The Game of Life](#60-the-game-of-life)




## Math Problems

### 1. Sum of naturals divisible by 3 and 5


```c++

#include <iostream>
int main() {
  unsigned int limit = 0;
  std::cout << "Upper limit:";
  std::cin >> limit;
  unsigned long long sum = 0;
  for (unsigned int i = 3; i < limit; ++i) {
    if (i % 3 == 0 || i % 5 == 0)
      sum += i;
  }
  std::cout << "sum=" << sum << std::endl;
}
```

### 2. Greatest common divisor


```c++

#include <iostream>
unsigned int gcd(unsigned int const a, unsigned int const b) {
  return b == 0 ? a : gcd(b, a % b);
}
unsigned int gcd2(unsigned int a, unsigned int b) {
  while (b != 0) {
    unsigned int r = a % b;
    a = b;
    b = r;
  }
  return a;
}
int main() {
  std::cout << "Input numbers:";
  unsigned int a, b;
  std::cin >> a >> b;
  std::cout << "rec gcd(" << a << ", " << b << ")=" << gcd(a, b) << std::endl;
  std::cout << "    gcd(" << a << ", " << b << ")=" << gcd2(a, b) << std::endl;
}
```

### 3. Least common multiple


```c++

#include <iostream>
#include <numeric>
#include <vector>
int gcd(int const a, int const b) { return b == 0 ? a : gcd(b, a % b); }
int lcm(int const a, int const b) {
  int h = gcd(a, b);
  return h ? (a * (b / h)) : 0;
}
template <class InputIt> int lcmr(InputIt first, InputIt last) {
  return std::accumulate(first, last, 1, lcm);
}
int main() {
  int n = 0;
  std::cout << "Input count:";
  std::cin >> n;
  std::vector<int> numbers;
  for (int i = 0; i < n; ++i) {
    int v{0};
    std::cin >> v;
    numbers.push_back(v);
  }
  std::cout << "lcm=" << lcmr(std::begin(numbers), std::end(numbers))
            << std::endl;
}
```

### 4. Largest prime smaller than given number


```c++

#include <iostream>
bool is_prime(int const num) {
  if (num <= 3) {
    return num > 1;
  } else if (num % 2 == 0 || num % 3 == 0) {
    return false;
  } else {
    for (int i = 5; i * i <= num; i += 6) {
      if (num % i == 0 || num % (i + 2) == 0) {
        return false;
      }
    }
    return true;
  }
}
int main() {
  int limit = 0;
  std::cout << "Upper limit:";
  std::cin >> limit;
  for (int i = limit; i > 1; i--) {
    if (is_prime(i)) {
      std::cout << "Largest prime:" << i << std::endl;
      return 0;
    }
  }
}
```

### 5. Sexy prime pairs


```c++

#include <iostream>
bool is_prime(int const num) {
  if (num <= 3) {
    return num > 1;
  } else if (num % 2 == 0 || num % 3 == 0) {
    return false;
  } else {
    for (int i = 5; i * i <= num; i += 6) {
      if (num % i == 0 || num % (i + 2) == 0) {
        return false;
      }
    }
    return true;
  }
}
int main() {
  int limit = 0;
  std::cout << "Upper limit:";
  std::cin >> limit;
  for (int n = 2; n <= limit; n++) {
    if (is_prime(n) && is_prime(n + 6)) {
      std::cout << n << "," << n + 6 << std::endl;
    }
  }
}
```

### 6. Abundant numbers


```c++

#include <iostream>
#include <cmath>
int sum_proper_divisors(int const number) {
  int result = 1;
  for (int i = 2; i <= std::sqrt(number); i++) {
    if (number % i == 0) {
      result += (i == (number / i)) ? i : (i + number / i);
    }
  }
  return result;
}
void print_abundant(int const limit) {
  for (int number = 10; number <= limit; ++number) {
    auto sum = sum_proper_divisors(number);
    if (sum > number) {
      std::cout << number << ", abundance=" << sum - number << std::endl;
    }
  }
}
int main() {
  int limit = 0;
  std::cout << "Upper limit:";
  std::cin >> limit;
  print_abundant(limit);
}
```

### 7. Amicable numbers


```c++

#include <iostream>
#include <set>
#include <cmath>
int sum_proper_divisors(int const number) {
  int result = 1;
  for (int i = 2; i <= std::sqrt(number); i++) {
    if (number % i == 0) {
      result += (i == (number / i)) ? i : (i + number / i);
    }
  }
  return result;
}
void print_amicables(int const limit) {
  for (int number = 4; number < limit; ++number) {
    auto sum1 = sum_proper_divisors(number);
    if (sum1 < limit) {
      auto sum2 = sum_proper_divisors(sum1);
      if (sum2 == number && number != sum1) {
        std::cout << number << "," << sum1 << std::endl;
      }
    }
  }
}
void print_amicables_once(int const limit) {
  std::set<int> printed;
  for (int number = 4; number < limit; ++number) {
    if (printed.find(number) != printed.end())
      continue;
    auto sum1 = sum_proper_divisors(number);
    if (sum1 < limit) {
      auto sum2 = sum_proper_divisors(sum1);
      if (sum2 == number && number != sum1) {
        printed.insert(number);
        printed.insert(sum1);
        std::cout << number << "," << sum1 << std::endl;
      }
    }
  }
}
int main() {
  print_amicables(1000000);
  print_amicables_once(1000000);
}
```

### 8. Armstrong numbers


```c++

#include <iostream>
#include <vector>
#include <numeric>
#include <cmath>
#include <chrono>
#include <functional>
template <typename Time = std::chrono::microseconds,
          typename Clock = std::chrono::high_resolution_clock>
struct perf_timer {
  template <typename F, typename... Args>
  static Time duration(F &&f, Args... args) {
    auto start = Clock::now();
    std::invoke(std::forward<F>(f), std::forward<Args>(args)...);
    auto end = Clock::now();
    return std::chrono::duration_cast<Time>(end - start);
  }
};
void print_narcissistics_1(bool const printResults) {
  for (int a = 1; a <= 9; a++) {
    for (int b = 0; b <= 9; b++) {
      for (int c = 0; c <= 9; c++) {
        auto abc = a * 100 + b * 10 + c;
        auto arm = a * a * a + b * b * b + c * c * c;
        if (abc == arm) {
          if (printResults)
            std::cout << arm << std::endl;
        }
      }
    }
  }
}
void print_narcissistics_2(bool const printResults) {
  for (int i = 100; i <= 1000; ++i) {
    int arm = 0;
    int n = i;
    while (n > 0) {
      auto d = n % 10;
      n = n / 10;
      arm += d * d * d;
    }
    if (i == arm) {
      if (printResults)
        std::cout << arm << std::endl;
    }
  }
}
void print_narcissistics_3(int const limit, bool const printResults) {
  for (int i = 1; i <= limit; ++i) {
    std::vector<int> digits;
    int n = i;
    while (n > 0) {
      digits.push_back(n % 10);
      n = n / 10;
    }
    int arm =
        std::accumulate(std::begin(digits), std::end(digits), 0,
                        [s = digits.size()](int const sum, int const digit) {
                          return sum + static_cast<int>(std::pow(digit, s));
                        });
    if (i == arm) {
      if (printResults)
        std::cout << arm << std::endl;
    }
  }
}
int main() {
  print_narcissistics_1(true);
  print_narcissistics_2(true);
  print_narcissistics_3(1000, true);
  auto t1 = perf_timer<>::duration([]() {
    for (int i = 0; i < 10000; ++i)
      print_narcissistics_1(false);
  });
  std::cout << std::chrono::duration<double, std::milli>(t1).count() << "ms"
            << std::endl;
  auto t2 = perf_timer<>::duration([]() {
    for (int i = 0; i < 10000; ++i)
      print_narcissistics_2(false);
  });
  std::cout << std::chrono::duration<double, std::milli>(t2).count() << "ms"
            << std::endl;
  auto t3 = perf_timer<>::duration([]() {
    for (int i = 0; i < 10000; ++i)
      print_narcissistics_3(1000, false);
  });
  std::cout << std::chrono::duration<double, std::milli>(t3).count() << "ms"
            << std::endl;
}
```

### 9. Prime factors of a number


```c++

#include <iostream>
#include <cmath>
#include <vector>
#include <iterator>
#include <algorithm>
std::vector<unsigned long long> prime_factors(unsigned long long n) {
  std::vector<unsigned long long> factors;
  while (n % 2 == 0) {
    factors.push_back(2);
    n = n / 2;
  }
  for (unsigned long long i = 3; i <= std::sqrt(n); i += 2) {
    while (n % i == 0) {
      factors.push_back(i);
      n = n / i;
    }
  }
  if (n > 2)
    factors.push_back(n);
  return factors;
}
int main() {
  unsigned long long number = 0;
  std::cout << "number:";
  std::cin >> number;
  auto factors = prime_factors(number);
  std::copy(std::begin(factors), std::end(factors),
            std::ostream_iterator<unsigned long long>(std::cout, " "));
}
```
### 10. Gray code


```c++

#include <iostream>
#include <bitset>
#include <string>
unsigned int gray_encode(unsigned int const num) { return num ^ (num >> 1); }
unsigned int gray_decode(unsigned int gray) {
  for (unsigned int bit = 1U << 31; bit > 1; bit >>= 1) {
    if (gray & bit)
      gray ^= bit >> 1;
  }
  return gray;
}
std::string to_binary(unsigned int value, int const digits) {
  return std::bitset<32>(value).to_string().substr(32 - digits, digits);
}
int main() {
  std::cout << "Number\tBinary\tGray\tDecoded\n";
  std::cout << "------\t------\t----\t-------\n";
  for (unsigned int n = 0; n < 32; ++n) {
    auto encg = gray_encode(n);
    auto decg = gray_decode(encg);
    std::cout << n << "\t" << to_binary(n, 5) << "\t" << to_binary(encg, 5)
              << "\t" << decg << "\n";
  }
}
```

### 11. Converting numerical values to Roman


```c++

#include <iostream>
#include <string>
#include <vector>
std::string to_roman(unsigned int value) {
  std::vector<std::pair<unsigned int, char const *>> roman{
      {1000, "M"}, {900, "CM"}, {500, "D"}, {400, "CD"}, {100, "C"},
      {90, "XC"},  {50, "L"},   {40, "XL"}, {10, "X"},   {9, "IX"},
      {5, "V"},    {4, "IV"},   {1, "I"}};
  std::string result;
  for (auto const &kvp : roman) {
    while (value >= kvp.first) {
      result += kvp.second;
      value -= kvp.first;
    }
  }
  return result;
}
int main() {
  for (int i = 1; i <= 100; ++i) {
    std::cout << i << "\t" << to_roman(i) << std::endl;
  }
  int number = 0;
  std::cout << "number:";
  std::cin >> number;
  std::cout << to_roman(number) << std::endl;
}
```

### 12. Largest Collatz sequence


```c++

#include <iostream>
#include <vector>
std::pair<unsigned long long, long>
longest_collatz_uncached(unsigned long long const limit) {
  long length = 0;
  unsigned long long number = 0;
  for (unsigned long long i = 2; i <= limit; i++) {
    auto n = i;
    long steps = 0;
    while (n != 1) {
      if ((n % 2) == 0)
        n = n / 2;
      else
        n = n * 3 + 1;
      steps++;
    }
    if (steps > length) {
      length = steps;
      number = i;
    }
  }
  return std::make_pair(number, length);
}
std::pair<unsigned long long, long>
longest_collatz(unsigned long long const limit) {
  long length = 0;
  unsigned long long number = 0;
  std::vector<int> cache(limit + 1, 0);
  for (unsigned long long i = 2; i <= limit; i++) {
    auto n = i;
    long steps = 0;
    while (n != 1 && n >= i) {
      if ((n % 2) == 0)
        n = n / 2;
      else
        n = n * 3 + 1;
      steps++;
    }
    cache[i] = steps + cache[n];
    if (cache[i] > length) {
      length = cache[i];
      number = i;
    }
  }
  return std::make_pair(number, length);
}
int main() {
  struct test_data {
    unsigned long long limit;
    unsigned long long start;
    long steps;
  };
  std::vector<test_data> data{{10ULL, 9ULL, 19},
                              {100ULL, 97ULL, 118},
                              {1000ULL, 871ULL, 178},
                              {10000ULL, 6171ULL, 263},
                              {100000ULL, 77031ULL, 350},
                              {1000000ULL, 837799ULL, 524},
                              {10000000ULL, 8400511ULL, 685},
                              {100000000ULL, 63728127ULL, 949}};
  for (auto const &d : data) {
    auto result = longest_collatz(d.limit);
    if (result.first != d.start || result.second != d.steps)
      std::cout << "error on limit " << d.limit << std::endl;
    else
      std::cout << "less than      : " << d.limit << std::endl
                << "starting number: " << result.first << std::endl
                << "sequence length: " << result.second << std::endl;
  }
}
```

### 13. Computing the value of Pi


```c++

#include <iostream>
#include <random>
#include <algorithm>
#include <array>
#include <functional>
template <typename E = std::mt19937,
          typename D = std::uniform_real_distribution<>>
double compute_pi(E &engine, D &dist, int const samples = 1000000) {
  auto hit = 0;
  for (auto i = 0; i < samples; i++) {
    auto x = dist(engine);
    auto y = dist(engine);
    if (y <= std::sqrt(1 - std::pow(x, 2)))
      hit += 1;
  }
  return 4.0 * hit / samples;
}
int main() {
  std::random_device rd;
  auto seed_data = std::array<int, std::mt19937::state_size>{};
  std::generate(std::begin(seed_data), std::end(seed_data), std::ref(rd));
  std::seed_seq seq(std::begin(seed_data), std::end(seed_data));
  auto eng = std::mt19937{seq};
  auto dist = std::uniform_real_distribution<>{0, 1};
  for (auto j = 0; j < 10; j++) {
    std::cout << compute_pi(eng, dist) << std::endl;
  }
}
```

### 14. Validating ISBNs


```c++

#include <iostream>
#include <string>
#include <algorithm>
#include <numeric>
#include <string_view>
#include <assert.h>
bool validate_isbn_10(std::string_view isbn) {
  auto valid = false;
  if (isbn.size() == 10 &&
      std::count_if(std::begin(isbn), std::end(isbn), isdigit) == 10) {
    auto w = 10;
    auto sum = std::accumulate(std::begin(isbn), std::end(isbn), 0,
                               [&w](int const total, char const c) {
                                 return total + w-- * (c - '0');
                               });
    valid = !(sum % 11);
  }
  return valid;
}
int main() {
  assert(validate_isbn_10("0306406152"));
  assert(!validate_isbn_10("0306406151"));
  std::string isbn;
  std::cout << "isbn:";
  std::cin >> isbn;
  std::cout << "valid: " << validate_isbn_10(isbn) << std::endl;
}
```


## Language Features

### 15. IPv4 data type


```c++

#include <iostream>
#include <array>
#include <sstream>
#include <assert.h>
class ipv4 {
  std::array<unsigned char, 4> data;
public:
  constexpr ipv4() : data{{0}} {}
  constexpr ipv4(unsigned char const a, unsigned char const b,
                 unsigned char const c, unsigned char const d)
      : data{{a, b, c, d}} {}
  explicit constexpr ipv4(unsigned long a)
      : data{{static_cast<unsigned char>((a >> 24) & 0xFF),
              static_cast<unsigned char>((a >> 16) & 0xFF),
              static_cast<unsigned char>((a >> 8) & 0xFF),
              static_cast<unsigned char>(a & 0xFF)}} {}
  ipv4(ipv4 const &other) noexcept : data(other.data) {}
  ipv4 &operator=(ipv4 const &other) noexcept {
    data = other.data;
    return *this;
  }
  std::string to_string() const {
    std::stringstream sstr;
    sstr << *this;
    return sstr.str();
  }
  constexpr unsigned long to_ulong() const noexcept {
    return (static_cast<unsigned long>(data[0]) << 24) |
           (static_cast<unsigned long>(data[1]) << 16) |
           (static_cast<unsigned long>(data[2]) << 8) |
           static_cast<unsigned long>(data[3]);
  }
  friend std::ostream &operator<<(std::ostream &os, const ipv4 &a) {
    os << static_cast<int>(a.data[0]) << '.' << static_cast<int>(a.data[1])
       << '.' << static_cast<int>(a.data[2]) << '.'
       << static_cast<int>(a.data[3]);
    return os;
  }
  friend std::istream &operator>>(std::istream &is, ipv4 &a) {
    char d1, d2, d3;
    int b1, b2, b3, b4;
    is >> b1 >> d1 >> b2 >> d2 >> b3 >> d3 >> b4;
    if (d1 == '.' && d2 == '.' && d3 == '.')
      a = ipv4(b1, b2, b3, b4);
    else
      is.setstate(std::ios_base::failbit);
    return is;
  }
};
int main() {
  ipv4 a(168, 192, 0, 1);
  std::cout << a << std::endl;
  std::cout << a.to_string() << std::endl;
  ipv4 b = a;
  ipv4 c;
  c = b;
  ipv4 ip;
  std::cout << ip << std::endl;
  std::cin >> ip;
  if (!std::cin.fail())
    std::cout << ip << std::endl;
}
```

### 16. Enumerating IPv4 addresses in a range


```c++

#include <iostream>
#include <array>
#include <sstream>
class ipv4 {
  std::array<unsigned char, 4> data;
public:
  constexpr ipv4() : data{{0}} {}
  constexpr ipv4(unsigned char const a, unsigned char const b,
                 unsigned char const c, unsigned char const d)
      : data{{a, b, c, d}} {}
  explicit constexpr ipv4(unsigned long a)
      : data{{static_cast<unsigned char>((a >> 24) & 0xFF),
              static_cast<unsigned char>((a >> 16) & 0xFF),
              static_cast<unsigned char>((a >> 8) & 0xFF),
              static_cast<unsigned char>(a & 0xFF)}} {}
  ipv4(ipv4 const &other) noexcept : data(other.data) {}
  ipv4 &operator=(ipv4 const &other) noexcept {
    data = other.data;
    return *this;
  }
  constexpr unsigned long to_ulong() const noexcept {
    return (static_cast<unsigned long>(data[0]) << 24) |
           (static_cast<unsigned long>(data[1]) << 16) |
           (static_cast<unsigned long>(data[2]) << 8) |
           static_cast<unsigned long>(data[3]);
  }
  std::string to_string() const {
    std::stringstream sstr;
    sstr << *this;
    return sstr.str();
  }
  constexpr bool is_loopback() const noexcept {
    return (to_ulong() & 0xFF000000) == 0x7F000000;
  }
  constexpr bool is_unspecified() const noexcept { return to_ulong() == 0; }
  constexpr bool is_class_a() const noexcept {
    return (to_ulong() & 0x80000000) == 0;
  }
  constexpr bool is_class_b() const noexcept {
    return (to_ulong() & 0xC0000000) == 0x80000000;
  }
  constexpr bool is_class_c() const noexcept {
    return (to_ulong() & 0xE0000000) == 0xC0000000;
  }
  constexpr bool is_multicast() const noexcept {
    return (to_ulong() & 0xF0000000) == 0xE0000000;
  }
  ipv4 &operator++() {
    *this = ipv4(1 + to_ulong());
    return *this;
  }
  ipv4 &operator++(int) {
    ipv4 result(*this);
    ++(*this);
    return *this;
  }
  friend bool operator==(ipv4 const &a1, ipv4 const &a2) noexcept {
    return a1.data == a2.data;
  }
  friend bool operator!=(ipv4 const &a1, ipv4 const &a2) noexcept {
    return !(a1 == a2);
  }
  friend bool operator<(ipv4 const &a1, ipv4 const &a2) noexcept {
    return a1.to_ulong() < a2.to_ulong();
  }
  friend bool operator>(ipv4 const &a1, ipv4 const &a2) noexcept {
    return a2 < a1;
  }
  friend bool operator<=(ipv4 const &a1, ipv4 const &a2) noexcept {
    return !(a1 > a2);
  }
  friend bool operator>=(ipv4 const &a1, ipv4 const &a2) noexcept {
    return !(a1 < a2);
  }
  friend std::ostream &operator<<(std::ostream &os, const ipv4 &a) {
    os << static_cast<int>(a.data[0]) << '.' << static_cast<int>(a.data[1])
       << '.' << static_cast<int>(a.data[2]) << '.'
       << static_cast<int>(a.data[3]);
    return os;
  }
  friend std::istream &operator>>(std::istream &is, ipv4 &a) {
    char d1, d2, d3;
    int b1, b2, b3, b4;
    is >> b1 >> d1 >> b2 >> d2 >> b3 >> d3 >> b4;
    if (d1 == '.' && d2 == '.' && d3 == '.')
      a = ipv4(b1, b2, b3, b4);
    else
      is.setstate(std::ios_base::failbit);
    return is;
  }
};
int main() {
  std::cout << "input range: ";
  ipv4 a1, a2;
  std::cin >> a1 >> a2;
  if (a2 > a1) {
    for (ipv4 a = a1; a <= a2; a++) {
      std::cout << a << std::endl;
    }
  } else {
    std::cerr << "invalid range!" << std::endl;
  }
}
```

### 17. Creating a 2D array with basic operations


```c++

#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>
template <class T, size_t R, size_t C> class array2d {
  typedef T value_type;
  typedef value_type *iterator;
  typedef value_type const *const_iterator;
  std::vector<T> arr;
public:
  array2d() : arr(R * C) {}
  explicit array2d(std::initializer_list<T> l) : arr(l) {}
  constexpr T *data() noexcept { return arr.data(); }
  constexpr T const *data() const noexcept { return arr.data(); }
  constexpr T &at(size_t const r, size_t const c) { return arr.at(r * C + c); }
  constexpr T const &at(size_t const r, size_t const c) const {
    return arr.at(r * C + c);
  }
  constexpr T &operator()(size_t const r, size_t const c) {
    return arr[r * C + c];
  }
  constexpr T const &operator()(size_t const r, size_t const c) const {
    return arr[r * C + c];
  }
  constexpr bool empty() const noexcept { return R == 0 || C == 0; }
  constexpr size_t size(int const rank) const {
    if (rank == 1)
      return R;
    else if (rank == 2)
      return C;
    throw std::out_of_range("Rank is out of range!");
  }
  void fill(T const &value) {
    std::fill(std::begin(arr), std::end(arr), value);
  }
  void swap(array2d &other) noexcept { arr.swap(other.arr); }
  const_iterator begin() const { return arr.data(); }
  const_iterator end() const { return arr.data() + arr.size(); }
  iterator begin() { return arr.data(); }
  iterator end() { return arr.data() + arr.size(); }
};
template <class T, size_t R, size_t C>
void print_array2d(array2d<T, R, C> const &arr) {
  for (int i = 0; i < R; ++i) {
    for (int j = 0; j < C; ++j) {
      std::cout << arr.at(i, j) << ' ';
    }
    std::cout << std::endl;
  }
}
int main() {
  {
    std::cout << "test fill" << std::endl;
    array2d<int, 2, 3> a;
    a.fill(1);
    print_array2d(a);
  }
  {
    std::cout << "test operator()" << std::endl;
    array2d<int, 2, 3> a;
    for (size_t i = 0; i < a.size(1); ++i) {
      for (size_t j = 0; j < a.size(2); ++j) {
        a(i, j) = 1 + i * 3 + j;
      }
    }
    print_array2d(a);
  }
  {
    std::cout << "test move semantics" << std::endl;
    array2d<int, 2, 3> a{10, 20, 30, 40, 50, 60};
    print_array2d(a);
    array2d<int, 2, 3> b(std::move(a));
    print_array2d(b);
  }
  {
    std::cout << "test swap" << std::endl;
    array2d<int, 2, 3> a{1, 2, 3, 4, 5, 6};
    array2d<int, 2, 3> b{10, 20, 30, 40, 50, 60};
    print_array2d(a);
    print_array2d(b);
    a.swap(b);
    print_array2d(a);
    print_array2d(b);
  }
  {
    std::cout << "test capacity" << std::endl;
    array2d<int, 2, 3> const a{1, 2, 3, 4, 5, 6};
    for (size_t i = 0; i < a.size(1); ++i) {
      for (size_t j = 0; j < a.size(2); ++j) {
        std::cout << a(i, j) << ' ';
      }
      std::cout << std::endl;
    }
  }
  {
    std::cout << "test iterators" << std::endl;
    array2d<int, 2, 3> const a{1, 2, 3, 4, 5, 6};
    for (auto const e : a) {
      std::cout << e << ' ';
    }
    std::cout << std::endl;
    std::copy(std::begin(a), std::end(a),
              std::ostream_iterator<int>(std::cout, " "));
    std::cout << std::endl;
  }
}
```

### 18. Minimum function with any number of arguments


```c++

#include <iostream>
#include <functional>
template <typename T> T minimum(T const a, T const b) { return a < b ? a : b; }
template <typename T1, typename... T> T1 minimum(T1 a, T... args) {
  return minimum(a, minimum(args...));
}
template <class Compare, typename T>
T minimumc(Compare comp, T const a, T const b) {
  return comp(a, b) ? a : b;
}
template <class Compare, typename T1, typename... T>
T1 minimumc(Compare comp, T1 a, T... args) {
  return minimumc(comp, a, minimumc(comp, args...));
}
int main() {
  auto x = minimum(5, 4, 2, 3);
  std::cout << x << std::endl;
  auto y = minimumc(std::less<>(), 3, 2, 1, 0);
  std::cout << y << std::endl;
}
```

### 19. Adding a range of values to a container


```c++

#include <iostream>
#include <cstdlib>
#include <vector>
#include <iterator>
#include <list>
template <typename C, typename... Args> void push_back(C &c, Args &&... args) {
  (c.push_back(args), ...);
}
int main() {
  std::vector<int> v;
  push_back(v, 1, 2, 3, 4);
  std::copy(std::begin(v), std::end(v),
            std::ostream_iterator<int>(std::cout, " "));
  std::list<int> l;
  push_back(l, 1, 2, 3, 4);
  std::copy(std::begin(l), std::end(l),
            std::ostream_iterator<int>(std::cout, " "));
}
```

### 20. Container any, all, none


```c++

#include <iostream>
#include <vector>
#include <array>
#include <list>
#include <assert.h>
#include <functional>
template <class C, class T> bool contains(C const &c, T const &value) {
  return std::end(c) != std::find(std::begin(c), std::end(c), value);
}
template <class C, class... T> bool contains_any(C const &c, T &&... value) {
  return (... || contains(c, value));
}
template <class C, class... T> bool contains_all(C const &c, T &&... value) {
  return (... && contains(c, value));
}
template <class C, class... T> bool contains_none(C const &c, T &&... value) {
  return !contains_any(c, std::forward<T>(value)...);
}
int main() {
  std::vector<int> v{1, 2, 3, 4, 5, 6};
  std::array<int, 6> a{{1, 2, 3, 4, 5, 6}};
  std::list<int> l{1, 2, 3, 4, 5, 6};
  assert(contains(v, 3));
  assert(contains(a, 3));
  assert(contains(l, 3));
  assert(!contains(v, 30));
  assert(!contains(v, 30));
  assert(!contains(v, 30));
  assert(contains_any(v, 0, 3, 30));
  assert(contains_any(a, 0, 3, 30));
  assert(contains_any(l, 0, 3, 30));
  assert(!contains_any(v, 0, 30));
  assert(!contains_any(a, 0, 30));
  assert(!contains_any(l, 0, 30));
  assert(contains_all(v, 1, 3, 6));
  assert(contains_all(a, 1, 3, 6));
  assert(contains_all(l, 1, 3, 6));
  assert(!contains_all(v, 0, 1));
  assert(!contains_all(a, 0, 1));
  assert(!contains_all(l, 0, 1));
  assert(contains_none(v, 0, 7));
  assert(contains_none(a, 0, 7));
  assert(contains_none(l, 0, 7));
  assert(!contains_none(v, 0, 6, 7));
  assert(!contains_none(a, 0, 6, 7));
  assert(!contains_none(l, 0, 6, 7));
}
```

### 21. System handle wrapper


```c++

#ifdef _WIN32
#include <windows.h>
#else
typedef void *HANDLE;
#define DWORD unsigned long
#ifdef _LP64
#define LONG_PTR long long
#define ULONG_PTR unsigned long long
#else
#define LONG_PTR long
#define ULONG_PTR unsigned long
#endif
#define INVALID_HANDLE_VALUE ((HANDLE)(LONG_PTR)-1)
struct SECURITY_ATTRIBUTES {
  DWORD nLength;
  void *lpSecurityDescriptor;
  int bInheritHandle;
};
struct OVERLAPPED {
  ULONG_PTR Internal;
  ULONG_PTR InternalHigh;
  union {
    struct {
      DWORD Offset;
      DWORD OffsetHigh;
    } DUMMYSTRUCTNAME;
    void *Pointer;
  } DUMMYUNIONNAME;
  HANDLE hEvent;
};
int CloseHandle(HANDLE hObject) { return 0; }
HANDLE CreateFile(char const *, DWORD, DWORD, SECURITY_ATTRIBUTES *, DWORD,
                  DWORD, HANDLE) {
  return INVALID_HANDLE_VALUE;
}
int ReadFile(HANDLE, void *, DWORD, DWORD *, OVERLAPPED *) { return 0; }
#define GENERIC_READ 0x80000000L
#define GENERIC_WRITE 0x40000000L
#define CREATE_NEW 1
#define CREATE_ALWAYS 2
#define OPEN_EXISTING 3
#define OPEN_ALWAYS 4
#define TRUNCATE_EXISTING 5
#define FILE_SHARE_READ 1
#define FILE_ATTRIBUTE_NORMAL 0x00000080
#endif
#include <algorithm>
#include <vector>
#include <stdexcept> // 'runtime_error' is not a member of 'std'
template <typename Traits> class unique_handle {
  using pointer = typename Traits::pointer;
  pointer m_value;
public:
  unique_handle(unique_handle const &) = delete;
  unique_handle &operator=(unique_handle const &) = delete;
  explicit unique_handle(pointer value = Traits::invalid()) noexcept
      : m_value{value} {}
  unique_handle(unique_handle &&other) noexcept : m_value{other.release()} {}
  unique_handle &operator=(unique_handle &&other) noexcept {
    if (this != &other) {
      reset(other.release());
    }
    return *this;
  }
  ~unique_handle() noexcept { Traits::close(m_value); }
  explicit operator bool() const noexcept {
    return m_value != Traits::invalid();
  }
  pointer get() const noexcept { return m_value; }
  pointer release() noexcept {
    auto value = m_value;
    m_value = Traits::invalid();
    return value;
  }
  bool reset(pointer value = Traits::invalid()) noexcept {
    if (m_value != value) {
      Traits::close(m_value);
      m_value = value;
    }
    return static_cast<bool>(*this);
  }
  void swap(unique_handle<Traits> &other) noexcept {
    std::swap(m_value, other.m_value);
  }
};
template <typename Traits>
void swap(unique_handle<Traits> &left, unique_handle<Traits> &right) noexcept {
  left.swap(right);
}
template <typename Traits>
bool operator==(unique_handle<Traits> const &left,
                unique_handle<Traits> const &right) noexcept {
  return left.get() == right.get();
}
template <typename Traits>
bool operator!=(unique_handle<Traits> const &left,
                unique_handle<Traits> const &right) noexcept {
  return left.get() != right.get();
}
struct null_handle_traits {
  using pointer = HANDLE;
  static pointer invalid() noexcept { return nullptr; }
  static void close(pointer value) noexcept { CloseHandle(value); }
};
struct invalid_handle_traits {
  using pointer = HANDLE;
  static pointer invalid() noexcept { return INVALID_HANDLE_VALUE; }
  static void close(pointer value) noexcept { CloseHandle(value); }
};
using null_handle = unique_handle<null_handle_traits>;
using invalid_handle = unique_handle<invalid_handle_traits>;
void function_that_throws() {
  throw std::runtime_error("an error has occurred");
}
void bad_handle_example() {
  bool condition1 = false;
  bool condition2 = true;
  HANDLE handle =
      CreateFile("sample.txt", GENERIC_READ, FILE_SHARE_READ, nullptr,
                 OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, nullptr);
  if (handle == INVALID_HANDLE_VALUE)
    return;
  if (condition1) {
    CloseHandle(handle);
    return;
  }
  std::vector<char> buffer(1024);
  unsigned long bytesRead = 0;
  ReadFile(handle, buffer.data(), buffer.size(), &bytesRead, nullptr);
  if (condition2) {
    return;
  }
  function_that_throws();
  CloseHandle(handle);
}
void good_handle_example() {
  bool condition1 = false;
  bool condition2 = true;
  invalid_handle handle{CreateFile("sample.txt", GENERIC_READ, FILE_SHARE_READ,
                                   nullptr, OPEN_EXISTING,
                                   FILE_ATTRIBUTE_NORMAL, nullptr)};
  if (!handle)
    return;
  if (condition1)
    return;
  std::vector<char> buffer(1024);
  unsigned long bytesRead = 0;
  ReadFile(handle.get(), buffer.data(), buffer.size(), &bytesRead, nullptr);
  if (condition2)
    return;
  function_that_throws();
}
int main() {
  try {
    bad_handle_example();
  } catch (...) {
  }
  try {
    good_handle_example();
  } catch (...) {
  }
  return 0;
}
```

### 22. Literals of various temperature scales


```c++

#include <cmath>
#include <assert.h>
bool are_equal(double const d1, double const d2, double const epsilon = 0.001) {
  return std::fabs(d1 - d2) < epsilon;
}
namespace temperature {
enum class scale { celsius, fahrenheit, kelvin };
template <scale S> class quantity {
  const double amount;
public:
  constexpr explicit quantity(double const a) : amount(a) {}
  explicit operator double() const { return amount; }
};
template <scale S>
inline bool operator==(quantity<S> const &lhs, quantity<S> const &rhs) {
  return are_equal(static_cast<double>(lhs), static_cast<double>(rhs));
}
template <scale S>
inline bool operator!=(quantity<S> const &lhs, quantity<S> const &rhs) {
  return !(lhs == rhs);
}
template <scale S>
inline bool operator<(quantity<S> const &lhs, quantity<S> const &rhs) {
  return static_cast<double>(lhs) < static_cast<double>(rhs);
}
template <scale S>
inline bool operator>(quantity<S> const &lhs, quantity<S> const &rhs) {
  return rhs < lhs;
}
template <scale S>
inline bool operator<=(quantity<S> const &lhs, quantity<S> const &rhs) {
  return !(lhs > rhs);
}
template <scale S>
inline bool operator>=(quantity<S> const &lhs, quantity<S> const &rhs) {
  return !(lhs < rhs);
}
template <scale S>
constexpr quantity<S> operator+(quantity<S> const &q1, quantity<S> const &q2) {
  return quantity<S>(static_cast<double>(q1) + static_cast<double>(q2));
}
template <scale S>
constexpr quantity<S> operator-(quantity<S> const &q1, quantity<S> const &q2) {
  return quantity<S>(static_cast<double>(q1) - static_cast<double>(q2));
}
template <scale S, scale R> struct conversion_traits {
  static double convert(double const value) = delete;
};
template <> struct conversion_traits<scale::celsius, scale::kelvin> {
  static double convert(double const value) { return value + 273.15; }
};
template <> struct conversion_traits<scale::kelvin, scale::celsius> {
  static double convert(double const value) { return value - 273.15; }
};
template <> struct conversion_traits<scale::celsius, scale::fahrenheit> {
  static double convert(double const value) {
    return (value * 9) / 5 + 32;
    ;
  }
};
template <> struct conversion_traits<scale::fahrenheit, scale::celsius> {
  static double convert(double const value) { return (value - 32) * 5 / 9; }
};
template <> struct conversion_traits<scale::fahrenheit, scale::kelvin> {
  static double convert(double const value) { return (value + 459.67) * 5 / 9; }
};
template <> struct conversion_traits<scale::kelvin, scale::fahrenheit> {
  static double convert(double const value) { return (value * 9) / 5 - 459.67; }
};
template <scale R, scale S>
constexpr quantity<R> temperature_cast(quantity<S> const q) {
  return quantity<R>(conversion_traits<S, R>::convert(static_cast<double>(q)));
}
namespace temperature_scale_literals {
constexpr quantity<scale::celsius> operator"" _deg(long double const amount) {
  return quantity<scale::celsius>{static_cast<double>(amount)};
}
constexpr quantity<scale::fahrenheit> operator"" _f(long double const amount) {
  return quantity<scale::fahrenheit>{static_cast<double>(amount)};
}
constexpr quantity<scale::kelvin> operator"" _k(long double const amount) {
  return quantity<scale::kelvin>{static_cast<double>(amount)};
}
} // namespace temperature_scale_literals
} // namespace temperature
int main() {
  using namespace temperature;
  using namespace temperature_scale_literals;
  auto t1{36.5_deg};
  auto t2{79.0_f};
  auto t3{100.0_k};
  {
    auto tf = temperature_cast<scale::fahrenheit>(t1);
    auto tc = temperature_cast<scale::celsius>(tf);
    assert(t1 == tc);
  }
  {
    auto tk = temperature_cast<scale::kelvin>(t1);
    auto tc = temperature_cast<scale::celsius>(tk);
    assert(t1 == tc);
  }
  {
    auto tc = temperature_cast<scale::celsius>(t2);
    auto tf = temperature_cast<scale::fahrenheit>(tc);
    assert(t2 == tf);
  }
  {
    auto tk = temperature_cast<scale::kelvin>(t2);
    auto tf = temperature_cast<scale::fahrenheit>(tk);
    assert(t2 == tf);
  }
  {
    auto tc = temperature_cast<scale::celsius>(t3);
    auto tk = temperature_cast<scale::kelvin>(tc);
    assert(t3 == tk);
  }
  {
    auto tf = temperature_cast<scale::fahrenheit>(t3);
    auto tk = temperature_cast<scale::kelvin>(tf);
    assert(t3 == tk);
  }
}
```

## Strings and Regular Expressions

### 23. Binary to string conversion


```c++

#include <string>
#include <sstream>
#include <iomanip>
#include <vector>
#include <array>
#include <assert.h>
template <typename Iter>
std::string bytes_to_hexstr(Iter begin, Iter end,
                            bool const uppercase = false) {
  std::ostringstream oss;
  if (uppercase)
    oss.setf(std::ios_base::uppercase);
  for (; begin != end; ++begin)
    oss << std::hex << std::setw(2) << std::setfill('0')
        << static_cast<int>(*begin);
  return oss.str();
}
template <typename C>
std::string bytes_to_hexstr(C const &c, bool const uppercase = false) {
  return bytes_to_hexstr(std::cbegin(c), std::cend(c), uppercase);
}
int main() {
  std::vector<unsigned char> v{0xBA, 0xAD, 0xF0, 0x0D};
  std::array<unsigned char, 6> a{{1, 2, 3, 4, 5, 6}};
  unsigned char buf[5] = {0x11, 0x22, 0x33, 0x44, 0x55};
  assert(bytes_to_hexstr(v, true) == "BAADF00D");
  assert(bytes_to_hexstr(a, true) == "010203040506");
  assert(bytes_to_hexstr(buf, true) == "1122334455");
  assert(bytes_to_hexstr(v) == "baadf00d");
  assert(bytes_to_hexstr(a) == "010203040506");
  assert(bytes_to_hexstr(buf) == "1122334455");
}
```

### 24. String to binary conversion


```c++

#include <string>
#include <string_view>
#include <sstream>
#include <vector>
#include <array>
#include <assert.h>
unsigned char hexchar_to_int(char const ch) {
  if (ch >= '0' && ch <= '9')
    return ch - '0';
  if (ch >= 'A' && ch <= 'F')
    return ch - 'A' + 10;
  if (ch >= 'a' && ch <= 'f')
    return ch - 'a' + 10;
  throw std::invalid_argument("Invalid hexadecimal character");
}
std::vector<unsigned char> hexstr_to_bytes(std::string_view str) {
  std::vector<unsigned char> result;
  for (size_t i = 0; i < str.size(); i += 2) {
    result.push_back((hexchar_to_int(str[i]) << 4) |
                     hexchar_to_int(str[i + 1]));
  }
  return result;
}
int main() {
  std::vector<unsigned char> expected{0xBA, 0xAD, 0xF0, 0x0D, 0x42};
  assert(hexstr_to_bytes("BAADF00D42") == expected);
  assert(hexstr_to_bytes("BaaDf00d42") == expected);
}
```

### 25. Capitalizing an article title


```c++

#include <string>
#include <sstream>
#include <cctype>
#include <assert.h>
template <class Elem>
using tstring =
    std::basic_string<Elem, std::char_traits<Elem>, std::allocator<Elem>>;
template <class Elem>
using tstringstream =
    std::basic_stringstream<Elem, std::char_traits<Elem>, std::allocator<Elem>>;
template <class Elem> tstring<Elem> capitalize(tstring<Elem> const &text) {
  tstringstream<Elem> result;
  bool newWord = true;
  for (auto const ch : text) {
    newWord = newWord || std::ispunct(ch) || std::isspace(ch);
    if (std::isalpha(ch)) {
      if (newWord) {
        result << static_cast<Elem>(std::toupper(ch));
        newWord = false;
      } else
        result << static_cast<Elem>(std::tolower(ch));
    } else
      result << ch;
  }
  return result.str();
}
int main() {
  using namespace std::string_literals;
  std::string text = "THIS IS an ExamplE, should wORk!";
  std::string expected = "This Is An Example, Should Work!";
  assert(expected == capitalize(text));
  assert(L"The C++ Challenger"s == capitalize(L"the c++ challenger"s));
  assert(L"This Is An Example, Should Work!"s ==
         capitalize(L"THIS IS an ExamplE, should wORk!"s));
}
```

### 26. Joining strings together separated by a delimiter


```c++

#include <string>
#include <iterator>
#include <sstream>
#include <vector>
#include <array>
#include <list>
#include <assert.h>
template <typename Iter>
std::string join_strings(Iter begin, Iter end, char const *const separator) {
  std::ostringstream os;
  std::copy(begin, end - 1, std::ostream_iterator<std::string>(os, separator));
  os << *(end - 1);
  return os.str();
}
template <typename C>
std::string join_strings(C const &c, char const *const separator) {
  if (c.size() == 0)
    return std::string{};
  return join_strings(std::begin(c), std::end(c), separator);
}
int main() {
  using namespace std::string_literals;
  std::vector<std::string> v1{"this", "is", "an", "example"};
  std::vector<std::string> v2{"example"};
  std::vector<std::string> v3{};
  assert(join_strings(v1, " ") == "this is an example"s);
  assert(join_strings(v2, " ") == "example"s);
  assert(join_strings(v3, " ") == ""s);
  std::array<std::string, 4> a1{{"this", "is", "an", "example"}};
  std::array<std::string, 1> a2{{"example"}};
  std::array<std::string, 0> a3{};
  assert(join_strings(a1, " ") == "this is an example"s);
  assert(join_strings(a2, " ") == "example"s);
  assert(join_strings(a3, " ") == ""s);
}
```

### 27. Splitting a string into tokens with a list of possible delimiters


```c++

#include <string>
#include <sstream>
#include <vector>
#include <assert.h>
template <class Elem>
using tstring =
    std::basic_string<Elem, std::char_traits<Elem>, std::allocator<Elem>>;
template <class Elem>
using tstringstream =
    std::basic_stringstream<Elem, std::char_traits<Elem>, std::allocator<Elem>>;
template <typename Elem>
inline std::vector<tstring<Elem>> split(tstring<Elem> text,
                                        Elem const delimiter) {
  auto sstr = tstringstream<Elem>{text};
  auto tokens = std::vector<tstring<Elem>>{};
  auto token = tstring<Elem>{};
  while (std::getline(sstr, token, delimiter)) {
    if (!token.empty())
      tokens.push_back(token);
  }
  return tokens;
}
template <typename Elem>
inline std::vector<tstring<Elem>> split(tstring<Elem> text,
                                        tstring<Elem> const &delimiters) {
  auto tokens = std::vector<tstring<Elem>>{};
  size_t pos, prev_pos = 0;
  while ((pos = text.find_first_of(delimiters, prev_pos)) !=
         std::string::npos) {
    if (pos > prev_pos)
      tokens.push_back(text.substr(prev_pos, pos - prev_pos));
    prev_pos = pos + 1;
  }
  if (prev_pos < text.length())
    tokens.push_back(text.substr(prev_pos, std::string::npos));
  return tokens;
}
int main() {
  using namespace std::string_literals;
  std::vector<std::string> expected{"this", "is", "a", "sample"};
  assert(expected == split("this is a sample"s, ' '));
  assert(expected == split("this,is a.sample!!"s, ",.! "s));
}
```

### 28. Longest palindromic substring


```c++

#include <string>
#include <string_view>
#include <vector>
#include <assert.h>
std::string longest_palindrome(std::string_view str) {
  size_t const len = str.size();
  size_t longestBegin = 0;
  size_t maxLen = 1;
  std::vector<bool> table(len * len, false);
  for (size_t i = 0; i < len; i++) {
    table[i * len + i] = true;
  }
  for (size_t i = 0; i < len - 1; i++) {
    if (str[i] == str[i + 1]) {
      table[i * len + i + 1] = true;
      if (maxLen < 2) {
        longestBegin = i;
        maxLen = 2;
      }
    }
  }
  for (size_t k = 3; k <= len; k++) {
    for (size_t i = 0; i < len - k + 1; i++) {
      size_t j = i + k - 1;
      if (str[i] == str[j] && table[(i + 1) * len + j - 1]) {
        table[i * len + j] = true;
        if (maxLen < k) {
          longestBegin = i;
          maxLen = k;
        }
      }
    }
  }
  return std::string(str.substr(longestBegin, maxLen));
}
int main() {
  using namespace std::string_literals;
  assert(longest_palindrome("sahararahnide") == "hararah");
  assert(longest_palindrome("level") == "level");
  assert(longest_palindrome("s") == "s");
  assert(longest_palindrome("aabbcc") == "aa");
  assert(longest_palindrome("abab") == "aba");
}
```

### 29. License plate validation


```c++

#include <string>
#include <string_view>
#include <regex>
#include <assert.h>
bool validate_license_plate_format(std::string_view str) {
  std::regex rx(R"([A-Z]{3}-[A-Z]{2} \d{3,4})");
  return std::regex_match(str.data(), rx);
}
std::vector<std::string> extract_license_plate_numbers(std::string const &str) {
  std::regex rx(R"(([A-Z]{3}-[A-Z]{2} \d{3,4})*)");
  std::smatch match;
  std::vector<std::string> results;
  for (auto i = std::sregex_iterator(std::cbegin(str), std::cend(str), rx);
       i != std::sregex_iterator(); ++i) {
    if ((*i)[1].matched)
      results.push_back(i->str());
  }
  return results;
}
int main() {
  assert(validate_license_plate_format("ABC-DE 123"));
  assert(validate_license_plate_format("ABC-DE 1234"));
  assert(!validate_license_plate_format("ABC-DE 12345"));
  assert(!validate_license_plate_format("abc-de 1234"));
  std::vector<std::string> expected{"AAA-AA 123", "ABC-DE 1234", "XYZ-WW 0001"};
  std::string text("AAA-AA 123qwe-ty 1234  ABC-DE 123456..XYZ-WW 0001");
  assert(expected == extract_license_plate_numbers(text));
}
```

### 30. Extracting URL parts


```c++

#include <string>
#include <string_view>
#include <regex>
#include <assert.h>
#ifdef USE_BOOST_OPTIONAL
#include <boost/optional.hpp>
using boost::optional;
#else
#include <optional>
using std::optional;
#endif
struct uri_parts {
  std::string protocol;
  std::string domain;
  optional<int> port;
  optional<std::string> path;
  optional<std::string> query;
  optional<std::string> fragment;
};
optional<uri_parts> parse_uri(std::string uri) {
  std::regex rx(
      R"(^(\w+):\/\/([\w.-]+)(:(\d+))?([\w\/\.]+)?(\?([\w=&]*)(#?(\w+))?)?$)");
  auto matches = std::smatch{};
  if (std::regex_match(uri, matches, rx)) {
    if (matches[1].matched && matches[2].matched) {
      uri_parts parts;
      parts.protocol = matches[1].str();
      parts.domain = matches[2].str();
      if (matches[4].matched)
        parts.port = std::stoi(matches[4]);
      if (matches[5].matched)
        parts.path = matches[5];
      if (matches[7].matched)
        parts.query = matches[7];
      if (matches[9].matched)
        parts.fragment = matches[9];
      return parts;
    }
  }
  return {};
}
int main() {
  auto p1 = parse_uri("https://packt.com");
  assert(p1);
  assert(p1->protocol == "https");
  assert(p1->domain == "packt.com");
  assert(!p1->port);
  assert(!p1->path);
  assert(!p1->query);
  assert(!p1->fragment);
  auto p2 = parse_uri("https://bbc.com:80/en/index.html?lite=true#ui");
  assert(p2);
  assert(p2->protocol == "https");
  assert(p2->domain == "bbc.com");
  assert(p2->port == 80);
  assert(p2->path.value() == "/en/index.html");
  assert(p2->query.value() == "lite=true");
  assert(p2->fragment.value() == "ui");
}
```

### 31. Transforming dates in strings


```c++

#include <string>
#include <string_view>
#include <regex>
#include <assert.h>
std::string transform_date(std::string_view text) {
  auto rx = std::regex{R"((\d{1,2})(\.|-|/)(\d{1,2})(\.|-|/)(\d{4}))"};
  return std::regex_replace(text.data(), rx, R"($5-$3-$1)");
}
int main() {
  using namespace std::string_literals;
  assert(transform_date("today is 01.12.2017!"s) == "today is 2017-12-01!"s);
}
```

## Streams and Filesystems

### 32. Pascal's triangle


```c++

#include <string>
#include <iostream>
#include <cmath>
unsigned int number_of_digits(unsigned int const i) {
  return i > 0 ? (int)log10((double)i) + 1 : 1;
}
void print_pascal_triangle(int const n) {
  for (int i = 0; i < n; i++) {
    auto x = 1;
    std::cout << std::string((n - i - 1) * (n / 2), ' ');
    for (int j = 0; j <= i; j++) {
      auto y = x;
      x = x * (i - j) / (j + 1);
      auto maxlen = number_of_digits(x) - 1;
      std::cout << y << std::string(n - 1 - maxlen - n % 2, ' ');
    }
    std::cout << std::endl;
  }
}
int main() {
  int n = 0;
  std::cout << "Levels (up to 10): ";
  std::cin >> n;
  if (n > 10)
    std::cout << "Value too large" << std::endl;
  else
    print_pascal_triangle(n);
}
```

### 33. Tabular printing of a list of processes


```c++

#include <iostream>
#include <iomanip>
#include <string>
#include <vector>
#include <algorithm>
enum class procstatus { suspended, running };
std::string status_to_string(procstatus const status) {
  if (status == procstatus::suspended)
    return "suspended";
  else
    return "running";
}
enum class platforms { p32bit, p64bit };
std::string platform_to_string(platforms const platform) {
  if (platform == platforms::p32bit)
    return "32-bit";
  else
    return "64-bit";
}
struct procinfo {
  int id;
  std::string name;
  procstatus status;
  std::string account;
  size_t memory;
  platforms platform;
};
void print_processes(std::vector<procinfo> processes) {
  std::sort(
      std::begin(processes), std::end(processes),
      [](procinfo const &p1, procinfo const &p2) { return p1.name < p2.name; });
  for (auto const &pi : processes) {
    std::cout << std::left << std::setw(25) << std::setfill(' ') << pi.name;
    std::cout << std::left << std::setw(8) << std::setfill(' ') << pi.id;
    std::cout << std::left << std::setw(12) << std::setfill(' ')
              << status_to_string(pi.status);
    std::cout << std::left << std::setw(15) << std::setfill(' ') << pi.account;
    std::cout << std::right << std::setw(10) << std::setfill(' ')
              << (int)(pi.memory / 1024);
    std::cout << std::left << ' ' << platform_to_string(pi.platform);
    std::cout << std::endl;
  }
}
int main() {
  using namespace std::string_literals;
  std::vector<procinfo> processes{
      {512, "cmd.exe"s, procstatus::running, "SYSTEM"s, 148293,
       platforms::p64bit},
      {1044, "chrome.exe"s, procstatus::running, "marius.bancila"s, 25180454,
       platforms::p32bit},
      {7108, "explorer.exe"s, procstatus::running, "marius.bancila"s, 2952943,
       platforms::p64bit},
      {10100, "chrome.exe"s, procstatus::running, "marius.bancila"s, 227756123,
       platforms::p32bit},
      {22456, "skype.exe"s, procstatus::suspended, "marius.bancila"s, 16870123,
       platforms::p64bit},
  };
  print_processes(processes);
}
```

### 34. Removing empty lines from a text file


```c++

// -lstdc++fs
#include <fstream>
#include <string> 
#ifdef USE_BOOST_FILESYSTEM
#include <boost/filesystem/path.hpp>
#include <boost/filesystem/operations.hpp>
namespace fs = boost::filesystem;
#else
#include <filesystem>
#ifdef FILESYSTEM_EXPERIMENTAL
namespace fs = std::experimental::filesystem;
#else
namespace fs = std::filesystem;
#endif
#endif
void remove_empty_lines(fs::path filepath) {
  std::ifstream filein(filepath.native(), std::ios::in);
  if (!filein.is_open())
    throw std::runtime_error("cannot open input file");
  auto temppath = fs::temp_directory_path() / "temp.txt";
  std::ofstream fileout(temppath.native(), std::ios::out | std::ios::trunc);
  if (!fileout.is_open())
    throw std::runtime_error("cannot create temporary file");
  std::string line;
  while (std::getline(filein, line)) {
    if (line.length() > 0 && line.find_first_not_of(' ') != line.npos) {
      fileout << line << '\n';
    }
  }
  filein.close();
  fileout.close();
  fs::remove(filepath);
  fs::rename(temppath, filepath);
}
int main() { remove_empty_lines("sample34.txt"); }
```

### 35. Computing the size of a directory


```c++

#include <iostream>
#include <numeric>
#include <string>
#ifdef USE_BOOST_FILESYSTEM
#include <boost/filesystem/path.hpp>
#include <boost/filesystem/operations.hpp>
namespace fs = boost::filesystem;
#else
#include <filesystem>
#ifdef FILESYSTEM_EXPERIMENTAL
namespace fs = std::experimental::filesystem;
#else
namespace fs = std::filesystem;
#endif
#endif
std::uintmax_t get_directory_size(fs::path const &dir,
                                  bool const follow_symlinks = false) {
#ifdef USE_BOOST_FILESYSTEM
  auto iterator = fs::recursive_directory_iterator(
      dir,
      follow_symlinks ? fs::symlink_option::recurse : fs::symlink_option::none);
#else
  auto iterator = fs::recursive_directory_iterator(
      dir, follow_symlinks ? fs::directory_options::follow_directory_symlink
                           : fs::directory_options::none);
#endif
  return std::accumulate(
      fs::begin(iterator), fs::end(iterator), 0ull,
      [](std::uintmax_t const total, fs::directory_entry const &entry) {
        return total +
               (fs::is_regular_file(entry) ? fs::file_size(entry.path()) : 0);
      });
}
int main() {
  std::string path;
  std::cout << "Path: ";
  std::cin >> path;
  std::cout << "Size: " << get_directory_size(path) << std::endl;
}
```

### 36. Deleting files older than a given date


```c++

#include <iostream>
#include <chrono>
#ifdef USE_BOOST_FILESYSTEM
#include <boost/filesystem/path.hpp>
#include <boost/filesystem/operations.hpp>
namespace fs = boost::filesystem;
#else
#include <filesystem>
#ifdef FILESYSTEM_EXPERIMENTAL
namespace fs = std::experimental::filesystem;
#else
namespace fs = std::filesystem;
#endif
#endif
namespace ch = std::chrono;
template <typename Duration>
bool is_older_than(fs::path const &path, Duration const duration) {
  auto lastwrite = fs::last_write_time(path);
#ifdef USE_BOOST_FILESYSTEM
  auto ftimeduration =
      ch::system_clock::from_time_t(lastwrite).time_since_epoch();
#else
  auto ftimeduration = lastwrite.time_since_epoch();
#endif
  auto nowduration = (ch::system_clock::now() - duration).time_since_epoch();
  return ch::duration_cast<Duration>(nowduration - ftimeduration).count() > 0;
}
template <typename Duration>
void remove_files_older_than(fs::path const &path, Duration const duration) {
  try {
    if (fs::exists(path)) {
      if (is_older_than(path, duration)) {
        fs::remove(path);
      } else if (fs::is_directory(path)) {
        for (auto const &entry : fs::directory_iterator(path)) {
          remove_files_older_than(entry.path(), duration);
        }
      }
    }
  } catch (std::exception const &ex) {
    std::cerr << ex.what() << std::endl;
  }
}
int main() {
  using namespace std::chrono_literals;
#ifdef _WIN32
  auto path = R"(..\Test\)";
#else
  auto path = R"(../Test/)";
#endif
  remove_files_older_than(path, 1h + 20min);
}
```

### 37. Finding files in a directory that match a regular expression


```c++

#include <iostream>
#include <regex>
#include <vector>
#include <string>
#include <string_view>
#include <functional>
#ifdef USE_BOOST_FILESYSTEM
#include <boost/filesystem/path.hpp>
#include <boost/filesystem/operations.hpp>
namespace fs = boost::filesystem;
#else
#include <filesystem>
#ifdef FILESYSTEM_EXPERIMENTAL
namespace fs = std::experimental::filesystem;
#else
namespace fs = std::filesystem;
#endif
#endif
std::vector<fs::directory_entry> find_files(fs::path const &path,
                                            std::string_view regex) {
  std::vector<fs::directory_entry> result;
  std::regex rx(regex.data());
  std::copy_if(fs::recursive_directory_iterator(path),
               fs::recursive_directory_iterator(), std::back_inserter(result),
               [&rx](fs::directory_entry const &entry) {
                 return fs::is_regular_file(entry.path()) &&
                        std::regex_match(entry.path().filename().string(), rx);
               });
  return result;
}
int main() {
  auto dir = fs::temp_directory_path();
  auto pattern = R"(wct[0-9a-zA-Z]{3}\.tmp)";
  auto result = find_files(dir, pattern);
  for (auto const &entry : result) {
    std::cout << entry.path().string() << std::endl;
  }
}
```

### 38. Temporary log files


```c++

#include <iostream>
#include <fstream>
#include "uuid.h"
#ifdef USE_BOOST_FILESYSTEM
#include <boost/filesystem/path.hpp>
#include <boost/filesystem/operations.hpp>
namespace fs = boost::filesystem;
#else
#include <filesystem>
#ifdef FILESYSTEM_EXPERIMENTAL
namespace fs = std::experimental::filesystem;
#else
namespace fs = std::filesystem;
#endif
#endif
class logger {
  fs::path logpath;
  std::ofstream logfile;
public:
  logger() {
    auto name = uuids::to_string(uuids::uuid_random_generator{}());
    logpath = fs::temp_directory_path() / (name + ".tmp");
    logfile.open(logpath.c_str(), std::ios::out | std::ios::trunc);
  }
  ~logger() noexcept {
    try {
      if (logfile.is_open())
        logfile.close();
      if (!logpath.empty())
        fs::remove(logpath);
    } catch (...) {
    }
  }
  void persist(fs::path const &path) {
    logfile.close();
    fs::rename(logpath, path);
    logpath.clear();
  }
  logger &operator<<(std::string_view message) {
    logfile << message.data() << '\n';
    return *this;
  }
};
int main() {
  logger log;
  try {
    log << "this is a line"
        << "and this is another one";
    throw std::runtime_error("error");
  } catch (...) {
    log.persist(R"(lastlog.txt)");
  }
}
```

## Date and Time

### 39. Measuring function execution time


```c++

#include <iostream>
#include <chrono>
#include <thread>
#include <functional>
template <typename Time = std::chrono::microseconds,
          typename Clock = std::chrono::high_resolution_clock>
struct perf_timer {
  template <typename F, typename... Args>
  static Time duration(F &&f, Args... args) {
    auto start = Clock::now();
    std::invoke(std::forward<F>(f), std::forward<Args>(args)...);
    auto end = Clock::now();
    return std::chrono::duration_cast<Time>(end - start);
  }
};
using namespace std::chrono_literals;
void f() {
  // simulate work
  std::this_thread::sleep_for(2s);
}
void g(int const a, int const b) {
  // simulate work
  std::this_thread::sleep_for(1s);
}
int main() {
  auto t1 = perf_timer<std::chrono::microseconds>::duration(f);
  auto t2 = perf_timer<std::chrono::milliseconds>::duration(g, 1, 2);
  auto total = std::chrono::duration<double, std::nano>(t1 + t2).count();
  std::cout << total << std::endl;
}
```

### 40. Number of days between two dates


```c++

// -I "C:\Codes\Codes\C++\Module 3_TheModernCppChallenge_Code\libs\date\include\date"
#include <iostream>
#include "date.h"
inline int number_of_days(int const y1, unsigned int const m1,
                          unsigned int const d1, int const y2,
                          unsigned int const m2, unsigned int const d2) {
  using namespace date;
  return (sys_days{year{y1} / month{m1} / day{d1}} -
          sys_days{year{y2} / month{m2} / day{d2}})
      .count();
}
inline int number_of_days(date::sys_days const &first,
                          date::sys_days const &last) {
  return (last - first).count();
}
int main() {
  unsigned int y1 = 0, m1 = 0, d1 = 0;
  std::cout << "First date" << std::endl;
  std::cout << "Year:";
  std::cin >> y1;
  std::cout << "Month:";
  std::cin >> m1;
  std::cout << "Date:";
  std::cin >> d1;
  std::cout << "Second date" << std::endl;
  unsigned int y2 = 0, m2 = 0, d2 = 0;
  std::cout << "Year:";
  std::cin >> y2;
  std::cout << "Month:";
  std::cin >> m2;
  std::cout << "Date:";
  std::cin >> d2;
  std::cout << "Days between:" << number_of_days(y1, m1, d1, y2, m2, d2)
            << std::endl;
  using namespace date::literals;
  std::cout << "Days between:"
            << number_of_days(2018_y / jun / 1, 15_d / sep / 2018) << std::endl;
}
```

### 41. Day of the week


```c++

// -I "C:\Codes\Codes\C++\Module 3_TheModernCppChallenge_Code\libs\date\include\date"
#include <iostream>
#include "date.h"
#include "iso_week.h"
unsigned int week_day(int const y, unsigned int const m, unsigned int const d) {
  using namespace date;
  if (m < 1 || m > 12 || d < 1 || d > 31)
    return 0;
  auto const dt = date::year_month_day{year{y}, month{m}, day{d}};
  auto const tiso = iso_week::year_weeknum_weekday{dt};
  return (unsigned int)tiso.weekday();
}
int main() {
  int y = 0;
  unsigned int m = 0, d = 0;
  std::cout << "Year:";
  std::cin >> y;
  std::cout << "Month:";
  std::cin >> m;
  std::cout << "Day:";
  std::cin >> d;
  std::cout << "Day of week:" << week_day(y, m, d) << std::endl;
}
```

### 42. Day and week of the year


```c++

// -I "C:\Codes\Codes\C++\Module 3_TheModernCppChallenge_Code\libs\date\include\date"
#include <iostream>
#include "date.h"
#include "iso_week.h"
unsigned int calendar_week(int const y, unsigned int const m,
                           unsigned int const d) {
  using namespace date;
  if (m < 1 || m > 12 || d < 1 || d > 31)
    return 0;
  auto const dt = date::year_month_day{year{y}, month{m}, day{d}};
  auto const tiso = iso_week::year_weeknum_weekday{dt};
  return (unsigned int)tiso.weeknum();
}
int day_of_year(int const y, unsigned int const m, unsigned int const d) {
  using namespace date;
  if (m < 1 || m > 12 || d < 1 || d > 31)
    return 0;
  return (sys_days{year{y} / month{m} / day{d}} - sys_days{year{y} / jan / 0})
      .count();
}
int main() {
  int y = 0;
  unsigned int m = 0, d = 0;
  std::cout << "Year:";
  std::cin >> y;
  std::cout << "Month:";
  std::cin >> m;
  std::cout << "Day:";
  std::cin >> d;
  std::cout << "Calendar week:" << calendar_week(y, m, d) << std::endl;
  std::cout << "Day of year:" << day_of_year(y, m, d) << std::endl;
}
```

### 43. Meeting time for multiple time zones


```c++

// -I "C:\Codes\Codes\C++\Module 3_TheModernCppChallenge_Code\libs\date\include\date"
#include <iostream>
#include <string>
#include <vector>
#include <string_view>
#include <iomanip>
#include "date.h"
#include "tz.h"
namespace ch = std::chrono;
struct user {
  std::string Name;
  date::time_zone const *Zone;
  user(std::string_view name, std::string_view zone)
      : Name{name.data()}, Zone(date::locate_zone(zone.data())) {}
};
template <class Duration, class TimeZonePtr>
void print_meeting_times(date::zoned_time<Duration, TimeZonePtr> const &time,
                         std::vector<user> const &users) {
  std::cout << std::left << std::setw(15) << std::setfill(' ')
            << "Local time: " << time << std::endl;
  for (auto const &user : users) {
    std::cout << std::left << std::setw(15) << std::setfill(' ') << user.Name
              << date::zoned_time<Duration, TimeZonePtr>(user.Zone, time)
              << std::endl;
  }
}
int main() {
  std::vector<user> users{user{"Ildiko", "Europe/Budapest"},
                          user{"Jens", "Europe/Berlin"},
                          user{"Jane", "America/New_York"}};
  unsigned int h, m;
  std::cout << "Hour:";
  std::cin >> h;
  std::cout << "Minutes:";
  std::cin >> m;
  date::year_month_day today = date::floor<date::days>(ch::system_clock::now());
  auto localtime = date::zoned_time<std::chrono::minutes>(
      date::current_zone(),
      static_cast<date::local_days>(today) + ch::hours{h} + ch::minutes{m});
  print_meeting_times(localtime, users);
}
```

### 44. Monthly calendar


```c++

// -I "C:\Codes\Codes\C++\Module 3_TheModernCppChallenge_Code\libs\date\include\date"
#include <iostream>
#include <iomanip>
#include "date.h"
#include "iso_week.h"
unsigned int week_day(int const y, unsigned int const m, unsigned int const d) {
  using namespace date;
  if (m < 1 || m > 12 || d < 1 || d > 31)
    return 0;
  auto const dt = date::year_month_day{year{y}, month{m}, day{d}};
  auto const tiso = iso_week::year_weeknum_weekday{dt};
  return (unsigned int)tiso.weekday();
}
void print_month_calendar(int const y, unsigned int m) {
  using namespace date;
  std::cout << "Mon Tue Wed Thu Fri Sat Sun" << std::endl;
  auto first_day_weekday = week_day(y, m, 1);
  auto last_day =
      (unsigned int)year_month_day_last(year{y}, month_day_last{month{m}})
          .day();
  unsigned int index = 1;
  for (unsigned int day = 1; day < first_day_weekday; ++day, ++index) {
    std::cout << "    ";
  }
  for (unsigned int day = 1; day <= last_day; ++day) {
    std::cout << std::right << std::setfill(' ') << std::setw(3) << day << ' ';
    if (index++ % 7 == 0)
      std::cout << std::endl;
  }
  std::cout << std::endl;
}
int main() {
  unsigned int y = 0, m = 0;
  std::cout << "Year:";
  std::cin >> y;
  std::cout << "Month:";
  std::cin >> m;
  print_month_calendar(y, m);
}
```

## Algorithms and Data Structures

### 45. Priority queue


```c++

#include <iostream>
#include <vector>
#include <algorithm>
#include <assert.h>
template <class T,
          class Compare = std::less<typename std::vector<T>::value_type>>
class priority_queue {
  typedef typename std::vector<T>::value_type value_type;
  typedef typename std::vector<T>::size_type size_type;
  typedef typename std::vector<T>::reference reference;
  typedef typename std::vector<T>::const_reference const_reference;
public:
  bool empty() const noexcept { return data.empty(); }
  size_type size() const noexcept { return data.size(); }
  void push(value_type const &value) {
    data.push_back(value);
    std::push_heap(std::begin(data), std::end(data), comparer);
  }
  void pop() {
    std::pop_heap(std::begin(data), std::end(data), comparer);
    data.pop_back();
  }
  const_reference top() const { return data.front(); }
  void swap(priority_queue &other) noexcept {
    swap(data, other.data);
    swap(comparer, other.comparer);
  }
private:
  std::vector<T> data;
  Compare comparer;
};
template <class T, class Compare>
void swap(priority_queue<T, Compare> &lhs,
          priority_queue<T, Compare> &rhs) noexcept(noexcept(lhs.swap(rhs))) {
  lhs.swap(rhs);
}
int main() {
  priority_queue<int> q;
  for (int i : {1, 5, 3, 1, 13, 21, 8}) {
    q.push(i);
  }
  assert(!q.empty());
  assert(q.size() == 7);
  while (!q.empty()) {
    std::cout << q.top() << ' ';
    q.pop();
  }
}
```

### 46. Circular buffer


```c++

#include <iostream>
#include <vector>
#include <assert.h>
template <class T>
class circular_buffer;
template <class T>
class circular_buffer_iterator
{
   typedef circular_buffer_iterator          self_type;
   typedef T                                 value_type;
   typedef T&                                reference;
   typedef T const&                          const_reference;
   typedef T*                                pointer;
   typedef std::random_access_iterator_tag   iterator_category;
   typedef ptrdiff_t                         difference_type;
public:
   circular_buffer_iterator(circular_buffer<T> const & buf, size_t const pos, bool const last) :
      buffer_(buf), index_(pos), last_(last)
   {}
   self_type & operator++ ()
   {
      if (last_)
         throw std::out_of_range("Iterator cannot be incremented past the end of range.");
      index_ = (index_ + 1) % buffer_.data_.size();
      last_ = index_ == buffer_.next_pos();
      return *this;
   }
   self_type operator++ (int)
   {
      self_type tmp = *this;
      ++*this;
      return tmp;
   }
   bool operator== (self_type const & other) const
   {
      assert(compatible(other));
      return index_ == other.index_ && last_ == other.last_;
   }
   bool operator!= (self_type const & other) const
   {
      return !(*this == other);
   }
   const_reference operator* () const
   {
      return buffer_.data_[index_];
   }
   const_reference operator-> () const
   {
      return buffer_.data_[index_];
   }
private:
   bool compatible(self_type const & other) const
   {
      return &buffer_ == &other.buffer_;
   }
   circular_buffer<T> const & buffer_;
   size_t index_;
   bool last_;
};
template <class T>
class circular_buffer
{
   typedef circular_buffer_iterator<T> const_iterator;
   circular_buffer() = delete;
public:
   explicit circular_buffer(size_t const size) :data_(size)
   {}
   bool clear() noexcept { head_ = -1; size_ = 0; }
   bool empty() const noexcept { return size_ == 0; }
   bool full() const noexcept { return size_ == data_.size(); }
   size_t capacity() const noexcept { return data_.size(); }
   size_t size() const noexcept { return size_; }
   void push(T const item)
   {
      head_ = next_pos();
      data_[head_] = item;
      if (size_ < data_.size()) size_++;
   }
   T pop()
   {
      if (empty()) throw std::runtime_error("empty buffer");
      auto pos = first_pos();
      size_--;
      return data_[pos];
   }
   const_iterator begin() const
   {
      return const_iterator(*this, first_pos(), empty());
   }
   const_iterator end() const
   {
      return const_iterator(*this, next_pos(), true);
   }
private:
   std::vector<T> data_;
   size_t head_ = -1;
   size_t size_ = 0;
   size_t next_pos() const noexcept { return size_ == 0 ? 0 : (head_ + 1) % data_.size(); }
   size_t first_pos() const noexcept { return size_ == 0 ? 0 : (head_ + data_.size() - size_ + 1) % data_.size(); }
   friend class circular_buffer_iterator<T>;
};
template <typename T>
void print(circular_buffer<T> & buf)
{
   for (auto & e : buf)
   {
      std::cout << e << ' ';
   }
   std::cout << std::endl;
}
int main()
{
   circular_buffer<int> cbuf(5);
   assert(cbuf.empty());
   assert(!cbuf.full());
   assert(cbuf.size() == 0);
   print(cbuf);
   cbuf.push(1);
   cbuf.push(2);
   cbuf.push(3);
   assert(!cbuf.empty());
   assert(!cbuf.full());
   assert(cbuf.size() == 3);
   print(cbuf);
   auto item = cbuf.pop();
   assert(item == 1);
   assert(!cbuf.empty());
   assert(!cbuf.full());
   assert(cbuf.size() == 2);
   cbuf.push(4);
   cbuf.push(5);
   cbuf.push(6);
   assert(!cbuf.empty());
   assert(cbuf.full());
   assert(cbuf.size() == 5);
   print(cbuf);
   cbuf.push(7);
   cbuf.push(8);
   assert(!cbuf.empty());
   assert(cbuf.full());
   assert(cbuf.size() == 5);
   print(cbuf);
   item = cbuf.pop();
   assert(item == 4);
   item = cbuf.pop();
   assert(item == 5);
   item = cbuf.pop();
   assert(item == 6);
   assert(!cbuf.empty());
   assert(!cbuf.full());
   assert(cbuf.size() == 2);
   print(cbuf);
   item = cbuf.pop();
   assert(item == 7);
   item = cbuf.pop();
   assert(item == 8);
   assert(cbuf.empty());
   assert(!cbuf.full());
   assert(cbuf.size() == 0);
   print(cbuf);
   cbuf.push(9);
   assert(!cbuf.empty());
   assert(!cbuf.full());
   assert(cbuf.size() == 1);
   print(cbuf);
}
```

### 47. Double buffer


```c++

#include <vector>
#include <iostream>
#include <algorithm>
#include <thread>
#include <chrono>
#include <mutex>
#include <iterator>
template <typename T> class double_buffer {
  typedef T value_type;
  typedef T &reference;
  typedef T const &const_reference;
  typedef T *pointer;
public:
  explicit double_buffer(size_t const size) : rdbuf(size), wrbuf(size) {}
  size_t size() const noexcept { return rdbuf.size(); }
  void write(T const *const ptr, size_t const size) {
    std::unique_lock<std::mutex> lock(mt);
    auto length = std::min(size, wrbuf.size());
    std::copy(ptr, ptr + length, std::begin(wrbuf));
    wrbuf.swap(rdbuf);
  }
  template <class Output> void read(Output it) const {
    std::unique_lock<std::mutex> lock(mt);
    std::copy(std::cbegin(rdbuf), std::cend(rdbuf), it);
  }
  pointer data() const {
    std::unique_lock<std::mutex> lock(mt);
    return rdbuf.data();
  }
  reference operator[](size_t const pos) {
    std::unique_lock<std::mutex> lock(mt);
    return rdbuf[pos];
  }
  const_reference operator[](size_t const pos) const {
    std::unique_lock<std::mutex> lock(mt);
    return rdbuf[pos];
  }
  void swap(double_buffer other) {
    std::swap(rdbuf, other.rdbuf);
    std::swap(wrbuf, other.wrbuf);
  }
private:
  std::vector<T> rdbuf;
  std::vector<T> wrbuf;
  mutable std::mutex mt;
};
template <typename T> void print_buffer(double_buffer<T> const &buf) {
  buf.read(std::ostream_iterator<T>(std::cout, " "));
  std::cout << std::endl;
}
int main() {
  double_buffer<int> buf(10);
  std::thread t([&buf]() {
    for (int i = 1; i < 1000; i += 10) {
      int data[] = {i,     i + 1, i + 2, i + 3, i + 4,
                    i + 5, i + 6, i + 7, i + 8, i + 9};
      buf.write(data, 10);
      using namespace std::chrono_literals;
      std::this_thread::sleep_for(100ms);
    }
  });
  auto start = std::chrono::system_clock::now();
  do {
    print_buffer(buf);
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(150ms);
  } while (std::chrono::duration_cast<std::chrono::seconds>(
               std::chrono::system_clock::now() - start)
               .count() < 12);
  t.join();
}
```

### 48. The most frequent element in a range


```c++

#include <iostream>
#include <map>
#include <vector>
#include <algorithm>
template <typename T>
std::vector<std::pair<T, size_t>>
find_most_frequent(std::vector<T> const &range) {
  std::map<T, size_t> counts;
  for (auto const &e : range)
    counts[e]++;
  auto maxelem = std::max_element(
      std::cbegin(counts), std::cend(counts),
      [](auto const &e1, auto const &e2) { return e1.second < e2.second; });
  std::vector<std::pair<T, size_t>> result;
  std::copy_if(
      std::begin(counts), std::end(counts), std::back_inserter(result),
      [maxelem](auto const &kvp) { return kvp.second == maxelem->second; });
  return result;
}
int main() {
  auto range = std::vector<int>{1, 1, 3, 5, 8, 13, 3, 5, 8, 8, 5};
  auto result = find_most_frequent(range);
  for (auto const &e : result) {
    std::cout << e.first << " : " << e.second << std::endl;
  }
}
```

### 49. Text histogram


```c++

#include <iostream>
#include <map>
#include <algorithm>
#include <numeric>
#include <iomanip>
#include <string>
#include <string_view>
std::map<char, double> analyze_text(std::string_view text) {
  std::map<char, double> frequencies;
  for (char ch = 'a'; ch <= 'z'; ch++)
    frequencies[ch] = 0;
  for (auto ch : text) {
    if (isalpha(ch))
      frequencies[tolower(ch)]++;
  }
  auto total = std::accumulate(std::cbegin(frequencies), std::cend(frequencies),
                               0ull, [](auto const sum, auto const &kvp) {
                                 return sum + static_cast<unsigned long long>(
                                                  kvp.second);
                               });
  std::for_each(
      std::begin(frequencies), std::end(frequencies),
      [total](auto &kvp) { kvp.second = (100.0 * kvp.second) / total; });
  return frequencies;
}
int main() {
  auto result = analyze_text(
      R"(Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.)");
  for (auto const &kvp : result) {
    std::cout << kvp.first << " : " << std::fixed << std::setw(5)
              << std::setfill(' ') << std::setprecision(2) << kvp.second
              << std::endl;
  }
}
```

### 50. Filtering a list of phone numbers


```c++

#include <string>
#include <string_view>
#include <vector>
#include <iostream>
#include <algorithm>
bool starts_with(std::string_view str, std::string_view prefix) {
  return str.find(prefix) == 0;
}
template <typename InputIt>
std::vector<std::string> filter_numbers(InputIt begin, InputIt end,
                                        std::string const &countryCode) {
  std::vector<std::string> result;
  std::copy_if(begin, end, std::back_inserter(result),
               [countryCode](auto const &number) {
                 return starts_with(number, countryCode) ||
                        starts_with(number, "+" + countryCode);
               });
  return result;
}
std::vector<std::string> filter_numbers(std::vector<std::string> const &numbers,
                                        std::string const &countryCode) {
  return filter_numbers(std::cbegin(numbers), std::cend(numbers), countryCode);
}
int main() {
  std::vector<std::string> numbers{"+40744909080", "44 7520 112233",
                                   "+44 7555 123456", "40 7200 123456",
                                   "7555 123456"};
  auto result = filter_numbers(numbers, "44");
  for (auto const &number : result) {
    std::cout << number << std::endl;
  }
}
```

### 51. Transforming a list of phone numbers


```c++

#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
bool starts_with(std::string_view str, std::string_view prefix) {
  return str.find(prefix) == 0;
}
void normalize_phone_numbers(std::vector<std::string> &numbers,
                             std::string const &countryCode) {
  std::transform(
      std::cbegin(numbers), std::cend(numbers), std::begin(numbers),
      [countryCode](std::string const &number) {
        std::string result;
        if (number.size() > 0) {
          if (number[0] == '0')
            result = "+" + countryCode + number.substr(1);
          else if (starts_with(number, countryCode))
            result = "+" + number;
          else if (starts_with(number, "+" + countryCode))
            result = number;
          else
            result = "+" + countryCode + number;
        }
        result.erase(std::remove_if(std::begin(result), std::end(result),
                                    [](const char ch) { return isspace(ch); }),
                     std::end(result));
        return result;
      });
}
int main() {
  std::vector<std::string> numbers{"07555 123456", "07555123456",
                                   "+44 7555 123456", "44 7555 123456",
                                   "7555 123456"};
  normalize_phone_numbers(numbers, "44");
  for (auto const &number : numbers) {
    std::cout << number << std::endl;
  }
}
```

### 52. Generating all the permutations of a string


```c++

#include <iostream>
#include <string>
#include <algorithm>
void print_permutations(std::string str)
{
   std::sort(std::begin(str), std::end(str));
   do
   {
      std::cout << str << std::endl;
   } while (std::next_permutation(std::begin(str), std::end(str)));
}
void next_permutation(std::string str, std::string perm)
{
   if (str.empty()) std::cout << perm << std::endl;
   else
   {
      for (size_t i = 0; i < str.size(); ++i)
      {
         next_permutation(str.substr(1), perm + str[0]);
         std::rotate(std::begin(str), std::begin(str) + 1, std::end(str));
      }
   }
}
void print_permutations_recursive(std::string str)
{
   next_permutation(str, "");
}
int main()
{
   std::cout << "non-recursive version" << std::endl;
   print_permutations("main");
   std::cout << "recursive version" << std::endl;
   print_permutations_recursive("main");
}
```

### 53. Average rating of movies


```c++

#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <numeric>
#include <iomanip>
struct movie {
  int id;
  std::string title;
  std::vector<int> ratings;
};
double truncated_mean(std::vector<int> values, double const percentage) {
  std::sort(std::begin(values), std::end(values));
  auto remove_count = static_cast<size_t>(values.size() * percentage + 0.5);
  values.erase(std::begin(values), std::begin(values) + remove_count);
  values.erase(std::end(values) - remove_count, std::end(values));
  auto total =
      std::accumulate(std::cbegin(values), std::cend(values), 0ull,
                      [](auto const sum, auto const e) { return sum + e; });
  return static_cast<double>(total) / values.size();
}
void print_movie_ratings(std::vector<movie> const &movies) {
  for (auto const &m : movies) {
    std::cout << m.title << " : " << std::fixed << std::setprecision(1)
              << truncated_mean(m.ratings, 0.05) << std::endl;
  }
}
int main() {
  std::vector<movie> movies{
      {101, "The Matrix", {10, 9, 10, 9, 9, 8, 7, 10, 5, 9, 9, 8}},
      {102, "Gladiator", {10, 5, 7, 8, 9, 8, 9, 10, 10, 5, 9, 8, 10}},
      {103, "Interstellar", {10, 10, 10, 9, 3, 8, 8, 9, 6, 4, 7, 10}}};
  print_movie_ratings(movies);
}
```

### 54. Pairwise algorithm


```c++

#include <iostream>
#include <vector>
template <typename Input, typename Output>
void pairwise(Input begin, Input end, Output result) {
  auto it = begin;
  while (it != end) {
    auto v1 = *it++;
    if (it == end)
      break;
    auto v2 = *it++;
    result++ = std::make_pair(v1, v2);
  }
}
template <typename T>
std::vector<std::pair<T, T>> pairwise(std::vector<T> const &range) {
  std::vector<std::pair<T, T>> result;
  pairwise(std::begin(range), std::end(range), std::back_inserter(result));
  return result;
}
int main() {
  std::vector<int> v{1, 1, 3, 5, 8, 13, 21};
  auto result = pairwise(v);
  for (auto const &p : result) {
    std::cout << '{' << p.first << ',' << p.second << '}' << std::endl;
  }
}
```

### 55. Zip algorithm


```c++

#include <iostream>
#include <vector>
template <typename Input1, typename Input2, typename Output>
void zip(Input1 begin1, Input1 end1, Input2 begin2, Input2 end2,
         Output result) {
  auto it1 = begin1;
  auto it2 = begin2;
  while (it1 != end1 && it2 != end2) {
    result++ = std::make_pair(*it1++, *it2++);
  }
}
template <typename T, typename U>
std::vector<std::pair<T, U>> zip(std::vector<T> const &range1,
                                 std::vector<U> const &range2) {
  std::vector<std::pair<T, U>> result;
  zip(std::begin(range1), std::end(range1), std::begin(range2),
      std::end(range2), std::back_inserter(result));
  return result;
}
int main() {
  std::vector<int> v1{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  std::vector<int> v2{1, 1, 3, 5, 8, 13, 21};
  auto result = zip(v1, v2);
  for (auto const &p : result) {
    std::cout << '{' << p.first << ',' << p.second << '}' << std::endl;
  }
}
```

### 56. Select algorithm


```c++

#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
template <typename T, typename A, typename F,
          typename R = typename std::decay<
              typename std::result_of<typename std::decay<F>::type &(
                  typename std::vector<T, A>::const_reference)>::type>::type>
std::vector<R> select(std::vector<T, A> const &c, F &&f) {
  std::vector<R> v;
  std::transform(std::cbegin(c), std::cend(c), std::back_inserter(v),
                 std::forward<F>(f));
  return v;
}
struct book {
  int id;
  std::string title;
  std::string author;
};
int main() {
  std::vector<book> books{
      {101, "The C++ Programming Language", "Bjarne Stroustrup"},
      {203, "Effective Modern C++", "Scott Meyers"},
      {404, "The Modern C++ Programming Cookbook", "Marius Bancila"}};
  auto titles = select(books, [](book const &b) { return b.title; });
  for (auto const &title : titles) {
    std::cout << title << std::endl;
  }
}
```

### 57. Sort algorithm


```c++

#include <iostream>
#include <vector>
#include <array>
#include <functional>
#include <numeric>
#include <random>
#include <array>
#include <stack>
#include <chrono>
#include <assert.h>
template <class RandomIt> RandomIt partition(RandomIt first, RandomIt last) {
  auto pivot = *first;
  auto i = first + 1;
  auto j = last - 1;
  while (i <= j) {
    while (i <= j && *i <= pivot)
      i++;
    while (i <= j && *j > pivot)
      j--;
    if (i < j)
      std::iter_swap(i, j);
  }
  std::iter_swap(i - 1, first);
  return i - 1;
}
template <class RandomIt, class Compare>
RandomIt partitionc(RandomIt first, RandomIt last, Compare comp) {
  auto pivot = *first;
  auto i = first + 1;
  auto j = last - 1;
  while (i <= j) {
    while (i <= j && comp(*i, pivot))
      i++;
    while (i <= j && !comp(*j, pivot))
      j--;
    if (i < j)
      std::iter_swap(i, j);
  }
  std::iter_swap(i - 1, first);
  return i - 1;
}
template <class RandomIt> void quicksorti(RandomIt first, RandomIt last) {
  std::stack<std::pair<RandomIt, RandomIt>> st;
  st.push(std::make_pair(first, last));
  while (!st.empty()) {
    auto iters = st.top();
    st.pop();
    if (iters.second - iters.first < 2)
      continue;
    auto p = partition(iters.first, iters.second);
    st.push(std::make_pair(iters.first, p));
    st.push(std::make_pair(p + 1, iters.second));
  }
}
template <class RandomIt> void quicksort(RandomIt first, RandomIt last) {
  if (first < last) {
    auto p = partition(first, last);
    quicksort(first, p);
    quicksort(p + 1, last);
  }
}
template <class RandomIt, class Compare>
void quicksort(RandomIt first, RandomIt last, Compare comp) {
  if (first < last) {
    auto p = partitionc(first, last, comp);
    quicksort(first, p, comp);
    quicksort(p + 1, last, comp);
  }
}
template <class RandomIt> void print(RandomIt first, RandomIt last) {
  for (auto it = first; it < last; ++it) {
    std::cout << *it << ' ';
  }
  std::cout << std::endl;
}
int main() {
  {
    std::vector<int> v{1, 5, 3, 8, 6, 2, 9, 7, 4};
    quicksort(std::begin(v), std::end(v));
    print(std::begin(v), std::end(v));
  }
  {
    std::array<int, 9> a{1, 2, 3, 4, 5, 6, 7, 8, 9};
    quicksort(std::begin(a), std::end(a));
    print(std::begin(a), std::end(a));
  }
  {
    int a[]{9, 8, 7, 6, 5, 4, 3, 2, 1};
    quicksort(std::begin(a), std::end(a));
    print(std::begin(a), std::end(a));
  }
  {
    std::vector<int> v{1, 5, 3, 8, 6, 2, 9, 7, 4};
    quicksort(std::begin(v), std::end(v), std::greater_equal<>());
    print(std::begin(v), std::end(v));
  }
  {
    std::array<int, 9> a{1, 2, 3, 4, 5, 6, 7, 8, 9};
    quicksort(std::begin(a), std::end(a), std::greater_equal<>());
    print(std::begin(a), std::end(a));
  }
  {
    int a[]{9, 8, 7, 6, 5, 4, 3, 2, 1};
    quicksort(std::begin(a), std::end(a), std::greater_equal<>());
    print(std::begin(a), std::end(a));
  }
  {
    std::vector<int> v{1, 5, 3, 8, 6, 2, 9, 7, 4};
    quicksorti(std::begin(v), std::end(v));
    print(std::begin(v), std::end(v));
  }
  {
    const size_t count = 1000000;
    std::vector<int> data(count);
    std::random_device rd;
    std::mt19937 mt;
    auto seed_data = std::array<int, std::mt19937::state_size>{};
    std::generate(std::begin(seed_data), std::end(seed_data), std::ref(rd));
    std::seed_seq seq(std::begin(seed_data), std::end(seed_data));
    mt.seed(seq);
    std::uniform_int_distribution<> ud(1, 1000);
    std::cout << "generating..." << std::endl;
    std::generate_n(std::begin(data), count, [&mt, &ud]() { return ud(mt); });
    auto d1 = data;
    auto d2 = data;
    std::cout << "sorting..." << std::endl;
    auto start1 = std::chrono::system_clock::now();
    quicksort(std::begin(d1), std::end(d1));
    auto end1 = std::chrono::system_clock::now();
    auto t1 =
        std::chrono::duration_cast<std::chrono::milliseconds>(end1 - start1);
    std::cout << "time: " << t1.count() << "ms" << std::endl;
    std::cout << "sorting..." << std::endl;
    auto start2 = std::chrono::system_clock::now();
    quicksorti(std::begin(d2), std::end(d2));
    auto end2 = std::chrono::system_clock::now();
    auto t2 =
        std::chrono::duration_cast<std::chrono::milliseconds>(end2 - start2);
    std::cout << "time: " << t2.count() << "ms" << std::endl;
    assert(d1 == d2);
  }
}
```

### 58. The shortest path between nodes


```c++

#include <iostream>
#include <queue>
#include <set>
#include <algorithm>
#include <vector>
#include <map>
#include <numeric>
#include <string>
template <typename Vertex = int, typename Weight = double> class graph {
public:
  typedef Vertex vertex_type;
  typedef Weight weight_type;
  typedef std::pair<Vertex, Weight> neighbor_type;
  typedef std::vector<neighbor_type> neighbor_list_type;
public:
  void add_edge(Vertex const source, Vertex const target, Weight const weight,
                bool const bidirectional = true) {
    adjacency_list[source].push_back(std::make_pair(target, weight));
    adjacency_list[target].push_back(std::make_pair(source, weight));
  }
  size_t vertex_count() const { return adjacency_list.size(); }
  std::vector<Vertex> verteces() const {
    std::vector<Vertex> keys;
    for (auto const &kvp : adjacency_list)
      keys.push_back(kvp.first);
    return keys;
  }
  neighbor_list_type const &neighbors(Vertex const &v) const {
    auto pos = adjacency_list.find(v);
    if (pos == adjacency_list.end())
      throw std::runtime_error("vertex not found");
    return pos->second;
  }
  constexpr static Weight Infinity = std::numeric_limits<Weight>::infinity();
private:
  std::map<vertex_type, neighbor_list_type> adjacency_list;
};
template <typename Vertex, typename Weight>
void shortest_path(graph<Vertex, Weight> const &g, Vertex const source,
                   std::map<Vertex, Weight> &min_distance,
                   std::map<Vertex, Vertex> &previous) {
  auto const n = g.vertex_count();
  auto const verteces = g.verteces();
  min_distance.clear();
  for (auto const &v : verteces)
    min_distance[v] = graph<Vertex, Weight>::Infinity;
  min_distance[source] = 0;
  previous.clear();
  std::set<std::pair<Weight, Vertex>> vertex_queue;
  vertex_queue.insert(std::make_pair(min_distance[source], source));
  while (!vertex_queue.empty()) {
    auto dist = vertex_queue.begin()->first;
    auto u = vertex_queue.begin()->second;
    vertex_queue.erase(std::begin(vertex_queue));
    auto const &neighbors = g.neighbors(u);
    for (auto const &neighbor : neighbors) {
      auto v = neighbor.first;
      auto w = neighbor.second;
      auto dist_via_u = dist + w;
      if (dist_via_u < min_distance[v]) {
        vertex_queue.erase(std::make_pair(min_distance[v], v));
        min_distance[v] = dist_via_u;
        previous[v] = u;
        vertex_queue.insert(std::make_pair(min_distance[v], v));
      }
    }
  }
}
template <typename Vertex>
void build_path(std::map<Vertex, Vertex> const &prev, Vertex const v,
                std::vector<Vertex> &result) {
  result.push_back(v);
  auto pos = prev.find(v);
  if (pos == std::end(prev))
    return;
  build_path(prev, pos->second, result);
}
template <typename Vertex>
std::vector<Vertex> build_path(std::map<Vertex, Vertex> const &prev,
                               Vertex const v) {
  std::vector<Vertex> result;
  build_path(prev, v, result);
  std::reverse(std::begin(result), std::end(result));
  return result;
}
template <typename Vertex> void print_path(std::vector<Vertex> const &path) {
  for (size_t i = 0; i < path.size(); ++i) {
    std::cout << path[i];
    if (i < path.size() - 1)
      std::cout << " -> ";
  }
}
graph<char, double> make_graph() {
  graph<char, double> g;
  g.add_edge('A', 'B', 4);
  g.add_edge('A', 'H', 8);
  g.add_edge('B', 'C', 8);
  g.add_edge('B', 'H', 11);
  g.add_edge('C', 'D', 7);
  g.add_edge('C', 'F', 4);
  g.add_edge('C', 'J', 2);
  g.add_edge('D', 'E', 9);
  g.add_edge('D', 'F', 14);
  g.add_edge('E', 'F', 10);
  g.add_edge('F', 'G', 2);
  g.add_edge('G', 'J', 6);
  g.add_edge('G', 'H', 1);
  g.add_edge('H', 'J', 7);
  return g;
}
graph<char, double> make_graph_wiki() {
  graph<char, double> g;
  g.add_edge('A', 'B', 7);
  g.add_edge('A', 'C', 9);
  g.add_edge('A', 'F', 14);
  g.add_edge('B', 'C', 10);
  g.add_edge('B', 'D', 15);
  g.add_edge('C', 'D', 11);
  g.add_edge('C', 'F', 2);
  g.add_edge('D', 'E', 6);
  g.add_edge('E', 'F', 9);
  return g;
}
graph<std::string, double> make_graph_map() {
  graph<std::string, double> g;
  g.add_edge("London", "Reading", 41);
  g.add_edge("London", "Oxford", 57);
  g.add_edge("Reading", "Swindon", 40);
  g.add_edge("Swindon", "Bristol", 40);
  g.add_edge("Oxford", "Swindon", 30);
  g.add_edge("London", "Southampton", 80);
  g.add_edge("Southampton", "Bournemouth", 33);
  g.add_edge("Bournemouth", "Exeter", 89);
  g.add_edge("Bristol", "Exeter", 83);
  g.add_edge("Bristol", "Bath", 12);
  g.add_edge("Swindon", "Bath", 35);
  g.add_edge("Reading", "Southampton", 50);
  return g;
}
int main() {
  {
    auto g = make_graph();
    char source = 'A';
    std::map<char, double> min_distance;
    std::map<char, char> previous;
    shortest_path(g, source, min_distance, previous);
    for (auto const &kvp : min_distance) {
      std::cout << source << " -> " << kvp.first << " : " << kvp.second << '\t';
      print_path(build_path(previous, kvp.first));
      std::cout << std::endl;
    }
  }
  {
    auto g = make_graph_wiki();
    char source = 'A';
    std::map<char, double> min_distance;
    std::map<char, char> previous;
    shortest_path(g, source, min_distance, previous);
    for (auto const &kvp : min_distance) {
      std::cout << source << " -> " << kvp.first << " : " << kvp.second << '\t';
      print_path(build_path(previous, kvp.first));
      std::cout << std::endl;
    }
  }
  {
    auto g = make_graph_map();
    std::string source = "London";
    std::map<std::string, double> min_distance;
    std::map<std::string, std::string> previous;
    shortest_path(g, source, min_distance, previous);
    for (auto const &kvp : min_distance) {
      std::cout << source << " -> " << kvp.first << " : " << kvp.second << '\t';
      print_path(build_path(previous, kvp.first));
      std::cout << std::endl;
    }
  }
}
```

### 59. The Weasel program


```c++

#include <iostream>
#include <string>
#include <sstream>
#include <string_view>
#include <random>
#include <algorithm>
#include <array>
#include <iomanip>
#include <functional>
class weasel {
  std::string target;
  std::uniform_int_distribution<> chardist;
  std::uniform_real_distribution<> ratedist;
  std::mt19937 mt;
  std::string const allowed_chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ ";
public:
  weasel(std::string_view t) : target(t), chardist(0, 26), ratedist(0, 100) {
    std::random_device rd;
    auto seed_data = std::array<int, std::mt19937::state_size>{};
    std::generate(std::begin(seed_data), std::end(seed_data), std::ref(rd));
    std::seed_seq seq(std::begin(seed_data), std::end(seed_data));
    mt.seed(seq);
  }
  void run(int const copies) {
    auto parent = make_random();
    int step = 1;
    std::cout << std::left << std::setw(5) << std::setfill(' ') << step
              << parent << std::endl;
    do {
      std::vector<std::string> children;
      std::generate_n(std::back_inserter(children), copies,
                      [parent, this]() { return mutate(parent, 5); });
      parent =
          *std::max_element(std::begin(children), std::end(children),
                            [this](std::string_view c1, std::string_view c2) {
                              return fitness(c1) < fitness(c2);
                            });
      std::cout << std::setw(5) << std::setfill(' ') << step << parent
                << std::endl;
      step++;
    } while (parent != target);
  }
private:
  weasel() = delete;
  double fitness(std::string_view candidate) {
    int score = 0;
    for (size_t i = 0; i < candidate.size(); ++i) {
      if (candidate[i] == target[i])
        score++;
    }
    return score;
  }
  std::string mutate(std::string_view parent, double const rate) {
    std::stringstream sstr;
    for (auto const c : parent) {
      auto nc = ratedist(mt) > rate ? c : allowed_chars[chardist(mt)];
      sstr << nc;
    }
    return sstr.str();
  }
  std::string make_random() {
    std::stringstream sstr;
    for (size_t i = 0; i < target.size(); ++i) {
      sstr << allowed_chars[chardist(mt)];
    }
    return sstr.str();
  }
};
int main() {
  weasel w("METHINKS IT IS LIKE A WEASEL");
  w.run(100);
}
```

### 60. The Game of Life


```c++

#include <iostream>
#include <vector>
#include <random>
#include <array>
#include <thread>
#include <chrono>
#include <functional>
class universe {
private:
  universe() = delete;
public:
  enum class seed { random, ten_cell_row, small_explorer, explorer };
public:
  universe(size_t const width, size_t const height)
      : rows(height), columns(width), grid(width * height), dist(0, 4) {
    std::random_device rd;
    auto seed_data = std::array<int, std::mt19937::state_size>{};
    std::generate(std::begin(seed_data), std::end(seed_data), std::ref(rd));
    std::seed_seq seq(std::begin(seed_data), std::end(seed_data));
    mt.seed(seq);
  }
  void
  run(seed const s, int const generations,
      std::chrono::milliseconds const ms = std::chrono::milliseconds(100)) {
    reset();
    initialize(s);
    display();
    int i = 0;
    do {
      next_generation();
      display();
      using namespace std::chrono_literals;
      std::this_thread::sleep_for(ms);
    } while (i++ < generations || generations == 0);
  }
private:
  void next_generation() {
    std::vector<unsigned char> newgrid(grid.size());
    for (size_t r = 0; r < rows; ++r) {
      for (size_t c = 0; c < columns; ++c) {
        auto count = count_neighbors(r, c);
        if (cell(c, r) == alive) {
          newgrid[r * columns + c] = (count == 2 || count == 3) ? alive : dead;
        } else {
          newgrid[r * columns + c] = (count == 3) ? alive : dead;
        }
      }
    }
    grid.swap(newgrid);
  }
  void reset_display() {
#ifdef _WIN32
    system("cls");
#endif
  }
  void display() {
    reset_display();
    for (size_t r = 0; r < rows; ++r) {
      for (size_t c = 0; c < columns; ++c) {
        std::cout << (cell(c, r) ? '*' : ' ');
      }
      std::cout << std::endl;
    }
  }
  void initialize(seed const s) {
    if (s == seed::small_explorer) {
      auto mr = rows / 2;
      auto mc = columns / 2;
      cell(mc, mr) = alive;
      cell(mc - 1, mr + 1) = alive;
      cell(mc, mr + 1) = alive;
      cell(mc + 1, mr + 1) = alive;
      cell(mc - 1, mr + 2) = alive;
      cell(mc + 1, mr + 2) = alive;
      cell(mc, mr + 3) = alive;
    } else if (s == seed::explorer) {
      auto mr = rows / 2;
      auto mc = columns / 2;
      cell(mc - 2, mr - 2) = alive;
      cell(mc, mr - 2) = alive;
      cell(mc + 2, mr - 2) = alive;
      cell(mc - 2, mr - 1) = alive;
      cell(mc + 2, mr - 1) = alive;
      cell(mc - 2, mr) = alive;
      cell(mc + 2, mr) = alive;
      cell(mc - 2, mr + 1) = alive;
      cell(mc + 2, mr + 1) = alive;
      cell(mc - 2, mr + 2) = alive;
      cell(mc, mr - 2) = alive;
      cell(mc + 2, mr + 2) = alive;
    } else if (s == seed::ten_cell_row) {
      for (size_t c = columns / 2 - 5; c < columns / 2 + 5; c++)
        cell(c, rows / 2) = alive;
    } else {
      for (size_t r = 0; r < rows; ++r) {
        for (size_t c = 0; c < columns; ++c) {
          cell(c, r) = dist(mt) == 0 ? alive : dead;
        }
      }
    }
  }
  void reset() {
    for (size_t r = 0; r < rows; ++r) {
      for (size_t c = 0; c < columns; ++c) {
        cell(c, r) = dead;
      }
    }
  }
  int count_alive() { return 0; }
  template <typename T1, typename... T> auto count_alive(T1 s, T... ts) {
    return s + count_alive(ts...);
  }
  int count_neighbors(size_t const row, size_t const col) {
    if (row == 0 && col == 0)
      return count_alive(cell(1, 0), cell(1, 1), cell(0, 1));
    if (row == 0 && col == columns - 1)
      return count_alive(cell(columns - 2, 0), cell(columns - 2, 1),
                         cell(columns - 1, 1));
    if (row == rows - 1 && col == 0)
      return count_alive(cell(0, rows - 2), cell(1, rows - 2),
                         cell(1, rows - 1));
    if (row == rows - 1 && col == columns - 1)
      return count_alive(cell(columns - 1, rows - 2),
                         cell(columns - 2, rows - 2),
                         cell(columns - 2, rows - 1));
    if (row == 0 && col > 0 && col < columns - 1)
      return count_alive(cell(col - 1, 0), cell(col - 1, 1), cell(col, 1),
                         cell(col + 1, 1), cell(col + 1, 0));
    if (row == rows - 1 && col > 0 && col < columns - 1)
      return count_alive(cell(col - 1, row), cell(col - 1, row - 1),
                         cell(col, row - 1), cell(col + 1, row - 1),
                         cell(col + 1, row));
    if (col == 0 && row > 0 && row < rows - 1)
      return count_alive(cell(0, row - 1), cell(1, row - 1), cell(1, row),
                         cell(1, row + 1), cell(0, row + 1));
    if (col == columns - 1 && row > 0 && row < rows - 1)
      return count_alive(cell(col, row - 1), cell(col - 1, row - 1),
                         cell(col - 1, row), cell(col - 1, row + 1),
                         cell(col, row + 1));
    return count_alive(cell(col - 1, row - 1), cell(col, row - 1),
                       cell(col + 1, row - 1), cell(col + 1, row),
                       cell(col + 1, row + 1), cell(col, row + 1),
                       cell(col - 1, row + 1), cell(col - 1, row));
  }
  unsigned char &cell(size_t const col, size_t const row) {
    return grid[row * columns + col];
  }
private:
  size_t rows;
  size_t columns;
  std::vector<unsigned char> grid;
  const unsigned char alive = 1;
  const unsigned char dead = 0;
  std::uniform_int_distribution<> dist;
  std::mt19937 mt;
};
int main() {
  using namespace std::chrono_literals;
  universe u(50, 20);
  u.run(universe::seed::random, 100, 100ms);
}
```












