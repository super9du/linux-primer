## 十一）服务与安全

### 系统服务

#### 名词解释

- **守护进程（daemon）**：常驻在内存的进程。大多数守护进程名以字母 `d` 结尾，比如 `crond`、`sshd` 等。
- **服务（service）**：守护进程提供的能力

#### CentOS 7

CentOS 7 推荐使用 `systemctl` 管理服务

**语法**

```bash
systemctl <command> <daemon_name>
```

**示例**

以 `ssh` 服务为例

* 启动服务：`systemctl start ssh`
* 查看服务状态：`systemctl status ssh`
* 停止服务：`systemctl stop ssh`
* 重启服务：`systemctl restart ssh`

#### CentOS 6

CentOS 6 没有 `systemctl`，使用 `service` 管理服务

**语法**

```bash
service <daemon_name> <command>
```

**示例**

以 `ssh` 服务为例

* 启动服务：`service ssh start`
* 查看服务状态：`service ssh status`
* 停止服务：`service ssh stop`
* 重启服务：`service ssh restart`

### 安全防护

#### `firewall-cmd` 命令

该命令主要用于管理 Linux 服务器开放哪些端口，防止 Linux 服务器开放了多余的端口，被黑客利用并入侵。

**启动服务**

使用这个命令需要先启动 `firewalld` 服务。

启用、查看、暂停该服务：`systemctl <start|status|stop> firewalld`

**查看开启状态端口**

查看 80 端口是否在防火墙中启用，结果为 no 表示没有启用

```bash
firewall-cmd --query-port=80/tcp
```

**列出当前开启端口**

```bash
firewall-cmd --list-ports
```

**添加端口**

```bash
firewall-cmd --zone=public --add-port=3306/tcp --permanent
```

`--zone` 表示作用域，`public` 表示公开

`--add-port` 表示添加端口，`3306/tcp` 中的 tcp 指的是传输协议

`--permanent` 表示永久生效

> 注：添加端口后必须重启防火墙，否则可能不会生效

**删除端口**

```bash
firewall-cmd --remove-port=6200/tcp
```

##### 重启防火墙

```bash
firewall-cmd --reload
```

**查看当前防火墙的状态**

```bash
firewall-cmd --state
```

#### `iptables` 命令

该命令与 `firewall` 作用相似，都是管理 Linux 端口开放的工具。但该命令不够平易近人，容易让人望而却步。但功能相比 `firewall` 更为强大。如有兴趣请看此文：[文科生也能看懂的iptables教程](https://www.cnblogs.com/walkerwang/p/5087563.html)

使用 `iptables -L` 可以查看 iptables 规则。

#### SELinux

**简介**

SELinux（Security Enhanced Linux），是美国国家安全局（NSA）开发的用于增强 Linux  安全性的。主要目的是防止系统资源被管理员滥用造成的安全问题。

在 SELinux 中进程又被称作「主体」（Subject），文件或目录又被称为「客体」（Object）。

**安全策略（Policy）**

用于指定不同服务是否开放某些资源的读写。

安全策略修改可以通过 `vim /etc/selinux/config` 进行修改，将 `SELINUXTYPE=xxx` 中的 `xxx` 修改为所需策略即可（策略选项以配置文件中的提示为主）。安全策略有如下三种：

- default/targeted/strict：针对网络服务限制多，本机限制少，默认策略
- mls：Multi-Level Security，完整的 SELinux 限制
- src/minimum：自定义策略

**安全上下文（Security Context）**

进程和文件都有自己的安全上下文，进程能否正确地访问文件资源取决于他们的安全上下文是否一致。

【查看安全上下文】

查看文件的安全上下文：

```bash
[root@localhost ~]# ls -Z
#使用选项-Z查看文件和目录的安全上下文
-rw-------．root root system_u:object_r:admin_home_t:s0 anaconda-ks.cfg
-rw-r--r--．root root system_u:object_r:admin_home_t:s0 install.log
-rw-r--r--．root root system_u:object_r:admin_home_t:s0 install.log.syslog
```

查看目录的安全上下文：

```bash
[root@localhost ~]# ls -Zd /var/www/html/
drwxr-xr-x．root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html/
```

查看进程的安全上下文：

```bash
[root@localhost ~]# ps -eZ  # 或 ps auxZ
```

以上内容中形如 `system_u:object_r:httpd_sys_content_t:s0` 这样的内容，称之为「安全上下文」。

【安全上下文解释】

安全上下文共有5个字段，分别使用 `:` 分割，最后一个字段可省略。

```
system_u:object_r:httpd_sys_content_t:s0:[类别]
身份     :角色    :类型                :灵敏度:[类别]
```

* 身份（identity）：标识数据身份，相当于权限中的用户。常见身份类型如下：
  * root：root 身份
  * system_u：系统身份
  * user_u：用户身份
* 角色（role）：表示数据是进程还是文件或目录。常见角色如下：
  * object_r：表示数据为文件或目录
  * system：表示数据是进程
* 类型（type）：用于确认进程是否可以访问文件。进程的安全上下文类型字段与文件的安全上下文类型匹配则可以访问。
  类型字段在进程的安全上下文中被称作域（domain），在文件或目录的安全上下文中被称作类型（type）。域与类型相匹配才能正确访问。
* 灵敏度：使用 `sN` 表示（N 代表数字），N 越大，灵敏度越高。
* 类别：可以使用 `seinfo` 查询。`seinfo` 具体用法见本页**参考**第 3 条。

**SELinux 启用模式**

SELinux 共有三种启用模式：

* Enforcing：强制模式。SELinux 已启用，且已经正确开始限制 domain/type。
* Permissive：宽容模式。SELinux 已启用，不过仅会有警告信息，不会实际限制 domain/type 读写。

* Disabled：关闭模式。SELinux 未启用。

查看 SELinux 启用模式：`getenforce`

**查看 SELinux 的策略**

语法

```bash
sestatus [-vb]
```

选项

* `-v` 检查文件与进程的安全上下文
* `-b` 将策略规则的布尔值列出（启动为 1，否则为 0）

**SELinux 的启动与关闭**

<table>
    <tr><td>注意</td><td>SELinux 的启动模式修改必须重启操作系统。</td></tr>
</table>

【配置修改】

使用 `vim` 修改 SELinux 的配置文件

```
vim /etc/selinux/config
```

把 `SELINUX=xxx` 中的 xxx 设置成 `enforcing`、`permissive`、`disabled` 中的一个。

【命令修改】

语法

```bash
setenforce [0|1]
```

选项

* `0`：转成 Permissive 模式
* `1`：转成 Enforcing 模式

注意

<table>
    <tr><td>如果 getenforce 的结果是 Disabled，则无法通过 setenforce 切换启动模式</td></tr>
</table>

### 参考

1. 《鸟哥的Linux私房菜》第16、17章
2. [中国大学慕课-Linux系统管理](https://www.icourse163.org/course/NBCC-437004?tid=1002729007) 视频教程
3. [SELinux系列（六）——SELinux安全上下文查看方法 详细介绍](https://blog.csdn.net/weixin_42350212/article/details/107843489)