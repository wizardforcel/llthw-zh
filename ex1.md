# 练习 1：文本编辑器，vim

> 原文：[Exercise 1. Text Editor, The: vim](https://archive.fo/5vf0X)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

在 Linux 中，就像任何类 Unix 操作系统，一切都只是文件。而 Unix 哲学指出，配置文件必须是人类可读和可编辑的。在几乎所有的情况下，它们只是纯文本。所以，首先，你必须学习如何编辑文本文件。

为此，我强烈建议你学习 vim 的基础知识，这是在 Linux 中处理文本的最强大的工具之一。Vim 是由 Bill Joy 于 1976 年编写的，[vi](http://en.wikipedia.org/wiki/Vi) 的重新实现。vi 实现了一个非常成功的概念，甚至 Microsoft Visual Studio 2012 有一个[插件](http://visualstudiogallery.msdn.microsoft.com/59ca71b3-a4a3-46ca-8fe1-0e90e3f79329/)，它提供了一个模式，与这个超过 35 岁的编辑器兼容。你可以在这里玩转它（[这是在浏览器中运行的真正的 Linux](https://bellard.org/jslinux/vm.html?url=https://bellard.org/jslinux/buildroot-x86.cfg)）。完成之后，最后获取我的虚拟机。

如果我还没成功说服你，你可以了解 [nano](http://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/)来代替。但至少要试试。

现在，登入`vm1`，之后键入：

```
vim hello.txt
```

你应该看到：

```
Hello, brave adventurer!
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
~
"hello.txt" [New File]      0,0-1         All
```

有一个笑话说，vim有两种模式 - “反复哔哔”和“破坏一切”。那么，如果你不知道如何使用 vim，这是非常真实的，因为 vim 是模态的文本编辑器。模式是：

+   普通模式：移动光标并执行删除，复制和粘贴等文本操作。
+   插入模式：输入文本。

> 译者注：还有一个命令模式，用于生成真 · 随机字符串（笑）。

这十分使新手头疼，因为他们试图尽可能地避免普通模式。那么这是错误的，所以现在我将给你正确的大纲来使用 vim ：


```
start vim
while editing is not finished, repeat
    navigate to desired position in NORMAL mode
    enter INSERT mode by pressing i
        type text
    exit INSERT mode by pressing <ESCAPE>
when editing is finished, type :wq
```

最重要的是，几乎任何时候都呆在普通模式，短时间内进入插入模式，然后立即退出。以这种方式，vim 只有一种模式，而这种模式是普通模式。

现在让我们试试吧。记住，按`i`进入插入模式，以及`<ESCAPE>` 返回到普通模式。键入以下内容（在每行末尾按`<ENTER>`）：

```
iRoses are red
Linux is scary
<ESCAPE>
```

这是你应该看到的：

```
Roses are red
Linux is scary
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
~
                            4,17          All
```

现在我给你命令列表，在普通模式下移动光标：

+   `h` - 向左移动
+   `j` - 向下移动
+   `k` - 向上移动
+   `l` - 右移
+   `i` - 进入插入模式
+   `o` - 在光标下插入一行并进入插入模式
+   `<ESCAPE>` - 退出插入模式
+   `x` - 删除光标下的符号
+   `dd` - 删除一行
+   `:wq` - 将更改写入文件并退出。是的，没错，这是一个冒号，后面跟着`wq`和`<ENTER>`。
+   `:q!` - 不要对文件进行更改并退出。

那就够了。现在，将光标放在第一行并输入：

```
oViolets are blue<ESCAPE>
```
之后，将光标放在`Linux is scary`那一行，并输入：


```
oBut I'm scary too<ESCAPE>
```

你应该看到：

```
Roses are red
Violets are blue
Linux is scary
But I'm scary too
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
                            4,17          All
```

现在键入`:wq`保存文件，并退出。你应该看到：

```
Violets are blue
Linux is scary
But I'm scary too
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
"hello.txt" 4L, 64C written
user1@vm1:~$
```

好的。你做到它了。你刚刚在 vim 中编辑了文本文件，很好很强大！

## 附加题

+   通过键入键入`vim hello.txt`再次启动 vim，并尝试我给你的一些命令。
+   玩这个游戏，它会让你更熟悉 vim：<http://vim-adventures.com/>
