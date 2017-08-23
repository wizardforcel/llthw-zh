# 练习 14：包管理：Debian 包管理工具`aptitude`

> 原文：[Exercise 14. Package management: Debian package management utility aptitude](https://archive.fo/NUuCN)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

现在是时候获得一些神圣的知识，向 Linux 系统添加新程序了。Linux 中的程序称为软件包，通常通过称作包管理器的工具，从网络仓库安装 。

+   软件包通常是一个压缩的程序，你可以像这样安装软件包：`aptitude install program...`。为了避免安装恶意程序，所有软件包都由其创建者进行数字签名，这意味着，如果软件包在创建后修改，包管理器不允许你安装它。
+   包管理器是一个程序，允许你安装其他程序。许多程序依赖于其他程序，例如使用对话窗口的程序通常需要一个程序，它知道如何绘制这些窗口。包管理器知道这些依赖关系，当你要求它安装一个特定的程序时，它会安装所需的所有程序，你要求的程序需要这些程序来工作。Debian 包管理器称为`aptitude`。

网络仓库是一个包含许多软件包的站点，可以随时安装。

这是程序安装的典型概述：

```
你
   使用包管理器搜索可用的程序
   请求包管理器安装程序
包管理器
    查找安装当前程序所需的所有程序
    在包管理器数据库中，为安装标记它们
    安装所有需要的程序，包括你所需的程序
        下载所有需要的程序
        从这些软件包提取文件，放到由 FHS 标准定义的，系统上的位置
            对于每个程序，运行一个特殊的安装脚本，允许它执行初始操作：
                创建目录
                创建数据库
                生成默认配置文件
                ...... 
    通过将已安装程序的状态修改为已安装，更新系统包的数据库
你
    能够立即运行你新安装的程序
```

现在是时候了解提取文件的位置。在 Linux 中，所有相同类型的文件都安装在相同的位置。例如，所有程序的可执行文件都安装在`/usr/bin`中，程序的文档在`/usr/share/doc`中，以及其它。这可能听起来有点凌乱，但它是非常有用的。一个名为 FHS 的标准文件定义了哪些文件在哪里，你可以通过调用`man 7 hier`来查看它 。我将在下面向你显示“文件系统层次标准”版本 2.2 的缩略版本：

+   `/` - 这是根目录。这是整棵树开始的地方。
+   `/bin` - 此目录包含在单用户模式下需要的可执行程序，并将其升级或修复。
+   `/boot` - 包含用于引导程序的静态文件。该目录仅保存引导过程所需的文件。映射安装程序和配置文件应该放在`/sbin`和`/etc`。
+   `/dev` - 特殊或设备文件，指的是物理设备。见`mknod(1)`。
+   `/etc` - 包含机器本地的配置文件。
+   `/home` - 在具有用户主目录的机器上，这些通常位于该目录下。该目录的结构取决于本地管理决策。
+   `/lib` - 此目录应该保存共享库，它们是启动系统和在根文件系统中运行命令所必需的。
+   `/media` - 此目录包含可移动介质的挂载点，如 CD 和 DVD 磁盘或 USB 记忆棒。
+   `/mnt` - 此目录是临时装载的文件系统的挂载点。在某些发行版中，`/mnt`包含子目录，用作多个临时文件系统的挂载点。
+   `/proc` - 这是`proc`文件系统的挂载点，它提供运行进程和内核的信息。这个伪文件系统在`proc(5)`中有更详细的描述。
+   `/root` - 此目录通常是`root`用户的主目录（可选）。
+   `/sbin` - 类似`/bin`，此目录包含启动系统所需的命令，但通常不会由普通用户执行。
+   `/srv` - 此目录包含由该系统提供的，站点特定的数据。
+   `/tmp` - 此目录包含临时文件，可能会在没有通知的情况下进行删除，例如通过普通任务或在系统启动时删除。
+   `/usr` - 此目录通常是从单独的分区挂载的。它应该只保存可共享的只读数据，以便它可以由运行 Linux 的各种机器来挂载。
+   `/usr/bin` - 这是可执行程序的主目录。普通用户执行的大多数程序不需要启动或修复系统，它们不在本地安装，并且应放在该目录中。
+   `/usr/local` - 这是站点本地的程序的通常位置。
+   `/usr/share` - 此目录包含具有特定应用程序数据的子目录，可以在同一操作系统的不同架构之间共享。通常可以在这里找到，以前存在于`/usr/doc`或`/usr/ lib`或`/usr/man`中的东西。
+   `/usr/share/doc` - 已安装程序的文档。
+   `/var` - 此目录包含可能会更改大小的文件，如假脱机和日志文件。
+   `/var/log` - 其他日志文件。
+   `/var/spool` - 各种程序的假脱机（或排队）文件。
+   `/var/tmp` - 类似`/tmp`，此目录保存临时文件，不知道存储多长时间。

真的很长，但是你不需要记住它，`man hier 7`总是在那里。现在你只需要知道`/usr/bin`，`/usr/share`和`/var/log`。

让我们再谈谈软件包和包管理器。首先让我们重复一下：

+   每个程序都叫做软件包。
+   包管理器管理所有软件包，即安装或卸载它们。
+   为此，包管理器拥有一个已安装和可用软件包的数据库。

此数据库中的每个包都具有状态，指示是否安装了软件包，软件包是否可以更新，以及其它。你可以通过键入`dpkg -l`打印当前安装的软件包。示例输出如下所示：

```
user1@vm1:~$ dpkg -l | head | less -S
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                      Version                Description
+++-=====================-====================-========================================================
ii  acpi                  1.5-2                displays information on ACPI devices
ii  acpi-support-base     0.137-5              scripts for handling base ACPI  events such as the power
ii  acpid                 1:2.0.7-1squeeze4    Advanced Configuration and Power Interface event daemon
```

你可以看到，这些状态显示在前三列中。从这个输出可以看出，所有的包都需要安装，或者确实已安装，没有错误，因为第三列是空的。以下是所有可能的包状态列表。

第一列。预期的操作，我们想要对软件包做的事情：

+   `u` = 未知（未知状态）
+   `i` = 安装。选择该软件包进行安装。
+   `r` = 选择该软件包进行卸载（即我们要删除所有文件，但配置文件除外）。
+   `p` = 清理 选择软件包进行清理（即我们要从系统目录，甚至配置文件中删除所有东西）。
+   `h` = 标记为保留的包，不由`dpkg`处理，除非强制使用选项`-force-hold`。

第二列。软件包状态，软件包目前是什么状态：

+   `n` = 未安装。该软件包未安装在你的系统上。
+   `c` = 配置文件。系统上只存在该包的配置文件。
+   `H` = 半安装。包的安装已经启动，但由于某种原因未完成。
+   `U` = 已解压缩。该软件包已解压缩，但未配置。
+   `F` = 半配置。软件包已解压缩，配置已启动，但由于某些原因尚未完成。
+   `W` = 触发器等待。软件包等待另一个包的触发器处理。
+   `t` = 触发中。软件包已被触发。
+   `i` = 已安装.该软件包已解压缩并配置好。

第三栏。出错的东西。

+   `R` = 需要恢复。标有“需要恢复”的软件包已损坏，需要重新安装。这些包不能被删除，除非强制使用选项`-force-remove-reinstreq`。

同样，你不需要记住它，只需记住`info dpkg`命令，它将显示这些信息。现在不要纠结包状态，只要记住，`ii`状态意味着这个包一切正常。

好了，让我们安装一个名为`midnight commander`的程序，它是一个文件管理器，它允许你直观地浏览系统上的目录，并对你的文件执行复制，重命名或删除操作。

现在，你将了解如何搜索，安装和删除软件包。

## 这样做

```
1: aptitude search mc | grep -i 'midnight commander'
2: sudo aptitude install mc
3: dpkg -L mc | grep '/usr/bin'
4: aptitude search mc | grep -i 'midnight commander'
5: mc
6: <F10><ENTER>
7: sudo aptitude remove mc
```

## 你应该看到什么

```
user1@vm1:~$ aptitude search mc | grep -i 'midnight commander'
p   mc                              - Midnight Commander - a powerful file manag
p   mc-dbg                          - Midnight Commander - a powerful file manag
user1@vm1:/home/user1# sudo aptitude install mc
The following NEW packages will be installed:
  libglib2.0-0{a} libglib2.0-data{a} mc shared-mime-info{a}
0 packages upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,957 kB/5,157 kB of archives. After unpacking 17.0 MB will be used.
Do you want to continue? [Y/n/?] y
Get:1 http://mirror.yandex.ru/debian/ squeeze/main libglib2.0-0 amd64 2.24.2-1 [1,122 kB]
Get:2 http://mirror.yandex.ru/debian/ squeeze/main libglib2.0-data all 2.24.2-1 [994 kB]
Get:3 http://mirror.yandex.ru/debian/ squeeze/main shared-mime-info amd64 0.71-4 [841 kB]
Fetched 2,957 kB in 0s (4,010 kB/s)
Selecting previously deselected package libglib2.0-0.
(Reading database ... 24220 files and directories currently installed.)
Unpacking libglib2.0-0 (from .../libglib2.0-0_2.24.2-1_amd64.deb) ...
Selecting previously deselected package libglib2.0-data.
Unpacking libglib2.0-data (from .../libglib2.0-data_2.24.2-1_all.deb) ...
Selecting previously deselected package mc.
Unpacking mc (from .../mc_3%3a4.7.0.9-1_amd64.deb) ...
Selecting previously deselected package shared-mime-info.
Unpacking shared-mime-info (from .../shared-mime-info_0.71-4_amd64.deb) ...
Processing triggers for man-db ...
Setting up libglib2.0-0 (2.24.2-1) ...
Setting up libglib2.0-data (2.24.2-1) ...
Setting up mc (3:4.7.0.9-1) ...
Setting up shared-mime-info (0.71-4) ...
user1@vm1:~$ aptitude search mc | grep -i 'midnight commander'
i   mc                              - Midnight Commander - a powerful file manag
p   mc-dbg                          - Midnight Commander - a powerful file manag
user1@vm1:~$ mc
  Left     File     Command     Options     Right
|<  ~ ---------------------.[^]>||<  ~ ---------------------.[^]>|
|'n  Name    | Size |Modify time||'n  Name    | Size |Modify time|
|/..         |P--DIR|un  6 21:49||/..         |P--DIR|un  6 21:49|
|/.aptitude  |  4096|un 25 18:34||/.aptitude  |  4096|un 25 18:34|
|/.mc        |  4096|un 25 18:41||/.mc        |  4096|un 25 18:41|
| .bash~story| 10149|un 21 12:01|| .bash~story| 10149|un 21 12:01|
| .bash~ogout|   220|un  6 21:48|| .bash~ogout|   220|un  6 21:48|
| .bashrc    |  3184|un 14 12:24|| .bashrc    |  3184|un 14 12:24|
| .lesshst   |   157|un 25 11:31|| .lesshst   |   157|un 25 11:31|
|----------------------------------------------------------------|
|UP--DIR                        --UP--DIR                        |
 ----------- 6367M/7508M (84%) -------------- 6367M/7508M (84%) -|
Hint: The homepage of GNU Midnight Commander: http://www.midnight-
user1@vm1:~$                                                   [^]
 1Help  2Menu  3View  4Edit  5Copy  6Re~ov 7Mkdir 8De~te 9Pu~Dn
user1@vm1:~$ sudo aptitude remove mc
The following packages will be REMOVED:
  libglib2.0-0{u} libglib2.0-data{u} mc shared-mime-info{u}
0 packages upgraded, 0 newly installed, 4 to remove and 0 not upgraded.
Need to get 0 B of archives. After unpacking 17.0 MB will be freed.
Do you want to continue? [Y/n/?] y
(Reading database ... 24637 files and directories currently installed.)
Removing shared-mime-info ...
Removing mc ...
Removing libglib2.0-data ...
Removing libglib2.0-0 ...
Processing triggers for man-db ...
user1@vm1:~$
```

## 解释

1.  搜索包含`mc`的包名称，并在描述中仅显示包含`midnight commander`的包。`grep -i`意味着，`grep`应该搜索小写和大写字母，如果没有它，`grep`不会显示包含`Midnight Commander`的行，因为它们以大写字母开头。请注意，`mc`状态为`p`状态，这意味着这个包的所需操作是清理，并且由于其他两个状态列中没有任何内容，因此我们可以得出结论，该包未安装。你的`man`注意到了，最开始你没有安装这个包，但这也没问题，因为没有安装的软件包 默认是清除状态。
1.  安装软件包`mc`。因为这个更改是系统范围的，所以这个命令需要使用超级用户，它能够写入系统中的所有目录。还要注意 debian 软件包管理器`aptitude`如何自动安装`mc`所需的`libglib2.0-0`，`libglib2.0-data`和`shared-mime-info`软件包。
1.  显示你安装的包的可执行文件。如你所见，他们放在`/usr/bin`中。
1.  调用`mc`。
1.  退出`mc`。
1.  删除`mc`。请注意，自动安装的软件包也会自动删除。如果在 安装`mc`之后，你安装一些需要这些软件包的东西，`aptitude`将保留它们。

## 附加题

好吧，东西真多。但这里还有更多：
键入`aptiutde search emacs`。弄清楚`v`的意思是什么。
阅读或浏览 Debian 手册中的[第 2 章 Debian 软件包管理](http://www.debian.org/doc/manuals/debian-reference/ch02.en.html)。
