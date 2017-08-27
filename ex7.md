# 练习 7：Bash：重定向，`stdin`，`stdout`，`stderr`，`<`，`>`，`>>`，`|`，`tee`，`pv`

> 原文：[Exercise 7. Bash: redirection, stdin, stdout, stderr, <, >, >>, |, tee, pv](https://archive.fo/hZqGb)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

在 Linux 中，一切都只是文件。这意味着，对于控制台程序：

+   键盘表示为一个文件，Bash 从中读取你的输入。
+   显示器表示为一个文件，Bash向输出写入它。

让我们假设，你有一个程序可以计算文件中的行。你可以通过键入`wc -l`来调用它。现在尝试一下 没有发生什么事吧？它只是卡在那里。错了，它正在等待你的输入。这是它的工作原理：

```
line_counter = 0
while end of file is not reached
    read a line
    add 1 to line_counter
print line_counter
```

所以`wc`目前从`/dev/tty`读取行，这在当前上下文中是你的键盘。每次你按下回车，`wc`都会获取一行。任意键入几行，然后按`CTRL + D`，这将为`wc`产生`EOF`字符，使其明白达到[文件末尾](http://en.wikipedia.org/wiki/End-of-file)。现在它会告诉你你输入了多少行。

但是如果你想计算现有文件中的行呢？那么，这就需要重定向 了！为了在其输入上提供现有文件，请键入以下内容：`wc -l < .bash_history`。你看，它有作用！真的是那么简单！[重定向](http://en.wikipedia.org/wiki/Redirection_%28computing%29) 是一种机制，允许你告诉程序，将其来自键入输入和/或到显示器的输出，重定向到另一个文件。为此，你可以使用这些特殊命令，然后启动程序：

+   `<` - 用文件替换标准输入（例如键盘）。
+   `>` - 用文件替换标准输出（例如显示器）。警告！此命令将覆盖 你的指定文件的内容，因此请小心。
+   `>>` - 与上面相同，但不是覆盖 文件，而是写入到它的末尾，保存在该文件中已存在的信息。小心不要混淆两者。
+   `|` - 从一个程序获取输出，并将其连接到另一个程序。这将在下一个练习中详细阐述。

现在，你将学习如何将程序的输入和输出重定向到文件或其他程序。

## 这样做

```
 1: sudo aptitude install pv
 2: read foo < /dev/tty
 3: Hello World!
 4: echo $foo > foo.out
 5: cat foo.out
 6: echo $foo >> foo.out
 7: cat foo.out
 8: echo > foo.out
 9: cat foo.out
10: ls -al | grep foo
11: ls -al | tee ls.out
12: dd if=/dev/zero of=~/test.img bs=1M count=10
13: pv ~/test.img | dd if=/dev/stdin of=/dev/null bs=1
14: rm -v foo.out
15: rm -v test.img
```

## 你应该看到什么

```
user1@vm1:~$ sudo aptitude install pv
The following NEW packages will be installed:
  pv
0 packages upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/28.9 kB of archives. After unpacking 143 kB will be used.
Selecting previously deselected package pv.
(Reading database ... 39657 files and directories currently installed.)
Unpacking pv (from .../archives/pv_1.1.4-1_amd64.deb) ...
Processing triggers for man-db ...
Setting up pv (1.1.4-1) ...
 
user1@vm1:~$ read foo < /dev/tty
Hello World!
user1@vm1:~$ echo $foo > foo.out
user1@vm1:~$ cat foo.out
Hello World!
user1@vm1:~$ echo $foo >> foo.out
user1@vm1:~$ cat foo.out
Hello World!
Hello World!
user1@vm1:~$ echo > foo.out
user1@vm1:~$ cat foo.out
 
user1@vm1:~$ ls -al | grep foo
-rw-r--r-- 1 user1 user1    1 Jun 15 20:03 foo.out
user1@vm1:~$ ls -al | tee ls.out
total 44
drwxr-xr-x 2 user1 user1 4096 Jun 15 20:01 .
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw------- 1 user1 user1 4865 Jun 15 19:34 .bash_history
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
-rw-r--r-- 1 user1 user1 3184 Jun 14 12:24 .bashrc
-rw-r--r-- 1 user1 user1    1 Jun 15 20:03 foo.out
-rw------- 1 user1 user1   50 Jun 15 18:41 .lesshst
-rw-r--r-- 1 user1 user1    0 Jun 15 20:03 ls.out
-rw-r--r-- 1 user1 user1  697 Jun  7 12:25 .profile
-rw-r--r-- 1 user1 user1  741 Jun  7 12:19 .profile.bak
-rw-r--r-- 1 user1 user1  741 Jun  7 13:12 .profile.bak1
-rw-r--r-- 1 user1 user1    0 Jun 15 20:02 test.img
user1@vm1:~$ dd if=/dev/zero of=~/test.img bs=1M count=10
10+0 records in
10+0 records out
10485760 bytes (10 MB) copied, 0.0130061 s, 806 MB/s
user1@vm1:~$ pv ~/test.img | dd if=/dev/stdin of=/dev/null bs=1
  10MB 0:00:03 [3.24MB/s] [=================================================================================>] 100%
10485760+0 records in
10485760+0 records out
10485760 bytes (10 MB) copied, 3.10446 s, 3.4 MB/s
user1@vm1:~$ rm -v foo.out
removed `foo.out'
user1@vm1:~$ rm -v test.img
removed `test.img'
user1@vm1:~$
```

## 解释

1.  在你的系统上安装`pv`（管道查看器）程序，稍后你需要它。
2.  将你的输入读取到变量`foo`。这是可能的，因为显示器和键盘实际上是系统的电传打字机。是的，[那个](http://www.google.ru/search?rlz=1C1CHKZ_enRU438RU438&sugexp=chrome,mod%3D11&q=unix+filter&um=1&ie=UTF-8&hl=en&tbm=isch&source=og&sa=N&tab=wi&ei=QWDbT7LILsTi4QTJxNXWCg&biw=1116&bih=875&sei=Q2DbT93XOLLS4QTmst2ACg%23um=1&hl=en&newwindow=1&rlz=1C1CHKZ_enRU438RU438&tbm=isch&sa=1&q=teletype&oq=teletype&aq=f&aqi=g10&aql=&gs_l=img.3..0l10.455489.456448.4.456736.8.6.0.2.2.0.144.567.4j2.6.0...0.0.Qa6W2PHvUWw&pbx=1&bav=on.2,or.r_gc.r_pw.r_qf.,cf.osb&fp=e87c07212bd9e2a6&biw=1116&bih=875)电传打字机！在线阅读更多关于`tty`的东西。
3.  这是你输入的东西。
4.  将`foo`变量的内容重定向到`foo.out`文件，在进程中创建文件或覆盖现有文件，而不会警告删除所有内容！
5.  打印出`foo.out`的内容。
6.  将`foo`变量的内容重定向到`foo.out`文件，在进程中创建文件或附加 到现有文件。这是安全的，但不要混淆这两者，否则你会有巨大的悲剧。
7.  再次打印出`foo.out`内容。
8.  将空内容重定向到`foo.out`，在进程中清空文件。
9.  显示文件确实是空的。
0.  列出你的目录并将其通过管道输出到`grep`。它的原理是，获取所有`ls -al`的输出，并将其扔给`grep`。这又称为管道。
1.  将你的目录列出到屏幕上，并写入`ls.out`。很有用！
2.  创建大小为 10 兆字节的清零文件。现在不要纠结它如何工作。
3.  将这个文件读取到`/dev/null`，这是你系统中终极的垃圾桶，什么都没有。写入它的一切都会消失。请注意，`pv`会向你展示读取文件的进程，如果你尝试使用其他命令读取它，你就不会知道它需要多长时间来完成。
4.  删除`foo.out`。记得自己清理一下。
5.  删除`test.img`。

## 附加题

+   阅读 [stackoverflow](http://stackoverflow.com/questions/5802879/difference-between-pipelining-and-redirection-in-linux) 上的管道和重定向，再次阅读 [stackoverflow](http://stackoverflow.com/questions/19122/bash-pipe-handling?rq=1) 和 [Greg 的 Wiki](http://mywiki.wooledge.org/BashFAQ/024)，这是非常有用的资源，记住它。
+   打开 bash 的`man`页面，向下滚动到 REDIRECTION 部分并阅读它。
+   阅读`man pv`和`man tee`的描述。
