# pkg-config
pkg-config 是Linux下头文件和库文件配置程序，在编译和链接的时候需要对于很多第三方库的头文件和库文件。使用方法如下：  
在/usr/lib/pkgconfig 、/usr/lib/x86_64-linux-gnu/pkgconfig等默认路径下面有*.pc文件，pc文件里配置了如下信息：
- prefix一般是指定库的默认安装路径
- exec_prefix一般是指库的另外指定的安装路径
- inludedir指定库的头文件路径
- libdir指定库的lib文件的路径
- Name指定库的名称，比如笔者使用的SQLite数据库 
- Description表示库的描述
- Version是版本号
- Cflags是gcc链接头文件的指令,以-I紧接头文件路径设置
- Libs是gcc链接lib文件的指令, 是-L紧接lib文件路径，-l紧接所使用的lib的名字。  
查看系统中有哪些可以使用pkgconfig配置的库：
```
pkg-config --list-all
```
当需要使用第三方的pkg-config配置文件时候，pkg-config程序查找这些pc文件时候需要配置PKG_CONFIG_PATH环境变量 export PKG_CONFIG_PATH=/home/opencv/pkgconfig:$PKG_CONFIG_PATH
1、单独用gcc编译源文件时候
~~~bash
gcc sample.c -o sample `pkg-config --cflags --libs glib-2.0`// 可以分为以下编译和链接两部分
gcc -c `pkg-config --cflags glib-2.0` sample.c 
gcc sample.o -o sample `pkg-config --libs glib-2.0`
~~~
上述--cflags 是gcc编译时指定编译时候需要的头文件，glib-2.0则是pkgconfig文件下glib-2.0.pc文件的文件名。--libs 则是gcc链接时候的指令；  其实在gcc中并没有cflags和libs指令，gcc中编译文件指定头文件和路径的方法如下：
```
g++ -o my.exe my.cpp -I/home/aaa/include -L/home/aaa/lib -ltest
```
2、在cmake中配置使用pkg：
~~~
CFLAGS += `pkg-config --cflags sqlite3`
LDFLAGS += `pkg-config --libs sqlite3`
~~~
3、在Qt的pro中使用pkg方法如下，就是qmake对pkgconfig的解析，上述是cmake对pkgconfig的解析：
~~~bash
unix {
    CONFIG += link_pkgconfig C++11
    PKGCONFIG += <pc_file_without_extension>
}
~~~
~~第一行则de声明使用pkgconfig程序进行头文件和库文件的寻找链接~~，其实它就是qmake会自动执行pkg-config这个工具，找到对应的库文件目录，第二行则是指定使用哪些库文件的pc文件，即使用哪些第三方库。  **只能自动链接库，并无法自动链接头文件**  
4、makefile中使用pkgconfig
~~~
CFLAGS = `pkg-config --cflags gtk+-2.0` 
LDFLAGS = `pkg-config --libs-only-L gtk+-2.0` 
LIBS = `pkg-config --libs-only-l gtk+-2.0`
make CFLAGS='-g -O2 -w' CXXFLAGS='-g -O2 -w' //在使用make命令使用也可以使用CFLAGS指令指定gcc编译参数
~~~
其中的 ` 符号，不是单引号。而是和~符号为同一按键的那个符号!!!# ``
# ld.so.conf
上述只是编译链接时候提高生产效率的方法，当程序编译完成需要运行的时候还是需要动态查找动态链接库，除了系统默认查找的路径外，当第三方库不在系统默认路径下的时候，我们需要讲第三方库路径目录添加到/etc/ld.so.conf.d/目录下；同时需要通过ldconfig实时刷新ld.so.cache。   