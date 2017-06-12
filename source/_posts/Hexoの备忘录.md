---
title: Hexoの备忘录
date: 2017-06-10 15:50:47
tags: 
	- 工具

thumbnail: 
	/gallery/pics/jrz.jpg
---

~个人hexo博客的使用经验记录~

### 快速新建博文（搭配cmder）

> 笔者win10，自定义批处理命令置于mycmd下

- mycmd下新建blog.bat
- 写入命令如下：

```bash
cd d:\GoogleDrive\NoteBook\blog\source\_posts #切入博文目录
explorer .  #在资源管理器中打开当前目录
hexo new %1 #新建指定名称的博文
```
> 键入`blog 博文标题`命令，`hexo new %1` 接收传递的第一个参数 `%1` 作为新博文名称。
 
*注*：原计划新建博文，并用sublime打开编辑，无奈hexo new命令之后就中断了，退而求其次，才有了如上方法。日后改进。

### 博客源文件备份（搭配GoogleDrive）

- 科学上网
- GoogleDrive关联本地，新建blog文件夹
- 在blog文件夹中初始化Hexo

**优点**：实时同步，还有回收站，安全有保障；

**缺点**：source文件夹和public文件夹重复的文件都会占用云盘空间

### 自动检测文件变化并编译（安装 Hexo Server）
> 参考hexo官方文档