##laboratory work III

Current task: 1

1. Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.

script:
```shell
cd lab03/formatter_lib
touch CMakeLists.txt
cat >> CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter)

add_library(formatter STATIC formatter.cpp)
EOF

```

____________________________________

Current task: 2

2. У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.

script:
```shell
cd ..
cd formatter_ex_lib
touch CMakeLists.txt
cat >> CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)

include_directories(../formatter_lib)
add_subdirectory(../formatter_lib formatter)

add_library(formatter_ex STATIC formatter_ex.cpp)

target_link_libraries(formatter_ex formatter)
> EOF
```


Current task: 3

3. Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

[-]hello_world, которое использует библиотеку formatter_ex;
[-]solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.

#hello_world

script:
```shell
cd hello_world_application
touch CMakeLists.txt
cat >> CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_world)

add_executable(hello_world hello_world.cpp)

include_directories(../formatter_ex_lib)
add_subdirectory(../formatter_ex_lib formatter_ex)

target_link_libraries(hello_world formatter_ex)
> EOF


```

```shell
cmake -H. -B_build
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/jamalay/Dzhamalay/workspace/projects/LAB03/lab03/hello_world_application/_build

cmake --build _build
[ 16%] Building CXX object formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter.a
[ 33%] Built target formatter
[ 50%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex.a
[ 66%] Built target formatter_ex
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world

cmake --build _build --target hello_world
Consolidate compiler generated dependencies of target formatter
[ 33%] Built target formatter
Consolidate compiler generated dependencies of target formatter_ex
[ 66%] Built target formatter_ex
Consolidate compiler generated dependencies of target hello_world
[100%] Built target hello_world

./_build/hello_world
-------------------------
hello, world!
-------------------------
```

#solver

```shell
cd ..
cd solver_lib
touch CMakeLists.txt
cat >> CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_lib)

add_library(solver_lib STATIC solver.cpp)
EOF
```

```shell
cd ..
cd solver_application
cat >> CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver)

add_executable(solver equation.cpp)

include_directories(../formatter_ex_lib ../solver_lib)

add_subdirectory(../formatter_ex_lib formatter_ex)
add_subdirectory(../solver_lib solver_lib)

target_link_libraries(solver formatter_ex solver_lib)
> EOF
```

```shell
cmake -H. -B_build
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/jamalay/Dzhamalay/workspace/projects/LAB03/lab03/solver_application/_build

cmake --build _build
Consolidate compiler generated dependencies of target solver_lib
[ 12%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 62%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex.a
[ 75%] Built target formatter_ex
[ 87%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver

cmake --build _build --target solver
Consolidate compiler generated dependencies of target solver_lib
[ 25%] Built target solver_lib
Consolidate compiler generated dependencies of target formatter
[ 50%] Built target formatter
Consolidate compiler generated dependencies of target formatter_ex
[ 75%] Built target formatter_ex
Consolidate compiler generated dependencies of target solver
[100%] Built target solver

./_build/solver
1
2
1
-------------------------
x1 = -1.000000
-------------------------
-------------------------
x2 = -1.000000
-------------------------

```











