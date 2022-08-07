## 软件管理

Linux 包管理工具主要分为两类：**dpkg** 和 **rpm**。dpkg 属于debian 系的默认包管理工具，比如 ubuntu 使用的就是 dpkg；rpm 属于 redhat 系的包管理工具，比如 CentOS 使用的就是 dpkg。apt-get 是 dpkg 的在线升级命令；yum 是 rpm 的在线升级命令。

这里主要讲 CentOS 的包管理工具。

### 使用 `rpm` 管理软件

rpm 在安装软件时，会先去 `/var/lib/rpm` 的 rpm 数据库中查询依赖的软件是否存在，如果不存在，则不能安装。这个问题称为【软件属性依赖】问题。

虽然 rpm 无法解决软件依赖问题，但还是有实用的地方的。同时由于 rpm 是 redhat 系官方包管理工具，许多软件安装可能绕不开 rpm，因此有必要学习一下。

#### 安装、更新、移除

**安装**

```bash
rpm -ivh <package_name>
```

- -i：安装
- -v：显示指令执行过程
- -h或 --hash：套件安装时列出标记

> rpm 的安装是离线安装，安装包必须存在才能安装

**更新**

```bash
rpm -Uvh <package_name>  # 名为 package_name 的软件不存在则安装，否则更新
```

```bash
rmp -Fvh <package_name> # 名为 package_name 的软件存在时则更新，否则什么也不做
```

**移除**

```bash
rpm -e <package_name> # 名为 package_name 的软件没有被依赖时则移除，否则报错
```

e 代表 erase（中文直译：抹去，擦除）

**通用长格式命令**

* `--nodeps` 无视依赖。安装时使用，无视依赖继续安装；移除时使用，无视依赖继续删除
* `--replacefiles` 强制替换文件
* `--replacpkgs` 强制替换包
* `--force` 相当于 `--replacefiles` 和 `--replacpkgs` 综合
* `--test` 测试是否有依赖性（比如和 `-ivh` 连用，则不进行安装，只检验是否依赖）
* `--justdb` 更新 rpm 数据库的错误或损坏数据

**重建数据库**

```bash
rpm --rebuild
```

#### 查询、验证

**通用**

`-a`, `--all` 查询或验证全部

`-f`, `-file` 查询或验证文件所属软件包

`-p`, `--package` 查询或验证软件包文件

**查询**

```bash
rpm -q <package_name> # 查询软件是否安装
rpm -qa # 列出所有已安装软件名称
rpm -qf <file_name> # 查询文件所属的软件名
rpm -q[licdR] <package_name> 
rpm -qp[licdR] <package_name>
```

`licdR` 分别表示的含义如下

* `-l` 显示软件的所有文件与目录
* `-i` 显示软件详细信息
* `-c` 显示所有配置文件
* `-d` 显示所有说明文件
* `-R` 显示本软件依赖包含的文件

`-qp` 查询 rpm 文件内的信息

**验证**

```bash
rpm -V <package_name> # 验证已安装的软件是否被修改
rpm -Va # 列出本机安装的所有软件是否被修改
rpm -Vp <package_name> # 列出该软件被修改过的文件
rpm -Vf <file_name> # 验证某个文件是否被修改
```

V 代表 verify，验证的意思。`-V` 命令输出的完整格式如下：

```
SM5DLUGTP c|d|g|l|r file
```

以空格分隔，第一段表示文件改变的内容，第二段表示文件的类型，第三段是文件的绝对路径

第一段：如果文件内容改变了，则第一段显示相应位置的字母，否则显示 `.`。这里挑几个重要的解释一下

* S（Size）文件大小
* M（Mode）文件的类型或属性（rwx）
* 5（MD5）MD5。MD5改变意味着内容的改变，所以可以简单理解为内容是否改变
* U（User）所属用户
* G（Group）所属用户组
* T（mTime）创建时间

例：

```
[root@50cc94ef8d05 /]# rpm -Va
S.5....T.  c /etc/dnf/vars/infra
```

上述示例表示文件的大小、内容、创建时间都改变了

第二段：文件的类型

* `c`：配置文件（config file）
* `d`：数据文件（documentation）
* `g`：幽灵文件（ghost file）
* `l`：许可证文件（license file）
* `r`：自述文件（read me）

> `rpm -Va` 可以帮我们验证系统上有没有数据被改动，如果二进制程序有被改动过，那么这个机器很有可能被入侵了。

### 使用 `yum` 管理软件

yum 全称 yellowdog updater modified （经我直译为：修改版黄狗更新器）。另外，yum 需要根用户权限。

#### 语法

```bash
yum [选项] 命令
```

命令

- `list [keyword]` 列出软件与版本，类似于 `rpm -qa`
- `info [software]` 列出软件具体信息，类似于 `rpm -qai`
- `search <keyword>` 根据关键字查找某软件具体名
- `install <software>` 安装软件
- `update <software>` 更新软件
- `remove <software>` 删除软件
- `provides <filename>` 根据文件名找软件名

选项

- `-y` 所有的询问选项都自动填 y
- `--installroot` 手动指定安装目录

#### 安装软件

```bash
yum install <软件名>
```

```
yum -y install <软件名>  #表示安装时所有提示默认yes
```

#### 查询软件状态

如果软件已安装，最后一列会被 `@` 标记，否则未安装

```
yum list <软件名>
```

yum 支持通配符。查找以 soft 开头的软件，可以这么写：

```
yum list soft*
```

例：查看已安装的软件

```bash
yum list installed
```

> 如果已安装，将提示“已安装的软件”。如果未安装，将提示“可安装的软件”。如果安装软件不存在 yum 服务器中，将提示“不存在”

#### 查看软件信息

```bash
yum info <软件名>
```

#### 更新软件

```bash
yum update <软件名>
```

#### 移除软件

```bash
yum remove <软件名>
```

### 设置软件安装源

由于 yum 使用的是 CentOS 的官方软件源，可能下载速度较慢。所以建议更改软件安装源。

（TODO）

### 参考资料

1. [中国大学慕课-Linux系统管理](https://www.icourse163.org/course/NBCC-437004?tid=1002729007) 视频教程
2. 《鸟哥的Linux私房菜》第22章
3. https://man.linuxde.net/rpm
4. `rpm --help` 和 `yum --help`

