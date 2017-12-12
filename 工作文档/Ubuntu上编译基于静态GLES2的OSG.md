# OSG和Earth在Linux上静态库的编译问题集锦（GLES2版本）

## 需要编译成静态库的第三方库

总结来看，都是configure加上配置参数，主要是--disable-shared和--enable--static

- ~~ZLIB~~
```bash
CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --static --prefix=/home/apple/Fun/3rdparty/
```
- ~~curl（cURL）~~  
curl和libcurl的区别，就是curl包含了可执行程序curl，所以编译curl可以得到libcurl;
静态库的libcurl下面命令获取帮助，主要是配置编译静态，禁止编译动态，以及一些without支持的协议和库等。

```bash
./configure --help
```

```bash
sh ./configure --prefix=/home/apple/Fun/3rdparty/ --disable-shared --enable-static
--without-libidn --without-ssl --without-librtmp --without-gnutls
--without-nss --without-libssh2 --without-zlib --without-winidn --disable-rtsp --disable-ldap
--disable-ldaps --disable-ipv6
```

- freeglut
- ~~freetype~~
```bash
 CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/ --without-bzip2
```
- ~~giflib~~
- ~~jpeg-9a~~
```bash
CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/
```
- ~~libgeos~~
```bash
CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/
```
- ~~libpng~~
```bash
CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/
```
- ~~libxml~~
```bash
CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/
```
- ~~proj~~
- ~~geotiff~~
- tiff
```bash
CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/ --disable-lzma --disable-jbig 
```
- gdal  
在编译提供的1.11版本的时候遇到了错误，错误提示如下![image](file:///F:/img/gdalerror.jpg)  
最终通过使用2.2.1版本解决问题
``` bash
./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/ --without-sqlite3 --with-xml2=/home/apple/3rdparty --without-openjpeg --without-expat --without-python --without-jasper
```
- sqlite  

## OSG遇到的问题及解决思路方法

- GLUint64类型错误  
首先，其中这个[网站](http://forum.openscenegraph.org/viewtopic.php?t=15102)上的解决方案就是现在OSG3.4的版本;
所以暂时确认问题出在src/osg/GL.in文件中，通过cmake配置后GL.in会生成解决方案中include/osg/GL头文件；现在暂时的处理方法是先将GL.in中的#cmakedefine GL_HEADER_HAS_GLUINT64 这两句注释，暂时编译过去，具体问题后续再说。
- 配置NacL版本的GLES2库  
此处采用网上下载的pepper49中的OpenGLES2库，具体配置如下图示意：
![image](http://ovybirdvz.bkt.clouddn.com/cmake1.JPG)  
![image](file:///F:/img/cmake2.jpg)
- 在编译的时候遇到如下jpeg报错，使用的是jpeg-9a版本
![image](file:///F:/img/jpeg.jpg)  
关于这个问题，这个网站上有详细的讨论，并给出了解决方案，主要是大于 typedef int boolean 这句话的错误，所以更改osgPlugins/jpeg/ReaderWriterJPEG.cpp中的
```C++
#if JPEG_LIB_VERSION < 90
    /* Some versions of jmorecfg.h define boolean, some don't...
    Those that do also define HAVE_BOOLEAN, so we can guard using that. */
    #ifndef HAVE_BOOLEAN
    typedef int boolean;
    #define FALSE 0
    #define TRUE 1
    #endif
#endif
```
所以当我们使用的Jpeg版本大于9的话，则不能typedef int boolean定义。
- freetype库问题，具体报错如下：![image](file:///F:/img/freetype.jpg)  
   解决办法：重新编译freetype，添加configure参数 --without-bzip2
- libosgViewerd.a的x11错误，应该是GLES2库的问题，具体问题报错如下：![image](http://ovybirdvz.bkt.clouddn.com/x11.JPG)
 首先，这个版本的OSG并不需要视窗系统支持，所以OSG_WINDOWING_SYSTEM设置为None，此时编译application里面的程序只会编译osgVersiond和osgPresent3D，但是osgPresent3D会报错，如下图所示：![image](file:///F:/img/present.jpg)根据提示，则将present3D.cpp中120行注释，重新编译通过。

## osgEarth遇到的问题
 - 小问题，关于文件名大小写问题，Linux是大小写敏感系统;

 ## test_earth例子编写遇到问题 
 - 首先将静态库的链接顺序调整即 将plugins、osgEarth 、OSG和3rdParty 这个顺序 ，其中要将example中CMakeLists中FREETYPE_LIBRARY_RELEASE改成FREETYPE_LIBRARY，因为根据上级CMakeLists确定。
 - 始终报错gdal的错误，如下图：
 ![gdalerror](http://ovybirdvz.bkt.clouddn.com/gdalerror.png)
 一开始以为是XML的错误，首先将LibXml2加到第三方库链接目录，后来发现可能是expat的错误,最后果然是expat的错误，需要加参数--without-expat；Expat是一个用C语言开发的、用来解析XML文档的开发库，它最初是开源的、Mozilla 项目下的一个XML解析器
 - /home/apple/Fun/prebuild/gdal-2.2.1/frmts/vrt/vrtderivedrasterband.cpp:672：对‘dlsym’未定义的引用，可能需要加入--without-python
 - gdal报jpeg2000错误，可能和openjpeg和jasper这两个库有关，禁用这两个库,Jasper是JPEG2000的一个非官方实现,由一个国外的一个大学教师实现,还算是个好用的LIB
 - 然后报错tiff对jbig和lzma这两个库的错误，所以重新编译禁用这两个库;这两个库的作用分别：LZMA（Lempel-Ziv-Markov chain-Algorithm的缩写）是2001年以来得到发展的一个数据压缩算法，它用于7-Zip归档工具中的7z格式和Unix-like 下的xz 格式。 它使用类似于LZ77的字典编码机制，在一般的情況下壓縮率比bzip2為高，用於壓縮的字典檔案大小可達4GB。 JBIG (Joint Bi-level Image Experts Group)，是由lossless image compression,的基础上特用于传真机的一种压缩技术。数据压缩的演绎法众多，其中使用于图像压缩的演绎法可粗分为两类，第一类就是lossless image compression,是一种压缩再解压缩后不会产生任何误差的演绎法。第二类则为lossy compression是一种压缩再解压缩后产生误差的演绎法。其中JBIG脱身于第一类lossless image compression，而JPEG（Joint Picture Expert Group）则是由第二类lossy compression发展而来的。这里需要说明的是，MH、MR、MMR、JBIG这几种压缩系统都是针对黑白文稿的压缩系统，而JPEG则是为了适应传真彩色文稿的需求而产生的。运用lossy compression来做图像压缩不但可以获得不错的压缩比，也可以兼顾不错的品质。
 - 修改example子文件下的CMakeLists文件，使用add_library
 - 遇到新的问题，如下图![fpic](http://ovybirdvz.bkt.clouddn.com/fpic.png),搜索得知，需要添加位置无关编译参数fpic进行osg和osgearth的重新编译，弄懂FPIC,gdal库同样遇到此错误，用下面编译命令参数重新编译gdal库
 ```bash
 CFLAGS+=-fPIC CXXFLAGS+=-fPIC ./configure --enable-static --disable-shared --prefix=/home/apple/Fun/3rdparty/ --without-sqlite3 --with-xml2=/home/apple/3rdparty --without-openjpeg --without-expat --without-python --without-jasper
 ```
 geos库同样出现此类问题，libz也是这个问题
 - 在CMAKE_SHARED_LINKER_FLAGS 添加了-Wl,--no-undefined参数以后，共享库的也会查找链接库定义，这个需要具体研究
 - 需要添加-lpthread -ldl这两个库，解决pthread和dlopen系列错误
 - 将png库版本从1.6降到1.2，解决png_get_asm这个错误
 - 在自己笔记本上编译跑通cube、gles2和2d  
 ```bash cube
 ./cefclient --enable-webgl --ignore-gpu-blacklist --no-sandbox --disable-web-security --register-pepper-plugins="/home/fun/Fun/test/gles2_spinning_cube/Debug/libgles2_spinning_cubed.so;application/x-ppapi-example-gles2-spinning-cube" --url="file:///home/fun/Fun/test/gles2_spinning_cube/gles2_spinning_cube.html"
 ```
 ```bash gles2
 ./cefclient --enable-webgl --ignore-gpu-blacklist --no-sandbox --disable-web-security --register-pepper-plugins="/home/fun/Fun/test/gles2/Debug/libgles2d.so;application/x-ppapi-example-gles2" --url="file:///home/fun/Fun/test/gles2/gles2.html"
 ```
 ```bash 2d
 ./cefclient --enable-webgl --ignore-gpu-blacklist --no-sandbox --disable-web-security --register-pepper-plugins="/home/fun/Fun/test/cefppapi1/Debug/libgraphics_2d_exampled.so;application/x-ppapi-example-2d" --url="file:///home/fun/Fun/test/cefppapi1/2d.html"
 ```
 - NIUBI_USE_PPAPI在Windows上代码是使用的这个宏定义，此宏定义从何而来  


 ```bash OE
 ./cefclient --enable-webgl --ignore-gpu-blacklist --no-sandbox --disable-web-security --ppapi-out-progress --register-pepper-plugins="/home/fun/Fun/projects/test_earth/lib/libosgMutid.so;application/x-ppapi-stub" --url="http://127.0.0.1/fun/stub.html"
 ```



 addAnnotation、addStreets、addBuildings  
 //USE_OSGEARTH_REGISTER_ANNOTATION(circle)
//USE_OSGEARTH_REGISTER_ANNOTATION(ellipse)
//USE_OSGEARTH_REGISTER_ANNOTATION(feature)
//USE_OSGEARTH_REGISTER_ANNOTATION(imageoverlay)
//USE_OSGEARTH_REGISTER_ANNOTATION(label)
//USE_OSGEARTH_REGISTER_ANNOTATION(local_geometry)
//USE_OSGEARTH_REGISTER_ANNOTATION(model)
//USE_OSGEARTH_REGISTER_ANNOTATION(place)
//USE_OSGEARTH_REGISTER_ANNOTATION(rectangle)  
此处也需要注释