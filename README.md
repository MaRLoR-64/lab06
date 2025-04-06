## Laboratory work VI
#Подготовка рабойчей директории
```sh 
student@LabPythonVM:~/MaRLoR-64/workspace$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab06
Клонирование в «projects/lab06»...
remote: Enumerating objects: 47, done.
remote: Counting objects: 100% (47/47), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 47 (delta 14), reused 43 (delta 13), pack-reused 0 (from 0)
Получение объектов: 100% (47/47), 18.13 КиБ | 269.00 КиБ/с, готово.
Определение изменений: 100% (14/14), готово.
student@LabPythonVM:~/MaRLoR-64/workspace$ cd projects/lab06
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git remote remove origin
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
```
#Настройка Cmake
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ gsed -i '/project(print)/a\
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git diff
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 89739e7..66530c1 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -7,6 +7,10 @@ option(BUILD_EXAMPLES "Build examples" OFF)
 option(BUILD_TESTS "Build tests" OFF)

 project(print)
+set(PRINT_VERSION_MAJOR 0)
+set(PRINT_VERSION_MINOR 1)
+set(PRINT_VERSION_PATCH 0)
+set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
 
 add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
 
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ touch DESCRIPTION && edit DESCRIPTION
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ touch ChangeLog.md
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ export DATE="`LANG=en_US date +'%a %b %d %Y'`"
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat > ChangeLog.md <<EOF
> ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
```
#Настройка Сpack
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat >> CPackConfig.cmake <<EOF
> set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat >> CPackConfig.cmake <<EOF
> set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat >> CPackConfig.cmake <<EOF
> set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat >> CPackConfig.cmake <<EOF
> set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat >> CPackConfig.cmake <<EOF
> include(CPack)
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cat >> CMakeLists.txt <<EOF
> include(CPackConfig.cmake)
> EOF
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ gsed -i 's/lab05/lab06/g' README.md
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git add .
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git commit -m"added cpack config"
[master c4e329a] added cpack config
 5 files changed, 67 insertions(+), 40 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION
#Сборка пакета 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git tag v0.1.0.0
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ git push origin master --tags
Username for 'https://github.com': MaRLoR-64
Password for 'https://MaRLoR-64@github.com': 
Перечисление объектов: 53, готово.
Подсчет объектов: 100% (53/53), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (33/33), готово.
Запись объектов: 100% (53/53), 19.17 КиБ | 19.17 МиБ/с, готово.
Всего 53 (изменений 18), повторно использовано 43 (изменений 14), повторно использовано пакетов 0
remote: Resolving deltas: 100% (18/18), done.
To https://github.com/MaRLoR-64/lab06
 * [new branch]      master -> master
 * [new tag]         v0.1.0.0 -> v0.1.0.0
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cmake -H. -B_build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/student/MaRLoR-64/workspace/projects/lab06/_build
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cd _build
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06/_build$ cpack -G "TGZ"
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/student/MaRLoR-64/workspace/projects/lab06/_build/print-0.1.0-Linux.tar.gz generated.
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06/_build$ cd ..
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
-- Configuring done
-- Generating done
-- Build files have been written to: /home/student/MaRLoR-64/workspace/projects/lab06/_build
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ cmake --build _build --target package
100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/student/MaRLoR-64/workspace/projects/lab06/_build/print-0.1.0-Linux.tar.gz generated.
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ mkdir artifacts
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ mv _build/*.tar.gz artifacts
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ tree artifacts
artifacts
└── print-0.1.0-Linux.tar.gz

1 directory, 1 file
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab06$ popd
~/MaRLoR-64/workspace
student@LabPythonVM:~/MaRLoR-64/workspace$ export LAB_NUMBER=06
student@LabPythonVM:~/MaRLoR-64/workspace$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
Клонирование в «tasks/lab06»...
remote: Enumerating objects: 117, done.
remote: Counting objects: 100% (37/37), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 117 (delta 35), reused 33 (delta 33), pack-reused 80 (from 1)
Получение объектов: 100% (117/117), 1.33 МиБ | 70.00 КиБ/с, готово.
Определение изменений: 100% (36/36), готово.
```
