# 练习 10：Bash：程序退出代码（返回状态）

> 原文：[Exercise 10. Bash: program exit code (return status)](https://archive.fo/ygzso)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

让我们假设你要复制一个目录。你可以通过键入`cp -vR /old/dir/path /new/dir/path`来执行此操作。发出此命令后，你可能想知道如何进行。目录是否被复制？还是出现了一些错误，因为目标目录空间不足，或其他出现错误的东西？

为了理解它是如何工作的，你必须了解两个程序如何通信。我们先这样说，bash 只是另一个程序，所以一般来说，当你发出上述的`cp`命令时，一个程序（bash，它是父进程）调用了另一个程序（`cp`，它是子进程）。

在 Linux 中，有一个标准机制，用于获取从子进程到父进程的信息，这个机制称为[退出状态或返回代码](http://en.wikipedia.org/wiki/Exit_status)。通过使用这种机制，当子进程完成其工作时，一个小的数字从子进程（或被调用者，这里是`cp`）传递给父进程（或调用者，这里是 bash）。当程序在执行期间没遇到错误时，它返回`0`，如果发生某些错误，则此代码不为零。就是这么简单。Bash 中的这个退出代码保存到`?`环境变量，你现在知道了，可以使用`$?`来访问。

让我再次重复一下我现在所说的话：

```
Bash 等待你的输入
Bash 解析你的输入
Bash 为你启动程序，并等待这个程序退出
    程序启动
    程序做你让他做的事情
    程序生成了退出代码
    程序退出并且将退出代码返回给 Bash
Bash 将这个退出代码赋给变量 ?
```

现在你学到了如何打印出你的程序的退出状态。

## 这样做

```
1: ls
2: echo $?
3: ls /no/such/dir
4: echo $?
```

## 你会看到什么

```
user1@vm1:~$ ls
hello.txt  ls.out
user1@vm1:~$ echo $?
0
user1@vm1:~$ ls /no/such/dir
ls: cannot access /no/such/dir: No such file or directory
user1@vm1:~$ echo $?
2
user1@vm1:~$
```

## 解释

+   打印出一个目录，成功。
+   打印出`ls`的退出代码，它是`0`，这意味着`ls`没有遇到任何错误。
+   尝试打印出不存在的目录，当然失败。
+   打印`ls /no/such/dir`的退出代码，它确实是非零。

## 附加题

阅读`man ls`的退出代码部分。
