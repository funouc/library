# github发布博客指南
github上发布博客主要是为了各个项目project进行项目介绍，以此衍生出了博客功能。
## github项目网页
例如所需项目library进行网页发布，则在library项目新建分支gh-pages，然后讲gh-pages分支中的文档都清空，并新建一个md文件，在此文件中进行相关项目介绍；并将gh-pages分支push到github上；然后按照此网址中介绍的在github上新建一个项目http://bluebiu.com/blog/create-blog-in-github.html(好像这一步不需要。。。)；
## 利用github搭建个人网站
按照此网址中介绍的在github上新建一个项目http://bluebiu.com/blog/create-blog-in-github.html；这两个是jekyll的主题网站：https://github.com/jekyll/jekyll/wiki/sites，https://jekyllthemes.io/ ；然后从这上面下载喜欢的jekyll主题；当然运行jekyll的网站需要jekyll组件 ，这是ruby的一个组件，所以需要先安装ruby：
```bash
sudo apt-get install ruby ruby-dev
sudo gem install jekyll/
```
ruby和Python一样，有自己的组件包管理工具，Python的组件包管理工具pip，例如pip install 组件名；

