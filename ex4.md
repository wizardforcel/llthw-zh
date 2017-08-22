# 练习 4：Bash：处理文件，`pwd`，`ls`，`cp`，`mv`，`rm`，`touch`

> 原文：[Exercise 4. Bash: working with files, pwd, ls, cp, mv, rm, touch](https://archive.fo/xb8YB)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

在 Linux 中，一切都是文件。但是什么是文件？现在完全可以说，它是一个包含一些信息的对象。它通常[定义](http://en.wikipedia.org/wiki/Computer_file)如下：

> 计算机文件是用于存储信息的，任意的信息块或资源。它可用于计算机程序，并且通常基于某种持久的存储器。文件是持久的，因为它在当前程序完成后，仍然可用于其它程序。计算机文件可以认为是纸质文档的现代对应物，它们通常保存于办公室和图书馆的文件中，这是该术语的来源。

但这个定义太笼统了，所以让我们更具体一些。`man stat`告诉我们，文件是一个对象，它包含以下属性：

```c
struct stat {
   dev_t     st_dev;     /* ID of device containing file */
   ino_t     st_ino;     /* inode number */
   mode_t    st_mode;    /* protection */
   nlink_t   st_nlink;   /* number of hard links */
   uid_t     st_uid;     /* user ID of owner */
   gid_t     st_gid;     /* group ID of owner */
   dev_t     st_rdev;    /* device ID (if special file) */
   off_t     st_size;    /* total size, in bytes */
   blksize_t st_blksize; /* blocksize for file system I/O */
   blkcnt_t  st_blocks;  /* number of 512B blocks allocated */
   time_t    st_atime;   /* time of last access */
   time_t    st_mtime;   /* time of last modification */
   time_t    st_ctime;   /* time of last status change */
};
```

不要害怕，只要记住以下属性：

+   大小，这正好是它所说的。
+   上次访问的时间，当你查看文件时更新。
+   上次修改的时间，当你更改文件时更新。

现在你将学习如何打印当前目录，目录中的文件，复制和移动文件。

## 这样做

```
 1: pwd
 2: ls
 3: ls -a
 4: ls -al
 5: ls -altr
 6: cp -v .bash_history{,1}
 7: cp -v .bash_history1 .bash_history2
 8: mv -v .bash_history1 .bash_history2
 9: rm -v .bash_history2
10: touch .bashrc
11: ls -al
12: ls .*
```

## 你应该看到什么

```
Hello, user1!
user1@vm1:~$ pwd
/home/user1
user1@vm1:~$ ls
user1@vm1:~$ ls -a
.  ..  .bash_history  .bash_history1  .bash_logout  .bashrc  .lesshst  .profile  .profile.bak  .profile.bak1
user1@vm1:~$ ls -al
total 40
drwxr-xr-x 2 user1 user1 4096 Jun  7 13:30 .
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw------- 1 user1 user1  853 Jun  7 15:03 .bash_history
-rw------- 1 user1 user1  308 Jun  7 13:14 .bash_history1
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
-rw-r--r-- 1 user1 user1 3184 Jun  6 21:48 .bashrc
-rw------- 1 user1 user1   45 Jun  7 13:31 .lesshst
-rw-r--r-- 1 user1 user1  697 Jun  7 12:25 .profile
-rw-r--r-- 1 user1 user1  741 Jun  7 12:19 .profile.bak
-rw-r--r-- 1 user1 user1  741 Jun  7 13:12 .profile.bak1
user1@vm1:~$ ls -altr
total 40
-rw-r--r-- 1 user1 user1 3184 Jun  6 21:48 .bashrc
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw-r--r-- 1 user1 user1  741 Jun  7 12:19 .profile.bak
-rw-r--r-- 1 user1 user1  697 Jun  7 12:25 .profile
-rw-r--r-- 1 user1 user1  741 Jun  7 13:12 .profile.bak1
-rw------- 1 user1 user1  308 Jun  7 13:14 .bash_history1
drwxr-xr-x 2 user1 user1 4096 Jun  7 13:30 .
-rw------- 1 user1 user1   45 Jun  7 13:31 .lesshst
-rw------- 1 user1 user1  853 Jun  7 15:03 .bash_history
user1@vm1:~$ cp -v .bash_history{,1}
`.bash_history' -> `.bash_history1'
user1@vm1:~$ cp -v .bash_history1 .bash_history2
`.bash_history1' -> `.bash_history2'
user1@vm1:~$ mv -v .bash_history1 .bash_history2
`.bash_history1' -> `.bash_history2'
user1@vm1:~$ rm -v .bash_history2
removed `.bash_history2'
user1@vm1:~$ touch .bashrc
user1@vm1:~$ ls -al
total 36
drwxr-xr-x 2 user1 user1 4096 Jun 14 12:23 .
drwxr-xr-x 3 root  root  4096 Jun  6 21:49 ..
-rw------- 1 user1 user1  853 Jun  7 15:03 .bash_history
-rw-r--r-- 1 user1 user1  220 Jun  6 21:48 .bash_logout
-rw-r--r-- 1 user1 user1 3184 Jun 14 12:24 .bashrc
-rw------- 1 user1 user1   45 Jun  7 13:31 .lesshst
-rw-r--r-- 1 user1 user1  697 Jun  7 12:25 .profile
-rw-r--r-- 1 user1 user1  741 Jun  7 12:19 .profile.bak
-rw-r--r-- 1 user1 user1  741 Jun  7 13:12 .profile.bak1
user1@vm1:~$
user1@vm1:~$ ls .*
.bash_history  .bash_logout  .bashrc  .lesshst  .profile  .profile.bak  .profile.bak1
 
.:
ls.out
 
..:
user1
```

## 解释

1.  打印你当前的工作目录，这是你的主目录。请注意它为何不同于`user1@vm1:~`中的`~`，这也表示，你在你的`home`目录中。这是因为`~`是你的主目录的缩写。

2.  这里没有任何东西，因为你的主目录中只有隐藏的文件。记住，隐藏的文件是以点开头的名称。

3.  打印主目录中的所有文件，因为`-a`参数让`ls`显示所有文件，包括隐藏文件。

4.  以长格式打印主目录中的所有文件：权限，所有者，组，大小，时间戳（通常是修改时间）和文件名。

5.  注意文件如何按日期安排，最新的文件是最后一个。`-t`告诉`ls`按时间排序，`-r`告诉`ls`反转排序。

6.  将`.bash_history`复制到`.bash_history1`。这似乎对你来说很神秘，但解释真的很简单。Bash 有一个称为花括号扩展的功能，它有一组规则，定义了如何 处理像`{1,2,3}`之类的结构。在我们的例子中，`.bash_history{,1}` 扩展为两个参数，即`.bash_history`和`.bash_history1`。Bash 仅仅接受花括号前的一个参数，在我们的例子中是`.bash_history`，并向参数添加花括号里的所有东西，以逗号分隔，并以此作为参数。第一个添加只是变成`.bash_history`，因为花括号中的第一个参数是空的，没有第一个参数。接下来，Bash 添加了`1`，因为这是第二个参数，就是这样。扩展后传递给`cp`的 结果参数为`-v .bash_history1 .bash_history2`。

7.  这可能对你来说很明显。将最近创建的`.bash_history1`复制到`.bash_history2`。

8.  向`.bash_history1`移动到`.bash_history2`。请注意，它会覆盖目标文件而不询问，所以不再有`.bash_history2`，没有了！

9.  将`.bashrc`时间戳更新为当前日期和时间。这意味着`.bashrc`的所有时间属性，`st_atime`，`st_mtime`和`st_ctime`都设置为当前日期和时间。你可以通过输入`stat .bashrc`来确定它。

0.  删除`.bash_history2`。这里没有警告，请小心。另外，总是用`-v`选项。

1.  再次以长格式打印所有文件。请注意`.bashrc`的时间戳更改。

2.  在你的主目录中以短格式打印文件。请注意，你不仅可以列出`/home/user1`目录，还可以列出`/home`目录本身。不要和任何命令一起使用这个结构，特别是 `rm`，永远不要！或许，你会意外地通过删除错误的文件或更改权限，来使系统崩溃。

## 附加题

玩转 bash 花括号扩展。从`echo test{1,2,foo,bar}`开始。尝试使用花括号键入几个单独的参数。

使用 Google 搜索 bash 花括号扩展，从搜索结果中打开“Bash 参考手册”，并阅读相应的部分。

尝试弄清楚`ls .*`如何和为什么工作。

对自己说10次：“我会一直使用 verbose 选项。verbose 选项通常用作`-v`参数”。

对自己说10次：“我会永远用`ls`检查任何危险的命令”。
