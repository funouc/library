# GCC(GNU)文档  

可以参考学习的文档 https://github.com/guodongxiaren/LinuxTool/blob/master/gcc.md  
https://gcc.gnu.org/onlinedocs/gcc/Invoking-GCC.html#Invoking-GCC  
http://blog.csdn.net/letshi/article/details/70332650  
首先解释一下GCC这个缩写的由来，以及和G++的关系：  
GCC一开始是GNU下C语言编译器的缩写（GNU C Compiler），但随着GNU项目扩大，慢慢的其它语言的编译器也放到了GCC中，所以现在GCC是GNU Compiler Collection的缩写，即GNU编译器集合，现在GCC能够支持的语言包括： C, C++, Objective-C, Objective-C++, Fortran, Ada, Go, and BRIG (HSAIL)。当然每个具体语言编译器有它自己名字，C++编译器是G++，Ada 的编译器是 GNAT 等等。  
编译参数概览：主要是预处理、编译、汇编、优化和链接等方面的参数；  
- 参数 -Wl,option 这个是C++语言添加链接参数用法，例如-Wl,--no-undefined；当编译共享库一般要加上这个，不然的话，对于引用的库不会去检查实现，只检查声明；所以GCC在编译共享库的时候，默认对于链接库并不真正链接；  用过microsoft的vc6或者vs的小伙伴们可能会说，在使用这些IDE时，不单单要设置正确include，而其还要设置其引用的lib路径和lib库名称。而且在链接时就能发现很多问题，从而避免运行时才出错的尴尬情况。
其实在使用gcc/g++连接时，只要添加“-Wl,--no-undefined ”参数，也可以实现上述的功能。如果你使用了include文件，而连接器找不到相应的实现，就会产生错误提示.。  
有个问题，当使用了-Wl,--no-undefined进行共享库编译的时候，即对链接库的定义查找在链接时候就展开，那么此时链接库的顺序问题也会出现；为什么在程序执行时候进行链接库定义展开不会出现问题，具体是GeoVisQt中的，明天在研究