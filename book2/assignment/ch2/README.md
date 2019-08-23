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

## 运行ORB-SLAM2
ORB-SLAM2[4] 是一个非常经典的视觉 SLAM 开源方案,它可以作为你学习 SLAM 的范本。

### 安装
#### CUDA9.0
自行安装cuda9.0

#### Pangolin
首先安装Pangolin,这是ORB-SLAM用的可视化框架.
```
sudo apt install libglew-dev
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build
cd build
cmake ..
cmake --build .
```

#### ORB-SLAM2
安装完毕
```
git clone https://github.com/raulmur/ORB_SLAM2.git ORB_SLAM2
cd ORB_SLAM2
chmod +x build.sh
./build.sh
```

> 如果报错: error: ‘usleep’ was not declared in this scope

在include/System.h文件添加`#include <unistd.h>`

### 调用摄像头

``` C++
//
// Created by xiang on 11/29/17.
//

// 该文件将打开你电脑的摄像头，并将图像传递给ORB-SLAM2进行定位

// 需要opencv
#include <opencv2/opencv.hpp>

// ORB-SLAM的系统接口
#include "System.h"

#include <string>
#include <chrono>   // for time stamp
#include <iostream>

using namespace std;

// 参数文件与字典文件
// 如果你系统上的路径不同，请修改它
string parameterFile = "./myslam.yaml";
string vocFile = "./Vocabulary/ORBvoc.txt";

int main(int argc, char **argv) {

    // 声明 ORB-SLAM2 系统
    ORB_SLAM2::System SLAM(vocFile, parameterFile, ORB_SLAM2::System::MONOCULAR, true);

    // 获取相机图像代码
    cv::VideoCapture cap(0);    // change to 1 if you want to use USB camera.

    // 分辨率设为640x480
    cap.set(CV_CAP_PROP_FRAME_WIDTH, 640);
    cap.set(CV_CAP_PROP_FRAME_HEIGHT, 480);

    // 记录系统时间
    auto start = chrono::system_clock::now();

    while (1) {
        cv::Mat frame;
        cap >> frame;   // 读取相机数据
        auto now = chrono::system_clock::now();
        auto timestamp = chrono::duration_cast<chrono::milliseconds>(now - start);
        SLAM.TrackMonocular(frame, double(timestamp.count())/1000.0);
    }

    return 0;
}
```

在ORB-SLAM2项目的CMakeLists.txt中添加
``` cmake
add_executable(slam_camera myslam.cpp)
target_link_libraries(slam_camera ${PROJECT_NAME})
```
运行
```
./slam_camera
```

