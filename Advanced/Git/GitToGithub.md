---
title: Git上传项目到Github
date: 2018-09-07 09:25:00
author: Goffy
img: /source/images/xxx.jpg
top: true
cover: true
coverImg: 
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: Git上传项目到Github
categories: Github
tags:
  - Git
  - Github
---
### Git上传项目到Github

#### 0、新建文件夹

![1559036553006](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559036553006.png)

#### 1、在此文件夹右键Git Bash Here，输入命令 

```Java
git init
```

![1559036600072](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559036600072.png)

此步操作为初始化git工作区间

#### 2、将需要上传的项目复制到工作区间

![1559036752091](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559036752091.png)

#### 3、输入命令git status 查看git工作区间的文件变化

```
git status
```

![1559036885875](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559036885875.png)

#### 4、输入命令git add . 将文件加入到暂存区

```
git add .
```

![1559036995464](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559036995464.png)

#### 5、将文件提交到本地仓库

```
git commit -m "the first commit"
```

![1559037091035](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559037091035.png)

#### 6、在Github种新建仓库

![1559037194341](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559037194341.png)

7、复制仓库链接（https链接）

![1559037237509](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559037237509.png)

#### 8、将本地仓库关联到Github仓库

```
git remote add origin https://...
```

![1559037332932](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559037332932.png)

#### 9、将本地仓库同步到GitHub仓库

```
git push origin master
```

![1559037744888](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1559037744888.png)

### 一点一点积累

git拉取远程仓库代码与本地合并：

```
git pull origin master
```

![1562244421283](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1562244421283.png)