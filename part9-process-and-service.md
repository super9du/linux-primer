## 九）进程与服务

### 名词解释

**程序（Program）**：可运行的二进制文件。

**进程（Process）**：正在运行的程序。

**服务**：也叫**守护进程（daemon）**，常驻在内存的进程。大多数守护进程名以字母 `d` 结尾，比如 `crond`、`sshd` 等。

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

### 任务管理

#### 后台任务

在当前会话界面执行如下命令（睡眠 1 分钟），表示当前进程在前台运行，此时 Linux 不接受除中断信号以外的其他命令。

```bash
sleep 1m
```

此时我们立刻按下 **`Ctrl`+`z`，会将该任务放到后台并暂停任务运行**。

操作后显示如下。其中第二行的数字 `1` 表示任务号。`+` 表示此任务为最近的一个后台任务，如果这个位置是 `-` 则表示这是最近的第二个后台任务，如果什么都没有则表示这是最近的两个以外的任务。`Stopped` 表示任务的状态是暂停，`Running` 表示运行。最后的 `sleep 1m` 表示任务名。

```
^Z
[1]+  Stopped                 sleep 1m
#[任务号][+|-] 任务状态		任务全名
```

我们再使用如下命令：

```bash
sleep 2m &
# 显示如下
[2] 405
#[任务号] PID
```

我们发现当前会话的前台允许我们继续执行其他任务。显示的两个数字分别为任务号和PID（进程 ID）。

**使用 `&` 符号能让进程在后台运行，而不会被 `Ctrl`+`c` 终结。**

#### `jobs` 命令

接上文，如下所示，此时运行 `jobs`，可以列出所有的后台任务。

```bash
jobs
[1]+  Stopped                 sleep 1m
[2]-  Running                 sleep 2m & 
```

**语法**：`jobs [-lrs]`

**选项**

- `-l` 额外显示 PID
- `-r` 仅显示运行的任务
- `-s` 仅显示暂停的任务

#### `fg` 命令

- `fg` 把最近一次后台运行的任务放到前台去执行（即带 `+` 的任务）。
- `fg -` 把最近的第二个后台运行的任务放到前台去执行（即带 `-` 的任务）。
- `fg <%job_number>` 把指定的任务放到前台去执行，其中 `%` 是可选的。

```bash
fg %1 # 取任务号为 1 的任务去前台执行
```

#### `bg` 命令

`bg <%job_number>` 把指定的任务（需要这个任务没有在运行）放到后台执行。

#### `nohup` 命令

`nohup` 表示在会话关闭时，不对该任务进行挂起。否则该任务可能因为会话中断发出的 SIGHUP 信号导致任务被终结。

```bash
nohup sleep 3m
```

但只使用 `nohup`，任务能够被 `Ctrl`+`c` 中断。另外 nohup 会默认将任务的输出写入到命令执行所在位置的 `nohup.out` 文件中。一般为了避免以上两种问题，通常可以使用如下方式执行 `nohup` 命令：

```bash
nohup sleep 3m &> /dev/null &
```

#### `&` 与 `nohup` 区别

> 这里的 `&` 单指与前方命令至少有一个空格，且放在命令最后的，用于后台运行命令。

- `&` 能让任务进入后台，但不能被 `Ctrl`+`c` 中断。
- `nohup` 不会进入后台，但能免疫会话关闭发出的 `SIGHUP` 信号。

通常会连用 `nohup` 和 `&` 同时免疫 `Ctrl`+`c` 和 `SIGHUP` 信号。

#### 请回答以下命令的异同

```bash
command
command > /dev/null
command > /dev/null 2>&1
command &
command > /dev/null &
command > /dev/null 2>&1 &
command &> /dev/null
nohup command &> /dev/null
nohup command &> /dev/null &
```

**解答**

