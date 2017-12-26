# Git学习文档
- Git初始化，即Git config -global user.name="" user.email="" 设置，使用git config --list 查看你的设置的信息；
- 如何删除github上的分支，git push origin : branch，我的理解为：因为远程仓库推送的命令是以下，所以用简写方式推送到remotebranch，默认是将当前分支localbranch推送，所以当localbranch为空时候，则remotebranch就被推送为空，即删除了；
```bash
git push [远程库名] [本地分支名] : [远程分支名]
git push origin localbranch:remotebranch
git push github localbranch//简写
```