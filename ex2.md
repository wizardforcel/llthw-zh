# 练习 2：文本浏览器，少即是多

> 原文：[Exercise 2. Text Viewer, The: less is More](https://archive.fo/nFH4J)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

现在你可以编辑文本文件，这很好。但是如果你只想查看一个文本文件呢？当然，你可以使用 vim，但很多时候它是过度的。还有两件事要考虑：

+   如果你想查看非常大的文件，你将需要在尽可能快的程序中查看它。
+   通常你不想意外地改变文件中的某些东西。

所以，我向你介绍强大的`less`，少即是多。“比什么多呢？”你可能会问。嗯...有一次，有一个被称为`more`的浏览器。它很简单，只是向你显示你要求它显示的文本文件。它是如此简单，只能以一个方向显示文本文件，也就是向前。 马克·恩德尔曼（Mark Nudelman）发现它并不那么令人满意 ，1983 年至 1985 年，他编写了`less`。从那以后，它拥有了许多先进的功能。因为它比`more`更先进，一句话就诞生了：“少即是多，多即是少”。

好吧，让我们试试吧。

输入：

```
less .bashrc
```

你应该看到：

```
user1@vm1:~$ less .bashrc
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples
 
# If not running interactively, don't do anything
[ -z "$PS1" ] && return
 
# don't put duplicate lines in the history. See bash(1) for more options
# don't overwrite GNU Midnight Commander's setting of `ignorespace'.
HISTCONTROL=$HISTCONTROL${HISTCONTROL+:}ignoredups
.bashrc
```

如果你的终端不是足够宽，文本将看起来像一团糟，因为它放不下整行。要修复它，请键入`- -ch<ENTER><ENTER>`。是的，`dash-dash-ch-ENTER-ENTER`。这将开启水平滚动。

为了向上向下文浏览文字，使用已经熟悉的`j`和`k`。退出按`q`。

现在我将向你展示`less`的高级功能，这样你只能看到所需的那些行。键入`&enable<ENTER>`。你应该看到这个：

```
# enable color support of ls and also add hand
# enable programmable completion features (you
# this, if it's already enabled in /etc/bash.b
~
~
~
~
~
~
~
~
~
~
~
~
& (END)
```

注意看！为了移除过滤器，只需键入`&<ENTER>`。同样，要记住的命令：

+   `j` - 向上移动
+   `k` - 向下移动
+   `q` - 退出`less`。
+   `- -chop-long-lines或`- -ch<ENTER><ENTER>` - 开启水平滚动。
+   `/` - 搜索。
+   `&something` - 只显示文件中包含某些内容的行。

## 附加题

+   Linux 具有在线手册，通过键入`man`来调用。默认情况下，在我们的系统中，本手册将使用`less`来查看。 键入`man man`并阅读，然后退出。
+   就是这样，没有更多的附加题了。
