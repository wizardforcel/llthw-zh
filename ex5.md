# 练习 5：Bash：环境变量，`env`，`set`，`export`

> 原文：[Exercise 5. Bash: environment variables, env, set, export](https://archive.fo/M8Ndm)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

请考虑以下内容：你希望程序打印出你的用户名。这个程序怎么知道的？在 Linux 中有一些环境变量。这意味着你的 shell 中有许多变量，其中许多变量自动设置，每次运行程序时，其中一些变量将传递给该程序。

详细说明：

+   一些变量只为你当前的 shell 设置。它们被称为本地 shell 变量。你可以通过键入`set`，一个 bash 内置命令来列出它们 ，这意味着没有启动其它程序，之后你执行了它。此命令由 bash 本身处理。

+   其他变量被传递到你从当前 shell 启动的每个程序。它们被称为环境变量，可以通过`env`程序列出，这意味着，通过键入`env`， 你将看到，你启动的每个程序获得了什么变量。

你可能想要深入挖掘。要做到这一点，掌握 [subshell](http://en.wikipedia.org/wiki/Child_process) 的概念，这是一个子进程，当你运行程序时创建，并且成为你的程序。

现在，你将学习如何创建变量以及如何使用变量。

## 这样做

```
1: foo='Hello World!'
2: echo $foo
3: set | grep foo
4: env | grep foo
5: export foo
6: env | grep foo
7: foo='ls -al'
8: $foo
```

## 你会看到什么

```
uset1@vm1:~$ foo='Hello World!'
user1@vm1:~$ echo $foo
Hello World!
user1@vm1:~$ set | grep foo
foo='Hello World!'
user1@vm1:~$ env | grep foo
user1@vm1:~$ export foo
user1@vm1:~$ env | grep foo
foo=Hello World!
user1@vm1:~$ foo='ls -al'
user1@vm1:~$ $foo
total 36
drwxr-xr-x 2 user1 user1 4096 Jun 14 12:23 .
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw------- 1 user1 user1 4042 Jun 15 18:52 .bash_history
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
-rw-r--r-- 1 user1 user1 3184 Jun 14 12:24 .bashrc
-rw------- 1 user1 user1   50 Jun 15 18:41 .lesshst
-rw-r--r-- 1 user1 user1  697 Jun  7 12:25 .profile
-rw-r--r-- 1 user1 user1  741 Jun  7 12:19 .profile.bak
-rw-r--r-- 1 user1 user1  741 Jun  7 13:12 .profile.bak1
```

## 解释

1.  创建变量`foo` ，并将`Hello World!`这一行放在其中。
2.  打印出变量`foo`。
3.  打印所有环境变量的列表，它传递给`grep`，打印出只包含变量`foo`的行。注意变量`foo`存在。
4.  打印所有环境变量列表，它们传递给你执行的任何程序。注意变量`foo`不存在。
5.  使变量`foo`可用于从当前 shell 执行的所有程序。
6.  现在你可以看到，你的变量确实可用于你执行的所有程序。
7.  将`ls /`放入变量`foo`。
8.  执行包含在变量`foo`中的命令。

## 附加题

+   输入并执行`env`和`set`。看看有多少个不同的变量？不用担心，通常你可以通过谷歌搜索，快速了解一个变量的作用。尝试这样做。

+   尝试输入：

    ```
    set | egrep '^[[:alpha:]].*$'
    ```
    
    现在，试试`set`。看见我在这里做了什么嘛？快速的解释是：
    
    ```
    egrep '^[[:alpha:]].*$'
    ```
    
    仅仅打印出以字母开头的行，它是每个字母，并忽略其他行。现在不要纠结这个，也不要纠结`|`符号。以后我会解释它。
    
+   在这里阅读`env`和`set`之间的区别：<http://stackoverflow.com/questions/3341372/differnce-between-the-shell-and-environment-variable-in-bash>。记住，stackoverflow 是你的朋友！我会重复多次。

+   尝试输入`set FOO=helloworld bash -c 'echo $FOO'`。这里是解释：<http://stackoverflow.com/questions/10938483/bash-specifying-environment-variables-for-echo-on-command-line>。同样，不要太纠结，你会在大量练习之后再次碰到它，我保证。

+   试试这个：

    ```
    PS1_BAK=$PS1
    PS1='Hello, world! $(pwd) > '
    PS1=$PS1_BAK
    ```
    
    注意我使用`$(pwd)`，将命令输出作为变量访问。现在，键入`man bash /PS1`（是的，只是斜杠），按下`<ENTER>`。你现在可以按下`n`查看下一个结果。浏览 PROMPTING，并键入`q`来退出`man`。现在，访问 <http://stackexchange.com/> 并搜索`bash PS1`。了解这两个文档来源的区别。
    