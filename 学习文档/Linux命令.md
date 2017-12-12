# Linux命令日常记录
在线man手册网站：http://man.linuxde.net/
1. rm 删除相关命令：  
如果删除的是文件夹，则需要rm -rf(recursive,force)，如果只有r参数，则会进行每个目录的提醒。  
"rm -f" 强行删除，忽略不存在的文件，不提示确认。(f为force的意思)  
"rm -i" 进行交互式删除，即删除时会提示确认。(i为interactive的意思)  
"rm -r" 将参数中列出的全部目录和子目录进行递归删除。(r为recursive的意思)  
"rm -v" 详细显示删除操作进行的步骤。(v为verbose的意思)
2. 修改用户密码passwd  
修改当前用户密码，则直接输入passwd；修改其它用户例如fun，则输入passwd fun；
3. grep搜索工具的用法
4. vim中搜索用法，/加上搜索字符串，n和N分别是向前向后跳转。
5. sz和rz是用来xsehll和Windows之间收发文件使用
6. 对于shell中输出过长的时候 如果你想一页一页查看；举例，例如cmake --help-commands，此命令直接在shell中输出过长，可以学习man命令输出方式，即cmake --help-commands | more，more或者less 命令，常用less命令；
7. man命令详解，man命令是查看shell中各种命令使用方法,具体如下图：
![man](http://ovybirdvz.bkt.clouddn.com/man.png)  
同时，whatis命令可以查看具体shell命令简短概述
8. RPM和DPKG 两种包管理器的区别：rpm全称RedHat Package Manager，dpkg全程Debian Package,其中Ubuntu是基于Debian开发的。