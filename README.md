## Ручная компиляция исходных файлов 
```bash
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ g++ -std=c++11 -I./include -c sources/print.cpp
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ ls print.o
print.o
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ nm print.o | grep print
000000000000009e t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ ar rvs print.a print.o
ar: создаётся print.a
a - print.o
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ file print.a
print.a: current ar archive
```
##Компиляция и запуск example1
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ g++ -std=c++11 -I./include -c examples/example1.cpp
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ ls example1.o
example1.o
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ g++ example1.o print.a -o example1
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ ./example1 && echo
hello

```
##Компиляция и запуск example2
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ g++ -std=c++11 -I./include -c examples/example2.cpp
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ nm example2.o
                 U __cxa_atexit
                 U __dso_handle
0000000000000000 V DW.ref.__gxx_personality_v0
                 U _GLOBAL_OFFSET_TABLE_
0000000000000133 t _GLOBAL__sub_I_main
                 U __gxx_personality_v0
0000000000000000 T main
                 U _Unwind_Resume
00000000000000e1 t _Z41__static_initialization_and_destruction_0ii
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSaIcEC1Ev
                 U _ZNSaIcED1Ev
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZNSt8ios_base4InitC1Ev
                 U _ZNSt8ios_base4InitD1Ev
0000000000000000 r _ZStL19piecewise_construct
0000000000000000 b _ZStL8__ioinit
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ g++ example2.o print.a -o example2
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ ./example2
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat log.txt && ech
```
##Отчитска временных файлов 
```student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ rm -rf example1.o example2.o print.o
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ rm -rf print.a
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ rm -rf example1 example2
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ rm -rf log.txt
```
##Создание базового СMakeLists.txt
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.4)
project(print)
> EOF

```
##Настройки сбокри библиотеки 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
> EOF

```
```sh 
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
> EOF

```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
> EOF

```
###Первоначальная сборка проекта 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat >> CMakeLists.txt <<EOF
> target_link_libraries(example1 print)
target_link_libraries(example2 print)
 EOF

```
##Полная сборка и тестирование 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cmake --build _build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/student/MaRLoR-64/workspace/projects/lab03/_build
[ 33%] Built target print
[ 50%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[ 66%] Linking CXX executable example1
[ 66%] Built target example1
[ 83%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[100%] Linking CXX executable example2
[100%] Built target example2
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cmake --build _build --target print
[100%] Built target print
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cmake --build _build --target example1
[ 50%] Built target print
[100%] Built target example1
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cmake --build _build --target example2
[ 50%] Built target print
[100%] Built target example2
```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ ls -la _build/libprint.a
-rw-r--r-- 1 student student 2982 апр  2 01:40 _build/libprint.a
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ _build/example1 && echo
hello
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ _build/example2
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat log.txt && echo
hello
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ rm -rf log.txt
```
##Обновление СMakeLists.txt из репозитория 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ git clone https://github.com/tp-labs/lab03 tmp
Клонирование в «tmp»...
remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Получение объектов: 100% (91/91), 1.02 МиБ | 352.00 КиБ/с, готово.
Определение изменений: 100% (41/41), готово.
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ mv -f tmp/CMakeLists.txt .
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ rm -rf tmp

```
#Установка проекта 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cat CMakeLists.txt
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
     RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
-- Configuring done
-- Generating done
-- Build files have been written to: /home/student/MaRLoR-64/workspace/projects/lab03/_build
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ cmake --build _build --target install
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /home/student/MaRLoR-64/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/student/MaRLoR-64/workspace/projects/lab03/_install/include
-- Installing: /home/student/MaRLoR-64/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/student/MaRLoR-64/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/student/MaRLoR-64/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ tree _install
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ sudo apt install tree
Чтение списков пакетов… Готово
Построение дерева зависимостей… Готово
Чтение информации о состоянии… Готово         
Следующие пакеты устанавливались автоматически и больше не требуются:
  libdbus-glib-1-2 libwpe-1.0-1 libwpebackend-fdo-1.0-1
Для их удаления используйте «sudo apt autoremove».
Следующие НОВЫЕ пакеты будут установлены:
  tree
Обновлено 0 пакетов, установлено 1 новых пакетов, для удаления отмечено 0 пакетов, и 82 пакетов не обновлено.
Необходимо скачать 52,5 kB архивов.
После данной операции объём занятого дискового пространства возрастёт на 116 kB.
Пол:1 http://deb.debian.org/debian bookworm/main amd64 tree amd64 2.1.0-1 [52,5 kB]
Получено 52,5 kB за 0с (183 kB/s)
ыбор ранее не выбранного пакета tree.
(Чтение базы данных … на данный момент установлено 186298 файлов и каталогов.)
Подготовка к распаковке …/tree_2.1.0-1_amd64.deb …
Распаковывается tree (2.1.0-1) …
Настраивается пакет tree (2.1.0-1) …
Обрабатываются триггеры для man-db (2.11.2-2) …
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ tree _install
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
   └── libprint.a

4 directories, 4 files

```
#Фиксация в Git
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ git add CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ git commit -m"added CMakeLists.txt"
[master 772ab3f] added CMakeLists.txt
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab03$ git push origin master
Username for 'https://github.com': MaRLoR-64
Password for 'https://MaRLoR-64@github.com': 
Перечисление объектов: 18, готово.
Подсчет объектов: 100% (18/18), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (14/14), готово.
Запись объектов: 100% (18/18), 3.59 КиБ | 3.59 МиБ/с, готово.
Всего 18 (изменений 1), повторно использовано 14 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/MaRLoR-64/lab03.git
 * [new branch]      master -> master

```

