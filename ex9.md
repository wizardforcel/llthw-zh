# 练习 9：Bash：任务控制，`jobs`，`fg`

> 原文：[Exercise 9. Bash: job control, jobs, fg](https://archive.fo/z1oWk)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

Linux是一个[多任务](http://en.wikipedia.org/wiki/Computer_multitasking)操作系统。这意味着有许多程序同时运行。从用户的角度来看，这意味着你可以同时运行几个程序，而且 bash 肯定有工具，为你控制多个任务的执行。为了能够使用此功能，你需要学习以下命令：

+   `<CTRL> + z` - 将当前运行的程序放在后台。
+   `jobs` - 列出所有后台程序。
+   `fg` - 把程序带到前台。`fg`接受一个数字作为参数，它可以从`jobs`中获取数，或者如果无参数调用，则将最后一个挂起的程序带到前台。
+   `ctrl + c` - 一次性停止执行当前运行的程序。虽然我不会在这个练习中使用它，但我必须说，这可能是非常有用的。

现在，你将学习如何使用 bash 内置的工具来控制程序的执行。

## 这样做

```
 1: less -S .profile
 2: <CTRL+z>
 3: less -S .bashrc
 4: <CTRL+z>
 5: less -S .bash_history
 6: <CTRL+z>
 7: jobs
 8: fg
 9: q
10: fg
11: q
12: fg
13: q
14: fg
15: jobs
```

## 你会看到什么

```
user1@vm1:~$ less -S .profile
# exists.
# see /usr/share/doc/bash/examples/startup-files for
# the files are located in the bash-doc package.
 
# the default umask is set in /etc/profile; for setti
# for ssh logins, install and configure the libpam-um
#umask 022
 
# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
        . "$HOME/.bashrc"
 
[1]+  Stopped                 less -S .profile
user1@vm1:~$ less -S .bashrc
# for examples
 
# If not running interactively, don't do anything
[ -z "$PS1" ] && return
 
# don't put duplicate lines in the history. See bash(
# don't overwrite GNU Midnight Commander's setting of
HISTCONTROL=$HISTCONTROL${HISTCONTROL+:}ignoredups
# ... or force ignoredups and ignorespace
HISTCONTROL=ignoreboth
 
# append to the history file, don't overwrite it
shopt -s histappend
 
[2]+  Stopped                 less -S .bashrc
user1@vm1:~$ less -S .bash_history
echo Hello, $LOGNAME!
echo 'echo Hello, $LOGNAME!' >> .profile
cp .profile .profile.bak
tail .profile
ls -altr
history -w
ls -al
cat .profile
echo Hello, $LOGNAME!
echo 'echo Hello, $LOGNAME!' >> .profile
cp .profile .profile.bak
tail .profile
ls -altr
 
[3]+  Stopped                 less -S .bash_history
user1@vm1:~$ jobs
[1]   Stopped                 less -S .profile
[2]-  Stopped                 less -S .bashrc
[3]+  Stopped                 less -S .bash_history
user1@vm1:~$ fg
user1@vm1:~$ fg
user1@vm1:~$ fg
user1@vm1:~$ fg
-bash: fg: current: no such job
user1@vm1:~$ jobs
user1@vm1:~$
```

## 解释

1.  打开`.profile`来查看。注意我如何使用`-S`参数，让`less`开启`-chop-long-lines`选项来启动。
1.  挂起`less`。
1.  打开`.bashrc`来查看。
1.  挂起`less`。
1.  打开`.bash_history`来查看。
1.  挂起`less`。
1.  打印挂起程序的列表。
1.  切换到`less`。
1.  退出它。
1.  切换到第二个`less`。
1.  退出它。
1.  切换到第一个`less`。
1.  退出它。
1.  尝试切换到最后一个程序。没有任何程序，但你这样做是为了确保确实没有。
1.  打印挂起程序的列表。这是为了确保没有后台任务，通过看到`jobs`打印出空的输出。

## 附加题

打开`man bash`，搜索 JOB CONTROL，输入`/, JOB CONTROL, <ENTER>`，并阅读它。
