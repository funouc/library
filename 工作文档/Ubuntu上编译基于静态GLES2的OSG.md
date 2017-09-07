# OSG和Earth在Linux上静态库的编译问题集锦（GLES2版本）

## 需要编译成静态库的第三方库

总结来看，都是configure加上配置参数，主要是--disable-shared和--enable--static

- ~~ZLIB~~
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
- ~~giflib~~
- ~~jpeg-9a~~
- ~~libgeos~~
- ~~libpng~~
- ~~libxml~~
- ~~proj~~
- ~~tiff~~
- gdal  
在编译提供的1.11版本的时候遇到了错误，错误提示如下![image](file:///F:/img/gdalerror.jpg)  
最终通过使用2.2.1版本解决问题
- sqlite  

## 遇到的问题及解决思路方法

- GLUint64类型错误  
首先，其中这个[网站](http://forum.openscenegraph.org/viewtopic.php?t=15102)上的解决方案就是现在OSG3.4的版本;
所以暂时确认问题出在src/osg/GL.in文件中，通过cmake配置后GL.in会生成解决方案中include/osg/GL头文件；现在暂时的处理方法是先将GL.in中的#cmakedefine GL_HEADER_HAS_GLUINT64 这两句注释，暂时编译过去，具体问题后续再说。
- 配置NacL版本的GLES2库  
此处采用网上下载的pepper49中的OpenGLES2库，具体配置如下图示意：
![image](file:///F:/img/cmake1.jpg)![image](file:///F:/img/cmake2.jpg)
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
 - libosgViewerd.a的x11错误，应该是GLES2库的问题，具体问题报错如下：![image](file:///F:/img/x11.jpg)
 首先，这个版本的OSG并不需要视窗系统支持，所以OSG_WINDOWING_SYSTEM设置为None，此时编译application里面的程序只会编译osgVersiond和osgPresent3D，但是osgPresent3D会报错，如下图所示：![image](file:///F:/img/present.jpg)根据提示，则将present3D.cpp中120行注释，重新编译通过。