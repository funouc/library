# CMake 学习整理文档

## [基本语法]
包括*内置变量(variable例如CMAKE_BUILD_TYPE)*、*命令*(command，SET)、*属性*（property，例如OUTPUT_NAME）、*模组*（module，例如自带的FindOSG）和*语法*（主要是IF语句使用频繁)四个方面,下面列出一些有用网址 ：  
https://blog.gmem.cc/cmake-study-note  
https://github.com/sailorhero/cmake_study
- 当前项目CMakeList.txt使用了FindOSG，则会在CMake自带的CMakeModules目录和当前项目CMakeModules中查找FindOSG.cmake;但是在CMake的自带CMakeModules和CMakeList.txt所在目录CMaeModules中没有FindOSG.cmake，则会报错，让用户指定包含FindOSG.cmake的目录；
- target_link_libraries(<name> lib1 lib2)，lib1和lib2是存在依赖顺序，即lib1依赖lib2
- cmake --help-variable-list 这个命令很重要 可以查看所有CMake内置变量

## [Linux 上的cmake]
主要分为三种：cmake ccmake 和 cmake-gui；对应的软件包cmake、cmake-curses-gui和cmake-qt-gui；下图是ccmake示意图
![image](/image/ccmake.png)
cmake则是纯终端程序，cmake-gui则是基于qt的可视化工具；