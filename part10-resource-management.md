## 资源管理

资源管理导航

* [磁盘管理](part6-disk-management.md)
* [网络管理](part7-network-management.md)

### `free` 命令

单独使用 `free`，输出的结果以 `k` 为单位。

**语法**

```bash
free [选项]
```

**选项**

* `-b|-k|-m|-g` 单独使用 `free`，输出的结果以 `k` 为单位的内存信息。使用 `-b`，则以 Byte 为单位，以此类推，k 、m、g 分别代表以 KB、MB、GB 为单位。
* `-h` 表示以人类可读的方式显示内存信息。
* `-s N` 表示每隔 N 秒刷新一次 `free` 的数据
* `-c M` 与 `-s` 搭配使用，刷新 M 次后退出（即：不再继续刷新）

**示例**

```
> free -h
total        used        free      shared  buff/cache   available
Mem:          9.3Gi        61Mi       9.1Gi       0.0Ki        57Mi       9.0Gi
Swap:         3.0Gi          0B       3.0Gi
```

### `netstat` 命令

查看进程的网络连接状况。一些系统里可能没有 `netstat`，所以也可以使用 `ss` 命令，其用法与 `netstat` 相似，这里仅介绍 `netstat`。

**语法**

```bash
netstat [选项]
```

**选项**

* `-a` 全部
* `-n` 将服务名换为端口号显示
* `-p` 列出网络服务进程的 PID
* `-t` 列出 tcp 网络封包的信息
* `-u` 列出 udp 网络封包的信息
* `-l` 列出正在网络监听（listen）的服务

**示例**

```bash
netstat -anp # 或 ss -anp
```

### `vmstat` 命令

**语法**

```bash
vmstat [选项] [delay [count]]
```

**选项**

`-S <unit>` 定义展示的单位，单位可以是 `k`，`m`，`K`，`M`

`delay` 表示刷新频率

`count` 表示刷新次数

**示例**

```
> vmstat -S m 1 1
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
1  0      0   9817     10     50    0    0     1    69    0    3  0  0 100  0  0 
```

```
> vmstat -S k 1 2
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
1  0      0 9817604  10346  50827   0    0     1    69    0    3  0  0 100  0  0
0  0      0 9817604  10346  50827   0    0     0     0    4   23  0  0 100  0  0
```

**表头解释**

* `procs` 进程
  `r` 或 `b` 值越大，代表系统越忙（等待和无法唤醒的进程多）
  * `r` 等待运行中的进程数量
  * `b` 不可被唤醒的进程数量

* `memory` 内存
  * `swpd` 虚拟内存被使用的容量
  * `free` 未被使用的内存容量
  * `buff` 缓冲

* `swap` 内存交换分区
  `si` 或 `so` 值越大，代表数据在内存和磁盘中传输越频繁，系统性能越差
  * `si` 从磁盘中将进程取到内存的容量
  * `so` 由于内存不足，从内存写入磁盘的容量
* `system` 系统
  `in` 或 `cs` 值越大，代表系统与外界设备沟通越频繁（设备如磁盘、网卡等）
  * `in` 每秒被中断的进程次数
  * `cs` 每秒执行的事件切换此时
* `cpu` 
  * `us` 用户态的CPU使用率
  * `sy` 内核的CPU使用率
  * `id` 闲置的CPU占用率

### 其他

`uname -a` 查看系统信息。与 `cat /proc/version` 输出相似。

`uptime` 系统启动时间与任务负载

`top` 查看进程 CPU 占用及空闲 CPU 等信息，详见[进程管理——`top` 命令](part9-process-management.md#`top`-命令)。

`cat /proc/cpuinfo` 查看 CPU 信息，如核数、厂商等信息

### 参考

1. 《鸟哥的Linux私房菜》第16章