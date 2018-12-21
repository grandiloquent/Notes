# The Modern C++ Challenge

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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


```

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




