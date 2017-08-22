# 练习 3：Bash：Shell、`.profile`、`.bashrc`、`.bash_history`。

> 原文：[Exercise 3. Bash: The shell, .profile, .bashrc, .bash_history](https://archive.fo/DKP67)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

当使用 CLI（命令行界面）来使用 Linux 时，你正在与一个名为 shell 的程序进行交互。所有你输入的都传递给 shell，它解释你输入的内容，执行参数扩展（这有点类似于代数中的花括号扩展），并为你执行程序。我们将使用的 Shell 称为 Bash，它代表 Bourne Again Shell，而 Bourne Again Shell 又是一个双关语。现在我将使用纯中文，向大家介绍一下 bash 的工作方式：

+   你
    +   登入 Linux 虚拟机
    +   你的身份由用户名（`user1`）和密码（`123qwe`）确定。
    +   Bash 执行了。
+   Bash
    +   从你的配置中读取并执行首个命令，它定义了：
        +   命令提示符是什么样子
        +   使用 Linux 时，你会看到什么颜色
        +   你的编辑器是什么
        +   你的浏览器是什么
        +   ...
    +   读取首个命令后，Bash 进入循环
        +   没有通过输入`exit`或者按下`<CTRL+D>`，来要求退出的时候：
            +   读取一行
            +   解析这一行，扩展花括号
            +   使用扩展参数执行命令

我重复一下，你输入的任何命令都不会直接执行，而是首先扩展，然后执行。例如，当你输入`ls *`时，星号`*`将扩展为当前目录中所有文件的列表。

现在你将学习如何修改你的配置，以及如何编写和查看你的历史记录。

## 这样做

```
 1: ls -al
 2: cat .profile
 3: echo Hello, $LOGNAME!
 4: cp -v .profile .profile.bak
 5: echo 'echo Hello, $LOGNAME!' >> .profile
 6: tail -n 5 .profile
 7: history -w
 8: ls -altr
 9: cat .bash_history
10: exit
```

## 你会看到什么

```
user1@vm1's password:
Linux vm1 2.6.32-5-amd64 #1 SMP Sun May 6 04:00:17 UTC 2012 x86_64
 
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
 
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jun  7 12:03:29 2012 from sis.site
Hello, user1!
user1@vm1:~$ ls -al
total 20
drwxr-xr-x 2 user1 user1 4096 Jun  7 12:18 .
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
-rw-r--r-- 1 user1 user1 3184 Jun  6 21:48 .bashrc
-rw-r--r-- 1 user1 user1  697 Jun  7 12:04 .profile
user1@vm1:~$ cat .profile
# ~/.profile: executed by the command interpreter for login shells.
# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
# exists.
# see /usr/share/doc/bash/examples/startup-files for examples.
# the files are located in the bash-doc package.
 
# the default umask is set in /etc/profile; for setting the umask
# for ssh logins, install and configure the libpam-umask package.
#umask 022
 
# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
        . "$HOME/.bashrc"
    fi
fi
 
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
echo Hello, $LOGNAME!
user1@vm1:~$ echo Hello, $LOGNAME!
Hello, user1!
user1@vm1:~$ cp -v .profile .profile.bak
`.profile' -> `.profile.bak'
user1@vm1:~$ echo 'echo Hello, $LOGNAME!' >> .profile
user1@vm1:~$ tail -n 5 .profile
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
echo Hello, $LOGNAME!
user1@vm1:~$ history -w
user1@vm1:~$ ls -altr
total 28
-rw-r--r-- 1 user1 user1 3184 Jun  6 21:48 .bashrc
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw-r--r-- 1 user1 user1  741 Jun  7 12:19 .profile.bak
-rw------- 1 user1 user1  308 Jun  7 12:21 .bash_history
-rw-r--r-- 1 user1 user1  697 Jun  7 12:25 .profile
drwxr-xr-x 2 user1 user1 4096 Jun  7 12:25 .
user1@vm1:~$ cat .bash_history
ls -al
cat .profile
echo Hello, $LOGNAME!
cp -v .profile .profile.bak
echo 'echo Hello, $LOGNAME!' >> .profile
tail -n 5 .profile
history -w
ls -altr
user1@vm1:~$ exit
logout
```

不要害怕，所有命令都会解释。行号对应“现在输入它”的部分。

## 解释

1.  打印当前目录中的所有文件，包括隐藏的文件。选项`-al`告诉`ls` 以`long`格式打印文件列表，并包括所有文件，包括隐藏文件。`.profile`和`.bash_rc`是隐藏文件，因为它们以点`.`开头。以点开头的每个文件都是隐藏的，这很简单。这两个特殊文件是 shell 脚本，它们包含登录时执行的指令。

2.  打印出你的`.profile`文件。只是这样。

3.  告诉你的 shell，你这里是 bash，输出一个字符串`Hello, $LOGNAME!`，用环境变量``$LOGNAME`替换$LOGNAME`，它包含你的登录名。

4.  将`.profile`文件复制到`.profile.bak`。选项`-v`让`cp`详细输出，这意味着它会打印所有的操作。记住这个选项，它通常用于让命令给你提供比默认更多的信息。

5.  在`.bash_rc`配置文件中添加一行。从现在开始，每次登录到`vm1`时， 都将执行该命令。注意，`>>`代表向文件添加了一些东西，但`>`意味着使用一些东西来替换文件。如果你不小心替换了`.profile`而不是向它添加，则命令

    ```
    cp -v .profile.bak .profile
    ```
    
    会向你返回旧的`.profile`文件。
    
6.  从`.profile`文件中精确打印出最后 5 行。

7.  将所有命令历史写入`.bash_history`文件。通常这是在会话结束时完成的，当你通过键入`exit`或按`<CTRL> + D`关闭它。

8.  打印当前目录中的文件。选项`-tr`表示文件列表按时间反向排序。这意味着最近创建和修改的文件最后打印。注意你现在有两个新的文件。

9.  打印出保存命令历史记录的文件。注意你所有的输入都在这里。

0.  关闭会话

## 附加题

+   在线搜索为什么`ls -al`告诉你“总共 20”，但是只有 5 个文件存在。 这是什么意思？ 请注意，`.`和`..`是特殊文件条目，分别对应于当前目录和父目录的。

+   登录`vm1`并键入`man -K /etc/profile`，现在使用光标键滚动到`INVOCATION`部分并阅读它。 要退出`man`，请键入`q`。 键入`man man`来找出`man -K`选项的含义。

+   在命令之前键入`uname`与空格。 现在，键入`history`。 看到了吗？如果你将空格放到命令前面，则不会将其保存在历史记录中！提示：当你需要在命令行上指定密码时，很实用。

+   找到 bash 的 wiki 页面，并尝试阅读它。不用担心，如果它吓到你，只需要省略可怕的部分。

