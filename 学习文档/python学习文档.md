# Python学习档
## Python安装方法
因为历史原因，现有Python版本包含了2.x和3.x两个版本，这两个版本竟然不兼容：），我建议还是学习3.x版本的，虽然现有很多Python包是2.x版本，但是历史的潮流终归向前，慢慢的肯定都是3.x的天下，而且2和3的差别也不是很大，学会了3基本也就学会了2；
我用的是Ubuntu系统，所以只介绍Ubuntu下的Python安装，当然安装是简单的，主要是通过安装介绍相应的包路径的配置；

- Python的安装直接通过apt就行，Python通过pip（Package index of Python，我的理解，currently 125131 packages）进行包或者库管理的，早些则是easy_install；
- Python的包存放路径和已安装包查看，路径查看可以通过以下代码进行显示：
```Python
import sys
print(sys.path)
```
总结来说呢，就是/usr//lib/python3.5/dist-packages/,这个路径存放的是自己通过pip install安装的包，这个/usr/local/lib/python3.5/dist-packages/则是系统自带的包路径；当然如果是python2的话，则自己安装的路径/home/fun/.local/lib/python2.7/site-packages，大概了解就行；最后可以通过如下命令查看以安装包的列表，pip list，这个会把所有包列出来，包括系统的和自己后期安装;
 ## Python 语法重点归纳
 - 字符串用单引号''&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;表示字符串，如果字符串中有'，则可以用""表示空格或者\转义；
 - 如果%d表示整数占位符，%f浮点数占位符，%s字符串占位符，%x十六进制占位符；
 - list表示可增删的有序集合；
 ```Python
 classmate = ['tom','lucy']
 classmate.append('fun')
 classmate.pop(1)#删除索引1位置
 classmate.insert(1,'jack')
 ```
 - tuple则是表示不可增删的有序集合；
 ```Python
 classmate = ('tom','lucy')
 ```
 - Python用缩进和冒号来表示代码块；
 ```Python
 age = 20
 if age >= 18:
    pirnt 'adult'
else:
    print 'teenager'
 ```
 - 用def定义函数，例子如下：
 ```Python
 def fun(x):
    if x >= 0:
        return x
    else:
        return -x
 ```

