# flightgear整理
## 编译
1、[flightgear](http://www.flightgear.org/)项目托管在SourceForge而不是github，原因是（Sourceforge has been chosen for a host of the FGDATA repo, among other considerations, due to its large size (currently at 1.3G)）,应该是github空间太小了？项目仓库地址：http://sourceforge.net/projects/flightgear/  ，各个仓库简介图：
![fl](http://ovybirdvz.bkt.clouddn.com/fl.png)
2、osgearth版本的flightgear遇到的问题：
a）教程地址如下：http://wiki.flightgear.org/index.php?title=Building_FlightGear_with_osgEarth_Integration&mobileaction=toggle_view_desktop
b) osgearth simgear-osgearth 和 flightgear-osgearth 版本要一致，需要从git上down下代码后切换分支版本
c）编译flightgear-osgearth的时候遇到关于gdal库加载的DSO missing from command line 问题，此问题通过手动更改src/Main/CMakeFiles/fgfs.dir/link.txt中gdal链接问题解决，具体原因还需要进一步弄懂
d）flightgear中飞机模型是ac格式，说明是使用[AC3D](http://www.inivis.com/)软件进行制作，可以使用osgviewer进行三维模型的查看；
## 使用
1、可以参考fgdata/Dcos/getstart-zh.pdf中的详细介绍；
2、ICAO机场代码，国际民航组织机场代码；International Civil Aviation Organization Airport Code，ICAO code，由四个英文字母组成；这个在--airport启动参数中可以指定