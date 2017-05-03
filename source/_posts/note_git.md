---
title: GIT基本使用笔记
date: 2016-12-03 10:19:20
tags:
---

#### 1.初始化一个本地GIT仓储

<!--more-->
```bash
$ cd /project       // 定位到项目文件根目录

$ git init          // 初始化本地仓储
```

#### 2.添加本地GIT忽略清单文件.gitignore

```
//win系统下无法直接创建以'.'开头的文件，需要使用下面方式间接创建。
$ echo '' >> .gitignore
//.gitignore文件中每行表示一个忽略清单，如下：
node_modules
/dist
```

#### 3.查看本地仓储的变更状态

```
$ git status
```

#### 4.添加本地暂存（托管）文件

```
$ git add README.md     // 方式1：添加指定文件名的文件

$ git add *.md          // 方式2：添加通配符匹配的文件

$ git add --all         // 方式3：添加所有未托管的文件（忽略.gitignore清单中的列表）
```

#### 5.提交被托管的文件变化到本地仓储

```
$ git commit -m 'Initial commit(change log)'
```

#### 6.为仓储添加远端（服务器端）地址

```
$ git remote add origin https://github.com/HavenXie/git-demo.git// 添加一个远端地址并起了一个别名叫origin

$ git remote -v         // 查看现有的远端列表
```

#### 7.将本地仓储的提交记录推送到远端的master分支

```
$ git push -u origin master
```

#### 8.拉取远端master分支的更新记录到本地

```
$ git pull origin master
```

#### 9.查看本地提交日志

```
$ git log              
```

#### 10.查看当前版本和本地仓库版本的修改差别。

```
$ git diff
```

#### 11.返回到之前某个版本

```
$ git reset --hard git的本地仓库某个提交版本前六位哈希数值
```

#### 12.创建一个可以预览页面的仓储

```
1. github上新建一个仓储（注意不要勾选readme那个东东）
2. 创建完成复制项目链接
3. $ git init           //在本地已有项目根目录下初始化git
4. $ git status         //查看本地文件变化
5. $ git add .          //添加文件监视
6. $ git commit -m 'init commit'  //本地初始化提交
7. $ git remote add origin 'url'  //添加远端地址
8. $ git git remote -v            //查看远端列表（此步骤可省略）
9. $ git push -u origin master    //本地代码提交到远端master分支

10. $ git branch gh-pages         //创建分支（用于显示页面）
11. $ git checkout gh-pages       //切换到gh-pages分支上
12. $ git push -u origin gh-pages //将代码同步到gh-pages分支上，其实此时并没有代码从本地上传到gh-pages而是直接从master分支上拉取过去。
13. 至此托管在github上的项目站点已经建立好了，项目地址havenxie.github.io 项目名（区分大小写）
```

#### 13.托管在github上面的页面绑定其他域名

```
1. 在项目根目录下创建CNAME的文本文件（不能有扩展名）
2. 在CNAME里面写上你需要绑定的域名，建议绑定二级域名。
3. 本地git提交
4. 把含有CNAME的项目push到gh-pages和master
```