# 介绍

<table>
    <tr>
        <td><strong>注意</strong></td>
        <td>本书内容可能存在错误、疏漏或过时，请大家注意甄别。</td>
    </tr>
</table>

## 关于本书

本书主要整理自

* [中国大学慕课-Linux系统管理](https://www.icourse163.org/course/NBCC-437004?tid=1002729007) 视频教程
* [实验楼-Linux基础入门](https://www.shiyanlou.com/courses/1)
* **网络**

本书的目标是

* 让初学者对 Linux 命令有一个大致掌握
* 让已掌握 Linux 的人梳理 Linux 命令的知识体系和脉络
* 为大家提供一个常用 Linux 命令的速查平台

## 关于学习

Linux 命令速查网站

* [Linux中国—— tl;dr](https://tldr.linux.cn/cmd/chown)，一款「Linux 命令速查工具」，内容由志愿者翻译自权威网站。另外，Linux中国有一款名为 Linux 的小程序，可以查到更多命令
* [Linux命令大全](https://man.linuxde.net/)，命令速查工具，错误挺多的，格式也不统一，应该是站长自己的笔记

Linux 书籍推荐

* 《[Linux命令行与shell脚本编程大全](https://www.douban.com/doubanapp/dispatch/book/26854226)》入门 Linux 和 shell 脚本编程（已读，推荐）
* 《[鸟哥的Linux私房菜](https://www.douban.com/doubanapp/dispatch/book/30359954)》（没读过，名气比较大）



<table>
    <tr>
        <td><font color="red"><strong>注意</strong></font></td>
        <td><font color="red">以下内容非常重要！</font></td>
    </tr>
</table>

## 关于一些格式

在本书命令介绍中

* 被 `<>` 包围表示必选项，如 `<操作>`

* 被 `[]` 包围表示可选项，如 `[选项]`

* 本文中如无意外`<操作>`均表示必选项，`[选项]`表示可选项

* `|` 表示可选该符号左边或右边的一种方案，如 `[文件|目录]`。鉴于 gitbook 算法的 bug，本文有时也会使用 `/` 替代 `|` 。

* `-` 后跟一个字符，称为短格式。比如 `rm -f filename`

* `--` 后跟一个单词或词组（中间用 `-` 连接），称为长格式。比如 `rm --force filename`

* 短格式的选项如果不冲突一般可以连用。如列出全部文件的长格式 `ls -a -l`，可以写为 `ls -al`

* 命令介绍中的 `=`表示该命令后紧跟 `=` 后的内容。比如 `tar` 命令的一个帮助页面有这么一段

  ```
  -f, --file=ARCHIVE         use archive file or device ARCHIVE
      --force-local          archive file is local even if it has 
  ```

  `-f, --file=ARCHIVE` 表示在实际输入时写为 `tar -f <archive>` 或 `tar --file <archive>` 即可。例如 `tar -xzvf demo.tgz` 

## 关于本书约定

**文件**: 一般我们口语中将文件和文件夹统称为「文件」，本书有严格区分。本书中的「文件」独指文件夹中除文件夹以外的文件(名)。

**文件夹**: 同目录。区别于文件，用于存放文件。

**目录**: 同文件夹。在本书中「文件夹」和「目录」可能会交叉出现。

## 友情链接

- [知名大学开源课程收录计划（北大、清华等985、211大学课程收录计划）](https://github.com/super9du/ggs-ddu)
- [SpringCloud 不入门到入门](https://github.com/super9du/mycloud2020)
- [这才是真正的书评！](https://book.douban.com/review/12437882/)

