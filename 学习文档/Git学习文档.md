# Git学习文档
- git三种文件状态对应的三个工作区域的理解：已修改（modified）和已暂存（staged）、已提交（committed）分别对应Git当前工作目录、暂存区域和本地仓库；
- Git初始化，即Git config -global user.name="" user.email="" 设置，使用git config --list 查看你的设置的信息；
- git 中如何查看某个命令如何使用：
```
git help <verb>
git <verb> --help
git help config //查看config命令使用方法
git help clone //查看clone命令使用方法
```
- 如何删除github上的分支，git push origin : branch，我的理解为：因为远程仓库推送的命令是以下，所以用简写方式推送到remotebranch，默认是将当前分支localbranch推送，所以当localbranch为空时候，则remotebranch就被推送为空，即删除了；
```bash
git push [远程库名] [本地分支名] : [远程分支名]
git push origin localbranch:remotebranch
git push github localbranch//简写
```