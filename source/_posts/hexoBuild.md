---
title: github搭建自己的博客步骤
---
先在github上注册账号，新建一个仓库，仓库名为用户名.github.io

1.安装git：sudo apt-get install git
2.git安装完成之后设置基本信息
* 1)设置用户名：git config --global user.name “用户名”

* 2)设置用户名邮箱：git config --global user.email "邮箱"

* 3)查看设置：git config --list

3.添加ssh公钥：ssh-keygen -t rsa -C "email",输入cat ~/.ssh/if_rsa.pub，将公钥添加到github中，可以输入ssh -T git@github.com，测试是否连接成功
4.安装nodejs：curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
5.安装npm：sudo apt-get install npm
6.安装hexo：sudo npm install hexo-cli -g
7.初始化hexo：hexo init
安装一些插件：

* npm install hexo-generator-index --save #索引生成器
* npm install hexo-generator-archive --save #归档生成器
* npm install hexo-generator-category --save #分类生成器
* npm install hexo-generator-tag --save #标签生成器
* npm install hexo-server --save #本地服务
* npm install hexo-deployer-git --save #hexo通过git发布（必装）
* npm install hexo-renderer-marked@0.2.7--save #渲染器
* npm install hexo-renderer-stylus@0.3.0 --save #渲染

8.初始化git：git init
9.git clone 仓库，查看clone 地址：git remote -v，为https:// 方式
移除https的方式，换成 ssh方式
 git remote rm origin，
 git remote add origin git地址

首先找到一个背景图片放到 hexo（hexo工程文件）-> themes -> next -> source -> images 的路径下；
hexo（hexo工程文件）-> themes -> next -> source -> css -> _custom ，找到路径下的custom.styl文件，在文件的最上方加上一代码 body { background:url(/images/backGround.jpg（这是你之前加的背景图片的名字）);} 就完事了。
出现   fatal: 不是一个 git 仓库（或者任何父目录）：.git   错误提示时，需要初始化本地仓库：git init
hexo new "postName" #新建文章

hexo new page "pageName" #新建页面

hexo generate #生成静态页面至public目录

hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）

hexo deploy #将.deploy目录部署到GitHub

hexo help # 查看帮助

hexo version #查看Hexo的版本

hexo deploy -g #生成加部署

hexo server -g #生成加预览

命令的简写hexo n == hexo new

hexo g == hexo generate

hexo s == hexo server

hexo d == hexo deploy

git中ssh的生成：ssh-keygen -t rsa -C "youremail@example.com"


从git中删除文件：git rm 文件名

从github上复制仓库中的项目：git clone 仓库地址

将本地仓库同步到远程仓库：git push

在工作区编写文件、通过git add 文件名添加到暂存区、通过git commit -m “提交信息”添加到本地仓库，通过git push同步到远程仓库

如果是私有仓库或者无法关联远程仓库，无法同步，需要输入用户名和密码的，可以：

vim .git/config

将

[remote "origin"]

url = https://github.com/用户名/仓库名.git

改为

[remote "origin"]

url = https://用户名:密码@github.com/用户名/仓库名.git

个人站点 -> 新建仓库 -> 仓库名必须是用户名.github.io

如果无法推送到远程仓库，原因是远程仓库有过变动，需要先将远程仓库同步到本地：git pull，再将本地文件添加到远程仓库：git push

打开hexo博客分类标签出现Cannot GET/xxxx，说明需要初始化子目录，输入hexo new page “categories”, 就会在source文件里面初始化这些目录，并生成一个index文件

