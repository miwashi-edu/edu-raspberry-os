# edu-raspberry-os

> Testing

### Login

```bash
```

## Instructions

### Scaffold Project

```bash
cd ~
cd ws
mkdir -p zero-project
cd zero-project
mkdir tests
touch ./include/Calculator.h
touch ./src/Calculator.cpp
touch ./tests/CMakeLists.txt
touch ./tests/test_calculator.cpp
```

### CMakeLists.txt (Project Structure)

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(zero-project LANGUAGES CXX)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(CTest)

add_subdirectory(src)
add_subdirectory(tests)
EOF
```

### ./src/CMakeLists.txt (Library and Executable)

```bash
cat > ./src/CMakeLists.txt << EOF
add_library(calculator Calculator.cpp)
target_include_directories(calculator PUBLIC \${CMAKE_SOURCE_DIR}/include)

add_executable(hello main.cpp)
target_link_libraries(hello calculator)
EOF
```

### src/main.cpp

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include "Calculator.h"
using namespace std;

int main() {
    Calculator calc;
    cout << "2 + 3 = " << calc.add(2, 3) << endl;
    return 0;
}
EOF
```

### include/Calculator.h

```bash
cat > ./include/Calculator.h << EOF
#pragma once

class Calculator {
public:
    int add(int a, int b);
    int subtract(int a, int b);
};
EOF
```

### src/Calculator.cpp

```bash
cat > ./src/Calculator.cpp << EOF
#include "Calculator.h"

int Calculator::add(int a, int b) {
    return a + b;
}

int Calculator::subtract(int a, int b) {
    return a - b;
}
EOF
```

### tests/CMakeLists.txt (GoogleTest and Unit Tests)

```bash
cat > ./tests/CMakeLists.txt << EOF
include(FetchContent)

FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.14.0.zip
)

FetchContent_MakeAvailable(googletest)

enable_testing()

add_executable(test_calculator test_calculator.cpp)
target_link_libraries(test_calculator gtest_main calculator)

add_test(NAME CalculatorTests COMMAND test_calculator)
EOF
```

### tests/test_calculator.cpp

```bash
cat > ./tests/test_calculator.cpp << EOF
#include <gtest/gtest.h>
#include "Calculator.h"

TEST(CalculatorTest, Addition) {
    Calculator calc;
    EXPECT_EQ(calc.add(2, 3), 5);
}

TEST(CalculatorTest, Subtraction) {
    Calculator calc;
    EXPECT_EQ(calc.subtract(5, 3), 2);
}
EOF
```

### Build the project

```bash
cmake -B build
make -C build
./bin/hello
```

### Run tests

```bash
cmake -B build
make -C build test
```

### Reset to commit or delete zero-project

#### Delete Project

```bash
cd ~
cd ws
rm -rf zero-project
```

#### Reset to Commit

```bash
cd ~
cd ws
cd zero-project
git reset --hard
git clean -df
```
