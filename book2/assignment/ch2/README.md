## 笔记
#### 1.g++常用参数
参考[g＋＋编译命令选项](https://blog.csdn.net/woshinia/article/details/11060797)
- `-o` 指定输出文件名[g++ -o Test.exe Test.cpp]
- `-E` 预处理,生成.i的文件 [g++ -E Test.cpp > Test.i ]
- `-S` 将预处理后的文件不转换成汇编语言,生成文件.s  [g++ -S Test.cpp]
- `-c` 由汇编变为目标代码(机器代码)生成.o的文件 [g++ -c Test.cpp ]
-  `-L` 连接目标代码,生成可执行程序[g++ Test.o -L F:\vs2008\VC\include\iostream]

#### 2.cmake 基本操作
``` cmake
# 声明要求的 cmake 最低版本
cmake_minimum_required(VERSION 2.8)

# 声明一个 cmake 工程
project(HelloSLAM)

# 设置编译模式
set(CMAKE_BUILD_TYPE "Debug")

# 添加一个可执行程序
# 语法：add_executable( 程序名 源代码文件 ）
add_executable(helloSLAM helloSLAM.cpp)

# 添加hello库
add_library(hello libHelloSLAM.cpp)
# 共享库
add_library(hello_shared SHARED libHelloSLAM.cpp)

# 添加可执行程序调用hello库中函数
add_executable(useHello useHello.cpp)
# 将库文件链接到可执行程序上
target_link_libraries(useHello hello_shared)
```

#### 3.修改cmake导致make报错
- 注释
``` cmake
# 将库文件链接到可执行程序上
# target_link_libraries(useHello hello_shared)
```
报错
``` cmake
Scanning dependencies of target useHello
[ 12%] Building CXX object CMakeFiles/useHello.dir/useHello.cpp.o
[ 25%] Linking CXX executable useHello
CMakeFiles/useHello.dir/useHello.cpp.o：在函数‘main’中：
/home/heolis/CLionProjects/slambook2/ch2/useHello.cpp:5：对‘printHello()’未定义的引用
collect2: error: ld returned 1 exit status
CMakeFiles/useHello.dir/build.make:94: recipe for target 'useHello' failed
make[2]: *** [useHello] Error 1
CMakeFiles/Makefile2:67: recipe for target 'CMakeFiles/useHello.dir/all' failed
make[1]: *** [CMakeFiles/useHello.dir/all] Error 2
Makefile:83: recipe for target 'all' failed
make: *** [all] Error 2
```
- 注释
``` cmake
# 共享库
# add_library(hello_shared SHARED libHelloSLAM.cpp)
```
报错
``` cmake 
Scanning dependencies of target useHello
[ 16%] Building CXX object CMakeFiles/useHello.dir/useHello.cpp.o
[ 33%] Linking CXX executable useHello
/usr/bin/ld: 找不到 -lhello_shared
collect2: error: ld returned 1 exit status
CMakeFiles/useHello.dir/build.make:94: recipe for target 'useHello' failed
make[2]: *** [useHello] Error 1
CMakeFiles/Makefile2:67: recipe for target 'CMakeFiles/useHello.dir/all' failed
make[1]: *** [CMakeFiles/useHello.dir/all] Error 2
Makefile:83: recipe for target 'all' failed
make: *** [all] Error 2
```


