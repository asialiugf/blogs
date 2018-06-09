### LINQ 使用

https://github.com/pfultz2/Linq

```c++
git clone https://github.com/pfultz2/Linq

cd Linq
mkdir build
cd build
cmake ..
make
sudo make install
```

### sample
```c++
cd Linq
cat tt.cpp
//----
@:~/github.com/Linq$ cat tt.cpp
#include "linq.h"
#include <chrono>
#include <iostream>

struct student_t {
  std::string last_name;
  std::vector<int> scores;
};

std::vector<student_t> students = {
  {"Omelchenko", {97, 72, 81, 60}},
  {"O'Donnell", {75, 84, 91, 39}},
  {"Mortensen", {88, 94, 65, 85}},
  {"Garcia", {97, 89, 85, 82}},
  {"Beebe", {35, 72, 91, 70}}
};

int main()
{
  std::chrono::time_point<std::chrono::steady_clock> start, end;
  start = std::chrono::steady_clock::now();

  auto scores = LINQ(from(student, students)
                     from(score, student.scores)
                     where(score > 60)
                     select(std::make_pair(student.last_name, score)));

  end = std::chrono::steady_clock::now();

  for(auto x : scores) {
    printf("%s score: %i\n", x.first.c_str(), x.second);
  }

  //std::chrono::duration<double> titi = end - start ;
  std::chrono::nanoseconds elapsed = end - start ;

  std::cout << elapsed.count() << std::endl;
}
@:~/github.com/Linq$ 
```
```c++
 /usr/bin/g++-7  tt.cpp  -o linq-test 
 ```
 ```c++
  @ :~/github.com/Linq$ ./linq-test 
Omelchenko score: 97
Omelchenko score: 72
Omelchenko score: 81
O'Donnell score: 75
O'Donnell score: 84
O'Donnell score: 91
Mortensen score: 88
Mortensen score: 94
Mortensen score: 65
Mortensen score: 85
Garcia score: 97
Garcia score: 89
Garcia score: 85
Garcia score: 82
Beebe score: 72
Beebe score: 91
Beebe score: 70
14648
 @ :~/github.com/Linq$ 
```
