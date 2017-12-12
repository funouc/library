# vim 使用说明
区别于大多数编辑器，Vim（Vi[Improved]）是有模式的，Vim具有6种基本模式和5种派生模式。https://github.com/aioutecism/amVim-for-VSCode/issues/1， 这是VSCode插件amVim的快捷键使用说明，和vim是基本一致。
## vim中的模式
### 基本模式（Normal Mode）
也称一般模式，此模式是默认模式，即用vim打开就是处于基本模式中，当然打开文件的命令也很多，Vim编辑方式的主要用途是在被编辑的文件中移动光标的位置。一旦光标移到到所要的位置，就可以进行剪切和粘贴正文块，删除正文和插入新的正文。大概如下图几种：
![normal](http://ovybirdvz.bkt.clouddn.com/normal.png)
- vim n filename，从第n行开始打开filename文件，vim filename 默认光标位于行首；
- vim /pattern filename，查找pattern，并将光标位于第一个pattern处，这个好像没用；
- vim filename1 filename2  ...   vim -o filename1 filename2 等多文档编辑在下一节具体讲解.
- 当完成所有的编辑工作后，需要保存编辑器结果，退出编辑程序回到终端，可以发出*ZZ*命令，连续按两次大写的Z键。
- 接下来总结下一般模式 基本模式 编辑模式下的 常见操作分为：跳转操作、复制黏贴操作、删除操作和查找替换操作
## vim中的多文档编辑
标签页(tab)、窗口(window)、缓冲区(buffer)是Vim多文件编辑的三种方式，它们可以单独使用，也可以同时使用。 它们的关系是这样的：