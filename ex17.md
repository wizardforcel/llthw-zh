# 练习 17：任务调度：`cron`，`at`

> 原文：[Exercise 17. Job schedulers: cron, at](https://archive.fo/TRJCB)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

通常我们需要按计划执行程序。例如，让我们想象一下，你需要在每天的半夜备份你的作品。为了在 Linux 中完成它，有一个叫`cron`的特殊程序。这是一个恶魔，这意味着，当计算机启动后，它就是启动了，并在后台默默等待，在时机到来时为你执行其他程序。`cron`具有多个配置文件，系统级的，或者用户级的。默认情况下，用户没有`crontab`，因为没有为它们安排任何东西。这是`cron`配置文件的位置：

`/etc/crontab` - 系统级`cron`配置文件。
`/var/spool/cron/crontabs/` - 用于存储用户配置文件的目录。

现在我们来谈谈`cron`配置文件的格式。如果你运行`cat /etc/crontab`，你将看到：

```
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.
 
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
 
# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

它的语法足够简单，让我们选取一行：

```
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly
```

然后拆开：

```
17                                  # 每个小时的第 17 个分钟
*                                   # 月中的每一天
*                                   # 年中的每一月
*                                   # 每个星期
root                                # 作为 root 用户
cd /                                # 执行命令 'cd /'
&&                                  # 如果 'cd /' 执行成功，那么
run-parts --report /etc/cron.hourly # 执行命令 'run-parts --report /etc/cron.hourly'
```

现在我总结`cron`的格式：

```
*    *    *    *    *     <用户>            <要执行的命令>
T    T    T    T    T     (仅仅用于系统
|    |    |    |    |      的 crontab)
|    |    |    |    |
|    |    |    |    +----- 星期几 (0 - 6) (0 是星期天, 或者使用名称)
|    |    |    +---------- 月份 (1 - 12)
|    |    +--------------- 天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
```

这是时间格式中，可能的字符的缩略列表：

+   星号（`*`） - 字段中的所有值，例如 分钟的`*`表示每分钟。
+   斜线（`/`） - 定义范围的增量。例如，`30-40/3`意味着在第 30 分钟运行程序，每 3 分钟一次，直到第 40 分钟。
+   百分比（`%`） - 在命令字段中，百分比后的所有数据将作为标准输入发送到命令。现在不要纠结这个。
+   逗号（`,`） - 指定列表，例如分钟字段的`30,40`表示第 30 和 40 `分钟。
+   连字（`-`） - 范围。例如，分钟字段的`30-40`意味着每分钟在30到40分钟之间。
+   `L` - 指定最后一个东西，例如它允许你指定一个月的最后一天。

现在我会给你一些例子：

```
# m      h   dom  mon  dow    user            command
# （每月每天每小时）每分钟
*        *    *    *    *     root            /bin/false
# （每月每天）每小时的第 30~40 分钟中的每分钟
30-40    *    *    *    *     root            /bin/false
# （每月每天）每小时的第 30~40 分钟中的每五分钟
30-40/5  *    *    *    *     root            /bin/false
# （每月每天每小时）每五分钟
*/5      *    *    *    *     user            command to be executed
# 每月的最后一天的每小时每分钟
*        *    L    *    *     root            /bin/false
# 每月的星期天和星期三的每小时每分钟
*        *    *    *    0,3   root          /bin/false
```

好的，但是如何加载`crontab`？这是`cron`命令的列表：

+   `crontab -l` - 打印出当前的`crontab`。
+   `crontab -e` - 为当前用户编辑`crontab`。
+   `crontab -r` - 删除当前用户的`crontab`。
+   `crontab /path/to/file` - 为当前用户加载`crontab`，覆盖过程中的现有项。
+   `crontab > /path/to/file` - 将`crontab`保存到文件中。

这就是如何使用`cron`系统守护进程。但是还有一个可以调度程序执行的选项。它就是`at`工具。它们之间的区别是，`cron`为重复运行任务而设计，而且是很多次，并且`at`为调度一次性的任务而设计。这是相关的命令：


+   `at` - 在指定的时间执行命令。
+   `atq` - 列出待处理的任务。
+   `atrm` - 删除任务。
+   `batch` - 执行命令，然后系统空转。

这个信息转储似乎不够，现在我会给你用于`at`的，时间规范表格，取自 <http://content.hccfl.edu/pollock/unix/atdemo.htm>。在下面的例子中，假定的当前日期和时间是 2001 年 9 月 18 日星期二上午 10:00。

| 示例 | 含义 |
| --- | --- |
| at noon | 2001 年 9 月 18 日星期二下午 12:00 |
| at midnight | 2001 年 9 月 18 日星期二上午 12:00 |
| at teatime | 2001 年 9 月 18 日星期二下午 4:00 |
| at tomorrow | 2001 年 9 月 19 日星期二上午 10:00 |
| at noon tomorrow | 2001 年 9 月 19 日星期二下午 12:00 |
| at next week | 2001 年 9 月 25 日星期二上午 10:00 |
| at next monday | 2001 年 9 月 24 日星期二上午 10:00 |
| at fri | 2001 年 9 月 21 日星期二上午 10:00 |
| at OCT | 2001 年 10 月 18 日星期二上午 10:00 |
| at 9:00 AM | 2001 年 9 月 18 日星期二上午 9:00 |
| at 2:30 PM | 2001 年 9 月 18 日星期二下午 2:30 |
| at 1430 | 2001 年 9 月 18 日星期二下午 2:30 |
| at 2:30 PM tomorrow | 2001 年 9 月 19 日星期二下午 2:30 |
| at 2:30 PM next month | 2001 年 10 月 18 日星期二下午 2:00 |
| at 2:30 PM Fri | 2001 年 9 月 21 日星期二下午 2:30 |
| at 2:30 PM 9/21 | 2001 年 9 月 21 日星期二下午 2:30 |
| at 2:30 PM Sept 21 | 2001 年 9 月 21 日星期二下午 2:30 |
| at 2:30 PM 9/21/2010 | 2001 年 9 月 21 日星期二下午 2:30 |
| at 2:30 PM 9.21.10 | 2001 年 9 月 21 日星期二下午 2:30 |
| at now + 30 minutes | 2001 年 9 月 18 日星期二上午 10:30 |
| at now + 1 hour | 2001 年 9 月 18 日星期二上午 11:00 |
| at now + 2 days | 2001 年 9 月 20 日星期二上午 10:00 |
| at 4 PM + 2 days | 2001 年 9 月 20 日星期二下午 4:00 |
| at now + 3 weeks | 2001 年 10 月 9 日星期二上午 10:00 |
| at now + 4 months | 2002 年 1 月 18 日星期二上午 10:00 |
| at now + 5 years | 2007 年 9 月 18 日星期二上午 10:00 |

现在你将学习如何添加、查看和移除`at`和`crontab`任务。

## 这样做

```
1: echo 'echo Here I am, sitting in ur at, staring at ur date: $(date) | write user1' | at now + 1 minutes
2: atq
```

等待你的消息出现，按下`<ENTER>`并输入更多东西：

```
3: echo '* * * * * echo Here I am, sitting in ur crontab, staring at ur date: $(date) | write user1' > ~/crontab.tmp
4: crontab -l
5: crontab ~/crontab.tmp
6: crontab -l
```

现在等待这个消息出现并移除它。

```
7: crontab -r
8: crontab -l
```

## 你会看到什么

```
user1@vm1:~$ echo 'echo Here I am, sitting in ur at, staring at ur date: $(date) | write user1' | at now + 1 minutes
warning: commands will be executed using /bin/sh
job 13 at Thu Jun 28 14:43:00 2012
user1@vm1:~$ atq
14      Thu Jun 28 14:45:00 2012 a user1
user1@vm1:~$
Message from user1@vm1 on (none) at 14:43 ...
Here I am, sitting in ur at, staring at ur date: Thu Jun 28 14:43:00 MSK 2012
EOF
 
user1@vm1:~$ crontab -l
no crontab for user1
user1@vm1:~$ echo '* * * * * echo Here I am, sitting in ur crontab, staring at ur date: $(date) | write user1' > ~/crontab.tmp
user1@vm1:~$ crontab -l
* * * * * echo Here I am, sitting in ur crontab, staring at ur date: $(date) | write user1
user1@vm1:~$
Message from user1@vm1 on (none) at 14:47 ...
Here I am, sitting in ur crontab, staring at ur date: Thu Jun 28 14:47:01 MSK 2012
EOF
 
user1@vm1:~$ crontab -r
user1@vm1:~$ crontab -l
no crontab for user1
user1@vm1:~$
```

## 解释


1.  让`at`在下一分钟执行命令`echo Here I am, sitting in ur at, staring at ur date: $(date) | write user1`。
1.  打印`at`的任务队列。
1.  将`echo '* * * * * echo Here I am, sitting in ur crontab, staring at ur date: $(date) | write user1 `写入你的主目录中的`crontab.tmp`。
1.  打印你当前的`crontab`，但目前没有东西，所以它只是把这个告诉你。
1.  将`crontab.tmp`的内容加载到你的个人`crontab`文件。
1.  打印你当前的`crontab`。现在有一些东西。
1.  删除你当前的`crontab`。
1.  告诉你，你再次没有了`crontab`。

## 附加题

+   阅读`man crontab`, `man at`, `man write`。
+   让你的系统每 5 分钟告诉你当前时间。
+   让你的系统在每小时的开始告诉你当前时间。
