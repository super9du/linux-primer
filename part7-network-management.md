## 网络管理

### 查看和操纵网络接口

本段内容提到的网络都是以太网[^以太网]。其中所提到的网络接口、IP 地址都是由网络中的 DHCP 服务器提供的。

#### `ifconfig` 命令

if（interface，接口）。`ifconfig` 可用于查看和操纵（启动、关闭、修改）网络接口。查看所有用户可用，操纵根用户可用。

**「查看用法」**

`ifconfig` 查看所有已开启网络接口的详细信息。

`ifconfig -a` 查看所有网络接口的详细信息。无论是否被启用。

`ifconfig <网络接口名>` 查看指定网络接口的详细信息。

**「开启/关闭用法」**

 `ifconfig <网络接口名> up|down` 开启指定网络接口。

**「修改用法」**

`ifconfig <网络接口名> ip地址 netmask 255.255.255.0` 修改IP地址。

`ifconfig <网络接口名> mtu mut值` 修改MTU值。

同理可以修改更多

> 注：`ifconfig` 改变的部分参数是临时的。 

**「 `ifconfig` 输出示例与解释」**

输出示例（CentOS 7）：

```
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.112  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::428:b7e3:9ebe:f4e7  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:87:5d:e4  txqueuelen 1000  (Ethernet)
        RX packets 157915  bytes 236347475 (225.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 51402  bytes 3628601 (3.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 64  bytes 5568 (5.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 64  bytes 5568 (5.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:00:a8:af  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

第一段落解释（CentOS 6）：

| 示例                           | 解释                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| eth0                           | 网络接口名。eth0 代表第 1 个网络接口。如果有多个网络接口则按照 eth1,eth2依次进行排序。如果只有 eth0 则表示只有一个网络接口 |
| Link encap                     | 表示网络连接的类型。Ethernet 表示以太网                      |
| HWaddr                         | 表示硬件地址                                                 |
| inet addr                      | 网络接口的 IP 地址                                           |
| Bcast                          | 网络广播地址                                                 |
| Mask                           | 子网掩码。一般是255.255.255.0                                |
| inet6 addr                     | 网络接口 IPV6 地址                                           |
| UP BROADCAST RUNNING MULTICAST | 网络接口的运行状态                                           |
| MTU                            | 当前接口 (这里是eth0) 的最大数据传送单元大小                 |
| Metric                         | 接口度量值                                                   |
| RX 和 TX 打头的两行            | 网络接口收发包的情况                                         |
| collisions                     | 数据传输发生冲突的次数                                       |

第二段落解释（CentOS 6）：

| 示例      | 解释                                                         |
| --------- | ------------------------------------------------------------ |
| lo        | 回环接口（loop）。是一个模拟的网络接口，每个系统都有之。为系统提供了一个单机网络环境，一般用于网络程序的调试。 |
| inet addr | lo 的 IP 永远为 127.0.0.1                                    |
| Mask      | 子网掩码永远为 255.0.0.0                                     |

### 配置 TCP / IP 网络参数

通过编辑文件配置

```
cd etc/sysconfig/network-scripts
```

### `ip` 命令

`ip a` 与 `ifconfig` 效果大致相同。列出网络接口详情

`ip r` 列出路由表

### `wget` 命令

`wget` 用于从网络上的指定链接下载指定内容。w 可能是 web （网络）的首字母缩写。

```bash
wget <url> #下载文件到当前目录下
```

### `curl` 命令

> 参考链接：[curl命令详解](https://blog.csdn.net/binglong_world/article/details/80755193)

**curl命令**是一个利用URL规则在命令行下工作的文件传输工具。它支持文件的上传和下载，所以是综合传输工具，但按传统，习惯称curl为下载工具。作为一款强力工具，curl支持包括HTTP、HTTPS、[ftp](http://man.linuxde.net/ftp)等众多协议，还支持POST、cookies、认证、从指定偏移处下载部分文件、用户代理字符串、限速、文件大小、进度条等特征。可被用于做网页处理流程和数据检索自动化。

**语法**

```bash
curl <选项> <参数>
```

**选项**

```
-A/--user-agent <string> 设置用户代理发送给服务器
-b/--cookie <name=string/file>    cookie字符串或文件读取位置
-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中
-C/--continue-at <offset>            断点续转
-D/--dump-header <file>              把header信息写入到该文件中
-e/--referer                                  来源网址
-f/--fail                                          连接失败时不显示http错误
-o/--output                                  把输出写到该文件中
-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名
-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围
-s/--silent                                    静音模式。不输出任何东西
-T/--upload-file <file>                  上传文件
-u/--user <user[:password]>      设置服务器的用户和密码
-w/--write-out [format]                什么输出完成后
-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理
-#/--progress-bar                        进度条显示当前的传送状态
```



[^以太网]: 英文 Ethernet，是一种计算机局域网技术