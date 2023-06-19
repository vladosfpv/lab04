# Report-03

# Task 1

Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории `formatter_lib`. В этой директории находятся файлы для статической библиотеки `formatter`. Создайте `CMakeList.txt` в директории `formatter_lib`, с помощью которого можно будет собирать статическую библиотеку formatter

```
$ echo "# lab-03" >> README.md
$ git init
$ git add README.md
$ git commit -m "fork"
$ git remote add origin https://github.com/ledibonibell/lab-03.git
$ git push -u origin master
```
![image](https://github.com/ledibonibell/lab-03/assets/57360144/2a5c88a9-7173-4196-98ea-2fbfc4b71625)


```
$ git checkout -b lab
$ cat >> CMakeLists.txt << EOF
> EOF
$ nano CMakeLists.txt
```


Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter)

add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

- Команда cmake_minimum_required указывает подходящую минимальную версию Cmake
- Команды set(CMAKE_CXX_STANDARD 11), set(CMAKE_CXX_STANDARD_REQUIRED ON) устанавливают значения стандартных переменных в Cmake(в данном случае CMAKE_CXX_STANDARD и CMAKE_CXX_STANDARD_REQUIRED)
- Команда project(formatter) создает проект formatter, к которому можно подключать библиотеки, исполняемы файлы и т.д.
- Команда add_library(formatter STATIC formatter.cpp) создает статическую библиотеку из указываемых файлов
- Команда target_include_directories связывает библиотеку formatter и CMAKE_CURRENT_SOURCE_DIR

```
$ git add CMakeLists.txt
$ git commit -m "added CMakeLists.txt"
$ git push origin lab
$ git add formatter.h
$ git commit -m "added formatter.h"
$ git push origin lab
$ git add formatter.cpp
$ git commit -m "added formatter.cpp"
$ git push origin lab
```

Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build

```
![image](https://github.com/ledibonibell/lab-03/assets/57360144/4869eb71-6bb2-49bb-8895-953b5921e2c7)



# Task 2

У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием `CMakeList.txt` для статической библиотеки formatter, ваш руководитель поручает заняться созданием `CMakeList.txt` для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter

```
$ cat >> CMakeLists.txt << EOF
> EOF
$ nano CMakeLists.txt
```

Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)

add_library(formatter_ex STATIC formatter_ex.cpp)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter)
target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_link_libraries(formatter_ex formatter)
```

- Команда add_subdirectory(../formatter_lib formatter) включает в сборку директорию с библиотекой formatter
- Команда target_link_libraries(formatter_ex formatter) связывает библиотеку formatter с formatter_ex
- Остальные команды аналогичны первому заданию

```
$ git add CMakeLists.txt
$ git commit -m "added CMakeLists.txt"
$ git push origin lab
$ git add formatter_ex.h
$ git commit -m "added formatter_ex.h"
$ git push origin lab
$ git add formatter_ex.cpp
$ git commit -m "added formatter_ex.cpp"
$ git push origin lab
```

Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build
```

![image](https://github.com/ledibonibell/lab-03/assets/57360144/1e207601-4813-41b5-9fb2-b0efa6178f99)


# Task 3
Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой `formatter_ex`, вам необходимо создать два `CMakeList.txt` для двух простых приложений:

- `hello_world`, которое использует библиотеку `formatter_ex`;
- `solver`, приложение которое испольует статические библиотеки `formatter_ex` и `solver_lib`

Удачной стажировки!

### solver.cpp

```
$ cat >> CMakeLists.txt << EOF
> EOF
$ nano CMakeLists.txt
```


Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_lib)

add_library(solver_lib STATIC solver.cpp)
target_include_directories(solver_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

- Команды аналогичны заданию 2 и 3

```
$ git add CMakeLists.txt
$ git commit -m "added CMakeLists.txt"
$ git push origin lab
$ git add solver.h
$ git commit -m "added solver.h"
$ git push origin lab
$ git add solver.cpp
$ git commit -m "added solver.cpp"
$ git push origin lab
```

Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build
```
![image](https://github.com/ledibonibell/lab-03/assets/57360144/9098fbb5-4481-4b7b-a9ff-73d80112c2da)



### solver_example

```
$ cat >> CMakeLists.txt << EOF
> EOF
$ nano CMakeLists.txt
```

Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_example)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib ${CMAKE_CURRENT_BINARY_DIR}/solver_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter_ex)

add_executable(example equation.cpp)

target_link_libraries(example solver_lib formatter_ex)
```

- Команды аналогичны заданию 2 и 3

```
$ git add CMakeLists.txt
$ git commit -m "added CMakeLists.txt"
$ git push origin lab
$ git add equation.cpp
$ git commit -m "added equation.cpp"
$ git push origin lab
```

Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build
```


Демонстрируем исполнение программы:
```
$ cmake --build _build --target example
$ ./_build/example
```

### hello_world.cpp

```
$ git checkout -b lab
$ cat >> CMakeLists.txt << EOF
> EOF
$ nano CMakeLists.txt
```


Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_world_example)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter_ex)
add_executable(example hello_world.cpp)

target_link_libraries(example formatter_ex)
```

- Команды аналогичны заданию 2 и 3

```
$ git add CMakeLists.txt
$ git commit -m "added CMakeLists.txt"
$ git push origin lab
$ git add hello_world.cpp
$ git commit -m "added hello_world.cpp"
$ git push origin lab
```

Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build
```
![image](https://github.com/ledibonibell/lab-03/assets/57360144/2f57af9c-5239-440b-9738-89e71b28fe79)



Демонстрируем исполнение программы:
```
$ cmake --build _build --target example
$ ./_build/example
```
![image](https://github.com/ledibonibell/lab-03/assets/57360144/1707ee34-dd6c-4ab7-b670-16edc631a92f)



## Required libraries
```
$ sudo apt install cmake - устанавливаем пакет cmake
```
