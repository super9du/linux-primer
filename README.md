<table>
    <tr><td><strong>广告：你可能需要一台自己的远程服务器（每月 8 块多）</strong></td></tr>
</table>

<a href="https://curl.qcloud.com/iTI9549b">![image-20201213232918801](README.assets/image-20201213232918801.png)</a>

# 《Linux 不入门到入门》介绍

<table>
    <tr>
        <td><strong>注意</strong></td>
        <td>本书内容可能存在错误、疏漏或过时，请大家注意甄别</td>
    </tr>
</table>

## 关于本书

> 本书的精华几乎全都在本页了，所以一定要好好看！

本书主要**整理自**

* [中国大学慕课-Linux系统管理](https://www.icourse163.org/course/NBCC-437004?tid=1002729007) 视频教程
* [实验楼-Linux基础入门](https://www.shiyanlou.com/courses/1)
* 网络等

本书的**目标**是

* 让初学者对 Linux 命令有一个大致掌握
* 让已掌握 Linux 的人梳理 Linux 命令的知识体系和脉络
* 为大家提供一个常用 Linux 命令的速查平台

如果你需要对本书的内容进行**勘误**

* 第一种方式：在本书所在的 [Git 仓库](https://github.com/super-9du/linux-primer)上提交 issue，阐明出错内容的位置和更正建议
* 第二种方式：Fork 本书所在的 [Git 仓库](https://github.com/super-9du/linux-primer)，修改你认为有问题的地方，提交 Pull Request

## 关于继续学习

Linux 书籍推荐

* 《[Linux命令行与shell脚本编程大全](https://www.douban.com/doubanapp/dispatch/book/26854226)》入门 Linux 和 shell 脚本编程（推荐）
* 《[鸟哥的Linux私房菜](https://www.douban.com/doubanapp/dispatch/book/30359954)》

Linux 网站推荐

* [Linux中国—— tl;dr](https://tldr.linux.cn/cmd/chown)，一款「Linux 命令速查工具」，内容由志愿者翻译自权威网站。另外，「Linux中国」有一款名为「Linux」的小程序，可以查到更多命令
* [Linux命令大全](https://man.linuxde.net/)，命令速查工具，有一些错误和疏漏，内容格式略有不一，需要注意甄别
* [鸟哥的 Linux 私房菜](http://linux.vbird.org/) 鸟哥自己做的网站，上述《鸟哥的Linux私房菜》最早其实就成型于此站，该书也可以在此站上免费阅读

## 关于学习的建议

Linux 命令繁多而复杂，将所有 Linux 命令全部都靠死记硬背地掌握下来是不太现实的。毕竟大家年纪都大了，记忆力远不如小孩子。所以第一点建议：

**不要死记硬背！**

我建议大家学习的时候，先跟着「[中国大学慕课-Linux系统管理](https://www.icourse163.org/course/NBCC-437004?tid=1002729007)」视频敲一遍。第一遍学习，不用太过在意细枝末节。因为学了不用，你也得忘，而且会忘得干干净净。俗话说熟能生巧，命令用的多了，自然就记住了。因此学完以后，一定把最佳实践中留给大家的任务做一遍。所以第二点建议：

**多动手实践！孰能生巧！**

另外，很多人喜欢记笔记，有时这不是一个好习惯（尤其是把笔记记在纸上）。现在是 21 世纪了，如果大家要记笔记，我建议大家记电子笔记。推荐大家选择一款名为 Typora 的 Markdown 编辑器来记笔记。相比纸质来说，可以搜索，能让笔记活起来，而不是让笔记放角落吃灰。对于初学者，我不建议边学边记笔记，因为这样浪费时间，也不利于记忆的连续。所以第三点建议：

**不推荐边学边记！如果非要记，记在电脑里！**

但学完一门课或一本书，记记笔记、画画脑图，把知识复习巩固、总结归纳一下，还是非常有必要的（只是不建议大家在学习过程中记笔记）。

| 不记笔记忘了怎么办？                                         |
| ------------------------------------------------------------ |
| 有几个方法。<br />**第一，本书就是你的笔记。**如果忘了，你可以来 https://super9du.github.io/linux-primer/ 搜索（以后还会发布可以下载的电子书）。<br />**第二，网上查询。**除了本书和搜索引擎，在<关于继续学习>这个部分我列举的网站也可以帮助大家。<br />**第三，买书。**在<关于继续学习>里提到的两本书大家可以买来速读一下，在需要的时候当作手册查阅。<br />**第四，使用帮助命令。**熟练使用帮助命令，即便记不住命令的诸多选项也能让你游刃有余。<br />**第五，多实践。**需要用的时候，想不起来可以使用帮助命令。如果帮助命令看不懂，还可以网上搜索或者查书。 |

## 关于一些格式

在本书命令介绍中

* 被 `<>` 包围表示必选项，如 `<操作>`。

* 被 `[]` 包围表示可选项，如 `[选项]`。

* 本书中如无意外`<操作>` 均表示必选项，`[选项]` 表示可选项。

* `|` 表示可选该符号左边或右边的一种方案，如 `[文件|目录]`。鉴于 gitbook 的渲染算法存在 bug，本书有时也会使用 `/` 替代 `|` 。

* `-` 后跟一个字符，称为短格式。比如 `rm -f filename`。

* `--` 后跟一个单词或词组（中间用 `-` 连接），称为长格式。比如 `rm --force filename`。

* 短格式的选项如果不冲突一般可以连用。如列出全部文件的长格式 `ls -a -l`，可以写为 `ls -al`。

* 命令介绍中的 `=` 表示该命令后紧跟 `=` 后的内容。比如 `tar` 命令的一个帮助页面有这么一段：

  ```
  -f, --file=ARCHIVE         use archive file or device ARCHIVE
      --force-local          archive file is local even if it has 
  ```

  `-f, --file=ARCHIVE` 表示在实际输入时写为 `tar -f <archive>` 或 `tar --file <archive>` 即可。例如 `tar -xzvf demo.tgz` 。
  
* 有时上条所属选项的格式也会写为 `-s N` 这样的格式，比如 `free` 命令的帮助格式：

  ```
  -s N, --seconds N   repeat printing every N seconds
  -c N, --count N     repeat printing N times, then exit
  ```

* bash 命令中上方和后方的 `#` 代表注释的开始，回车代表注释的结束。

## 友情链接

- [知名大学开源课程收录计划（北大、清华等985、211大学课程收录计划）](https://github.com/super9du/ggs-ddu)
- [SpringCloud 不入门到入门](https://github.com/super9du/mycloud2020)
- [这才是真正的书评！](https://book.douban.com/review/12437882/)

![img](README.assets/4281364.webp)