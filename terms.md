## 名词参考

**大小写敏感**: 即区分大小写。Linux 中的命令和脚本是区分大小写的，文件的名称也是区分大小写的。

**shell**: 原意壳，包裹在操作系统内核之外，是一种用于解释执行 `shell` 脚本的工具。Linux 中的 shell 有很多种，比如 sh（Bourne shell）、bash（Bourne Again shell）、dash（Debian Almquist shell）、zsh 等。shell 命令对大小写敏感。

**bash**: Linux 默认的 shell，在 Linux 上 sh 一般是 bash 的软连接。但 ubuntu 等 debain 系列的 Linux 上，sh 默认指向 dash。

**路径**: 由多级目录组成且中间使用斜杠连接的一串字符串。Windows 中开头必须是`盘符:\`，结尾必须是最后一级目录或文件名（Windows中除可执行文件，均需加扩展名）。如 Windows 中 cmd 的路径为： `C:\windows\system32\cmd.exe`。

**绝对路径**: 如 `/tmp/level1`, `/home`  这种从根目录一直列的路径称之为绝对路径（`home` 是根目录下的文件夹）。

**相对路径**: 如 `level/level2`  `level1`  `test` 这种从当前目录列的路径称之为绝对路径。

**归档**: 同打包，与压缩不同。归档代表着将文件不经压缩放在一个包中，类似于复制。

**解档**: 同解包，与解压不同。代表将归档的文件按照需要还原以供使用。

**打包**: 即归档，与压缩不同。Linux中打包和压缩是两种不同的存在。归档代表着将文件不经压缩放在一个包中，类似于复制。

**解包**: 即解档，与解压不同。代表将归档的文件按照需要还原以供使用。

**输出重定向**：指将某个程序默认指向 stdout 或者 stderr 的输出文本流转而指向另一个文件，也即程序输出到某个指定文件中而不是输出到终端屏幕或者终端窗口中了。 

**输入重定向**：指让某个程序从指定文件中获取输入而非从 stdin 中 （常常指键盘）获取输入。

**管道**：特殊的输入输出重定向。将一个命令的标准输出重定向为另一个命令的标准输入。

**程序（Program）**：可运行的二进制文件。

**进程（Process）**：正在运行的程序。

**控制终端（Controll Terminal）**：Linux 有 7 个终端界面（tty，六个命令行界面，一个图形界面），分别可以使用 `Alt`+`F1`/.../`F7` 切换，其中一个界面卡死，不影响打开其他几个。

**会话（Session）**：我们每新开一个命令行对话框，其实打开的不是一个真正的终端，而是一个会话。每个会话会关联一个终端。使用 `ps` 可以查看当前会话所属的终端。如下代码块所示，`TTY` 就代表所属的终端。

**任务（job）**：每个会话都会有一个前台任务，多个后台任务。如果会话所属的终端不是 `tty`，那么在会话关闭的时候，所有的 job 都会接收到 `SIGHUP` 信号，导致进程终止。

```sh
> ps
PID TTY          TIME CMD
241 pts/2    00:00:00 bash
349 pts/2    00:00:00 ps
```

**前台（Foreground）**：会话任务的命令行界面。前台任务执行时不能做其他操作，需要等待命令执行完毕，但可以通过信号对任务做一些操作，比如：可以通过 `Ctrl`+`c` 中断该任务，通过 `Ctrl`+`z` 暂停该任务。

**后台（Background）**：后台任务允许该会话执行其他命令，可以通过在命令最后加上空格与`&` 符号，使任务进入后台。

**守护进程（daemon）**：常驻在内存的进程。大多数守护进程名以字母 `d` 结尾，比如 `crond`、`sshd` 等。

**服务（service）**：守护进程提供的能力