```bash
command 					 # 前台执行命令
command > /dev/null 		 # 前台执行命令，丢弃标准输出
command > /dev/null 2>&1	 # 前台执行命令，丢弃标准输出，错误输出重定向到标准输出
command &					 # 后台执行命令，但仍显示标准输出和错误输出
command > /dev/null &		 # 后台执行命令，丢弃标准输出
command > /dev/null 2>&1 &	 # 后台执行命令，丢弃标准输出，错误输出重定向到标准输出
command &> /dev/null		 # 前台执行命令，丢弃标准输出和错误输出。等同于 command > /dev/null 2>&1
nohup command &> /dev/null	 # 前台执行命令，丢弃标准输出和错误输出，但不挂起（hugup）任务
nohup command &> /dev/null & # 后台执行命令，丢弃标准输出和错误输出，但不挂起（hugup）任务
```

#### `kill` - 终止进程

**语法**

```bash
kill [-s sigspec | -n signum | -sigspec]  < pid | jobspec > [...] 
```

- `signum` 表示信号数值
- `signame` 表示信号名
- `sigspec` 表示 `signum` 或 `signame`

**kill 常用信号**

`kill -l` 列出 `kill` 命令可以发出的所有信号。

* 15) SIGTERM 默认信号。`kill -15 <PID>` 等同于 `kill <PID>`。
*  9) SIGKILL 该信号会强制杀死一个任务。
*  1) SIGHUP 会话关闭会发出此信号。

上述内容中，如 `15) SIGTERM`，数字代表 `signum`，字母代表 `signame`

**示例**

```bash
kill 1234 # 使用 SIGTERM 终结 pid 为 1234 的进程
kill -9 1234 # 使用 SIGKILL 终结 pid 为 1234 的进程
kill -SIGTERM %1 # 使用 SIGTERM 终结任务号为 1 的任务
```

**衍生命令（请自行了解）**

- pkill

- xkill  图形界面终止进程命令

### `ps` - 查看进程（Process Status）

ps 由于历史原因，存在多种命令格式。比如 `ps -ef` 这样的短命令格式，比如 `ps aux` 这样的格式等。

**语法**：`ps [options]`

**选项**

-  `-A,` `-e`         all processes
-  `-a`                   all with tty, except session leaders
-   `a`                    all with tty, including other users
-  `-d`                   all except session leaders
-  `-N`, `--deselect`       negate selection
-   `r`                   only running processes
-   `T`                   all processes on this terminal
-   `x`                   processes without controlling ttys

**ps 输出的表头及其含义**

- UID 使用用户

- PID 进程 id

- PPID 进程的父进程

- C	占用资源的比例

- TTY 进程使用的终端。？表示不占用终端

- STIME	表示进程开始运行的时间

- TIME 进程运行持续的时间

- CMD 进程对应的程序名


ps -ef 

### `top` 命令

语法：`top [选项]`

选项

```
-b：以批处理模式操作；
-c：显示完整的治命令；
-d：屏幕刷新间隔时间；
-I：忽略失效过程；
-s：保密模式；
-S：累积模式；
-i<时间>：设置间隔时间；
-u<用户名>：指定用户名；
-p<进程号>：指定进程；
-n<次数>：循环显示的次数。
```

`top` 交互命令

```
h：显示帮助画面，给出一些简短的命令总结说明；
k：终止一个进程；
i：忽略闲置和僵死进程，这是一个开关式命令；
q：退出程序；
r：重新安排一个进程的优先级别；
S：切换到累计模式；
s：改变两次刷新之间的延迟时间（单位为s），如果有小数，就换算成ms。输入0值则系统将不断刷新，默认值是5s；
f或者F：从当前显示中添加或者删除项目；
o或者O：改变显示项目的顺序；
l：切换显示平均负载和启动时间信息；
m：切换显示内存信息；
t：切换显示进程和CPU状态信息；
c：切换显示命令名称和完整命令行；
M：根据驻留内存大小进行排序；
P：根据CPU使用百分比大小进行排序；
T：根据时间/累计时间进行排序；
w：将当前设置写入~/.toprc文件中。
```





### 查看操纵服务

守护进程 daemons

需要根用户权限

### 参考

* 《鸟哥的Linux私房菜》第16章
* [中国大学慕课-Linux系统管理](https://www.icourse163.org/course/NBCC-437004?tid=1002729007) 视频教程