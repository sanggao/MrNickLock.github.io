---
title: git不完全笔记
date: 2017-06-11 22:06:55
tags:
---

记录平时学习和使用的git知识。

## hexo搭博客时用到的git命令

- **ssh免密登录github**

```bash
ssh-keygen -t rsa -C "hanziqiabc@gmail.com"
```
key生成于~/.ssh/id_rsa.pub

- **关联远程仓库**:

```bash
git remote add origin git@github.com:MrNickLock/MrNickLock.github.io.git
```

- **把本地库的所有内容推送到远程库上**：

```bash
git push -u origin master #第一次
git push origin master
```
- 创建分支，切换分支

```bash
git checkout -b blog-source 

#git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
git branch blog-source
git checkout blog-source

```

- **git忽略规则**
[文](http://www.pfeng.org/archives/840)

> 在git中如果想忽略掉某个文件，不让这个文件提交到版本库中，可以使用修改根目录中 .gitignore 文件的方法（如无，则需自己手工建立此文件）。这个文件每一行保存了一个匹配的规则例如：

```bash
# 此为注释 – 将被 Git 忽略
 
*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt 
```


- *规则未生效*

> 未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

- **cmder 之 绿色版git for win 中文路径问题**——[文](http://939999807.iteye.com/blog/2227859)


`git status` 显示中文路径乱码。
> 在git项目目录中执行`git config core.quotepath false`就可以解决了

> 也可以执行`git config --global core.quotepath false`进行全局设置
