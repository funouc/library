# OSG和Earth在Linux上静态库的编译问题集锦（GLES2版本）
## 需要编译成静态库的第三方库
- ZLIB
- curl（cURL）  
curl和libcurl的区别，就是curl包含了可执行程序curl，所以编译curl可以得到libcurl；  
静态库的libcurl下面命令获取帮助，主要是配置编译静态，禁止编译动态，以及一些without支持的协议和库等。
```
./configure --help
```
  
```
sh ./configure --prefix=/home/apple/Fun/3rdparty/ --disable-shared --enable-static  
--without-libidn --without-ssl --without-librtmp --without-gnutls   
--without-nss --without-libssh2 --without-zlib --without-winidn --disable-rtsp --disable-ldap   
--disable-ldaps --disable-ipv6
```
- freeglut
- freetype
- giflib
- jpeg-9a
- libgeos
- libpng
- libxml
- proj
- tiff
- sqlite  

## 遇到的问题及解决思路方法
- GLUint64类型错误  
首先，其中这个[网站](http://forum.openscenegraph.org/viewtopic.php?t=15102)上的解决方案就是现在OSG3.4的版本；所以暂时确认问题出在src/osg/GL.in文件中，通过cmake配置后GL.in会生成解决方案中include/osg/GL头文件；现在暂时的处理方法是先将GL.in中的#cmakedefine GL_HEADER_HAS_GLUINT64 这两句注释，暂时编译过去，具体问题后续再说。紧接着