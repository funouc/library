# 分布式编译工具distcc
项目地址：https://github.com/distcc/distcc  ， 项目简介，distcc is a program to distribute compilation of C or C++ code across several machines on a network；  
分为项目开发机和远程服务器，都需安装distcc，Ubuntu直接从源安装：
```bash
sudo apt-get install distcc 
sudo apt-get install distccmon-gnome // distcc任务图形化监控
```
distcc程序分为客户端（开发机）和服务器端（远程编译服务器），客户端程序是distcc，服务器端程序是distccd，客户端使用distcc分发编译任务时候，需要找到服务器（hosts），客户端配置hosts列表方式：
1 、 /etc/distc/hosts 、 $HOME/.distcc/hosts 或者设置环境变量DISTCC_HOSTS，IP地址之间空格；
```
export DISTCC_HOSTS='127.0.0.1/4  192.168.91.63/4 ...'//需要通过127.0.0.1指定本机？
```
2、服务器端配置则是在/etc/default/distcc，具体配置项如下图
![image](/image/distcc.png)
```
STARTDISTCC=""//设置是否开机启动
ALLOWENNETS //允许接收客户端IP
LISTENER //监听的IP
ZEROCONF 
```
其次，也可以通过命令行进行服务器端配置
```
distccd --allow 192.168.56.0/8 --user nobody --daemon
```
工作流程就是服务器端打开distccd监控程序，实时监控是否有客户端分发任务；客户端启动分布式编译实时，需要设置环境变量
```
export PATH=/usr/lib/distcc:$PATH
export DISTCC_HOSTS=...
./configure
make -j$(distcc -j)// 可以直接 make -j
```
当使用automake工具，例如cmake时候
```
export PATH=/usr/lib/distcc:$PATH
export DISTCC_HOSTS=...
cmake-gui /path/to/source //先通过图形化界面配置好
cmake -DCMAKE_C_COMPILER_LAUNCHER=distcc /path/to/source //如果是C++，则用下面配置
cmake -DCMAKE_CXX_COMPILER_LAUNCHER=distcc /path/to/source 
make -j$(distcc -j)// 可以直接 make -j
```
可以通过distccmon-gnome 图形化监控实时任务分发编译情况