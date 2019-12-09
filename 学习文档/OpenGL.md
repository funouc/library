# OpenGL、mesa、GLES and Nouveau关系
OpenGL是  
图形界面（Xwindow）并不是Linux的一部分，Linux知识一个基于命令行的操作系统，Linux其实是一系列程序组成的（https://blog.csdn.net/u012822903/article/details/54572807） 。登录界面是登录管理器程序，桌面系统则是桌面管理器程序，桌面管理器（Desktop Enviroment:KDE GNOME Unity）和登录管理器关系（lightdm light display manager gdm gdm3），当然一般桌面管理器有相对应的登录管理器，KDE对应KDM，GNOME对应gdm；当然桌面管理器其实还包含更小的窗口管理器，例如gnome默认的窗口管理器是metacity，kde默认的是kwin等；  
具体什么是X协议，Xorg是X的具体开源实现，Linux下图形界面关系。
Mesa为基于DRI2/DRM架构的开源GPU驱动程序提供客户端OpenGL接口。或换句话说：它也是司机的一部分。

如果您已经安装了NVidia或AMD的专有驱动程序，则不需要Mesa。如果你想使用开源驱动（nouveau，radeon，radeonhd，intel），你需要需要 Mesa。Mesa已经是任何一个Linux版本首选的OpenGL实现

https://www.nomox.cn/post/amdgpu-drives-on-ubuntu1804/