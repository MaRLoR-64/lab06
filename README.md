## Laboratory work 5
#Настройка окружения 
```bash
student@LabPythonVM:~$ export GITHUB_USERNAME=MaRLoR-64
student@LabPythonVM:~$ alias gsed=sed # for *-nix system
student@LabPythonVM:~$ cd ${GITHUB_USERNAME}/workspace
student@LabPythonVM:~/MaRLoR-64/workspace$ pushd .
~/MaRLoR-64/workspace ~/MaRLoR-64/workspace
student@LabPythonVM:~/MaRLoR-64/workspace$ source scripts/activate
student@LabPythonVM:~/MaRLoR-64/workspace$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
Клонирование в «projects/lab05»...
remote: Enumerating objects: 33, done.
remote: Counting objects: 100% (33/33), done.
remote: Compressing objects: 100% (24/24), done.
remote: Total 33 (delta 8), reused 26 (delta 5), pack-reused 0 (from 0)
Получение объектов: 100% (33/33), 11.89 КиБ | 296.00 КиБ/с, готово.
Определение изменений: 100% (8/8), готово.
student@LabPythonVM:~/MaRLoR-64/workspace$ cd projects/lab05
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git remote remove origin
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ mkdir third-party
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git submodule add https://github.com/google/googletest third-party/gtest
Клонирование в «/home/student/MaRLoR-64/workspace/projects/lab05/third-party/gtest»...
remote: Enumerating objects: 27977, done.
remote: Counting objects: 100% (224/224), done.
remote: Compressing objects: 100% (142/142), done.
remote: Total 27977 (delta 143), reused 82 (delta 82), pack-reused 27753 (from 3)
Получение объектов: 100% (27977/27977), 13.46 МиБ | 260.00 КиБ/с, готово.
Определение изменений: 100% (20733/20733), готово.
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
Примечание: переключение на «release-1.8.1».

Вы сейчас в состоянии «отсоединённого указателя HEAD». Можете осмотреться,
внести экспериментальные изменения и зафиксировать их, также можете
отменить любые коммиты, созданные в этом состоянии, не затрагивая другие
ветки, переключившись обратно на любую ветку.

Если хотите создать новую ветку для сохранения созданных коммитов, можете
сделать это (сейчас или позже), используя команду switch с параметром -c.
Например:
git switch -c <новая-ветка>

Или отмените эту операцию с помощью:

  git switch -

Отключите этот совет, установив переменную конфигурации
advice.detachedHead в значение false

HEAD сейчас на 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git add third-party/gtest
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git commit -m"added gtest framework"
[master 2981c0d] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
```
#Добавление Google Test как submodule
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
' CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cat >> CMakeLists.txt <<EOF
> if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ mkdir tests
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cat > tests/test1.cpp <<EOF
> #include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cmake -H. -B_build -DBUILD_TESTS=ON
-- The C compiler identification is GNU 12.2.0
-- The CXX compiler identification is GNU 12.2.0
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
CMake Deprecation Warning at third-party/gtest/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 2.8.12 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at third-party/gtest/googlemock/CMakeLists.txt:42 (cmake_minimum_required):
  Compatibility with CMake < 2.8.12 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at third-party/gtest/googletest/CMakeLists.txt:49 (cmake_minimum_required):
  Compatibility with CMake < 2.8.12 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Found PythonInterp: /usr/bin/python3 (found version "3.11.2") 
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/student/MaRLoR-64/workspace/projects/lab05/_build
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cmake --build _build
[ 16%] Built target print
[ 33%] Built target gtest
[ 50%] Built target gtest_main
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library libgmock.a
[ 83%] Built target gmock
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main

```
#Запуск проекта
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cmake --build _build --target test
Running tests...
Test project /home/student/MaRLoR-64/workspace/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ _build/check
Running main() from /home/student/MaRLoR-64/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ cmake --build _build --target test -- ARGS=--verbose
Running tests...
UpdateCTestConfiguration  from :/home/student/MaRLoR-64/workspace/projects/lab05/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/student/MaRLoR-64/workspace/projects/lab05/_build/DartConfiguration.tcl
Test project /home/student/MaRLoR-64/workspace/projects/lab05/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/student/MaRLoR-64/workspace/projects/lab05/_build/check
1: Working Directory: /home/student/MaRLoR-64/workspace/projects/lab05/_build
1: Test timeout computed to be: 10000000

1: Running main() from /home/student/MaRLoR-64/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (0 ms)
1: [----------] 1 test from Print (0 ms total)
1: 
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ gsed -i 's/lab04/lab05/g' README.md
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ gsed -i '/cmake --build _build --target install/a\
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git add tests
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git add -p
diff --git a/.travis.yml b/.travis.yml
index 876ee12..f89849d 100644
--- a/.travis.yml
+++ b/.travis.yml
@@ -1,8 +1,9 @@
 language: cpp
 script:
-- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
+- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON
 - cmake --build _build
- cmake --build _build --target install
+- cmake --build _build --target test -- ARGS=--verbose
 addons:
   apt:
     sources:
(1/1) Индексировать этот блок [y,n,q,a,d,s,e,?]? y 

diff --git a/CMakeLists.txt b/CMakeLists.txt
index 96a361e..89739e7 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -4,6 +4,7 @@ set(CMAKE_CXX_STANDARD 11)
 set(CMAKE_CXX_STANDARD_REQUIRED ON)
 
 option(BUILD_EXAMPLES "Build examples" OFF)
+option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
 
(1/2) Индексировать этот блок [y,n,q,a,d,j,J,g,/,e,?]? y
@@ -34,3 +35,11 @@ install(TARGETS print
 
 install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
 install(EXPORT print-config DESTINATION cmake)
+if(BUILD_TESTS)
+  enable_testing()
+  add_subdirectory(third-party/gtest)
+  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
+  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
+  target_link_libraries(check ${PROJECT_NAME} gtest_main)
+  add_test(NAME check COMMAND check)
+endif()
(2/2) Индексировать этот блок [y,n,q,a,d,K,g,/,e,?]? y
diff --git a/README.md b/README.md
index d5a3349..cb775a0 100644
--- a/README.md
+++ b/README.md
@@ -20,10 +20,10 @@ gem install travis
 ```
 #Клонирование и настройка репозитория
 ```sh
-git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04
-cd projects/lab04
+git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab05
+cd projects/lab05
 git remote remove origin
-git remote add origin https://github.com/${GITHUB_USERNAME}/lab04
+git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
 ```
 #Создание конфигурации Travis CLI
 ```sh
(1/1) Индексировать этот блок [y,n,q,a,d,s,e,?]? y

```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ git push origin master
Username for 'https://github.com': MaRLoR-64
Password for 'https://MaRLoR-64@github.com': 
Перечисление объектов: 44, готово.
Подсчет объектов: 100% (44/44), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (30/30), готово.
Запись объектов: 100% (44/44), 13.43 КиБ | 13.43 МиБ/с, готово.
Всего 44 (изменений 13), повторно использовано 30 (изменений 8), повторно использовано пакетов 0
remote: Resolving deltas: 100% (13/13), done.
To https://github.com/MaRLoR-64/lab05
 * [new branch]      master -> master
```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab05$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png

```





