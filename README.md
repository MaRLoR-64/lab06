#LabWork02
##Создание конфигурационного файла
```bash
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ git pull origin master
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0 (from 0)
Распаковка объектов: 100% (3/3), 200 байтов | 200.00 КиБ/с, готово.
Из https://github.com/MaRLoR-64/lab02
 * branch            master     -> FETCH_HEAD
 * [новая ветка]     master     -> origin/master
```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ git log 
commit 1c740be3144fa73bb00d063c1dc277d88c61b474 (HEAD -> master, origin/master)
Author: MaRLoR-64 <erohin2512@icloud.com>
Date:   Wed Apr 2 00:49:40 2025 +0300

    added README.md
```
##Создание рабочих папок
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ mkdir sources
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ mkdir include
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ mkdir examples
```
##Создание когда 
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ cat > sources/print.cpp <<EOF
> #include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
> EOF

```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ cat > include/print.hpp <<EOF
> #include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
> EOF

```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ cat > examples/example1.cpp <<EOF
> #include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
> EOF

```
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ cat > examples/example2.cpp <<EOF
> #include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
> EOF
```
##Перенос файлов на GitHub
```sh
student@LabPythonVM:~/MaRLoR-64/workspace/projects/lab02$ git commit -m"added sources"
[master 3734454] added sources
 4 files changed, 32 insertions(+)
 create mode 100644 examples/example1.cpp
 create mode 100644 examples/example2.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp

```




