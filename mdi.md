# Debian 手动安装

> 原文：[Manual Debian installation](https://archive.fo/p1ZHn)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

尽管这个部分很啰嗦，但是不推荐给那些不熟悉 VirtualBox 和 Debian 的人。此外，它是为 Windows 编写的，如果你使用其他系统，我希望，对本指南进行适当的替换相当容易。

首先，下载你需要的东西：

+   下载并安装 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)。
+   下载最新的 [Debian 6 Squeeze CD 映像](http://cdimage.debian.org/debian-cd/6.0.5/amd64/iso-cd/)。你需要第一张 CD，例如`debian-6.0.5-amd64-CD-1.iso`。

对于 Windows 用户，你需要下载 [putty](ttp://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。你需要这个文件：`putty.exe`。它不需要安装，你可以像这样运行它。

## Debian 安装指南

1.  启动 VirtualBox。

    ![](https://archive.fo/p1ZHn/68fc7efcadb72e1e0f33f26176522a14607424ca.png)
    
2.  按下`New`按钮来创建新的虚拟机。在`Name`字段中输入`vm1`，之后选择`Operating System: Linux, Version: Debian (64 bit)`，之后按下`Next >`。

    ![](https://archive.fo/p1ZHn/c083a91d6b41d9216554816fbe0c0df9d6ed1e87.png)
    
3.  从内存至少选择`512 MB`。如果你的机子上安装了足够的 RAM，`1024 GB`也可以。按下`Next >`。

    ![](https://archive.fo/p1ZHn/ae59f8240ff9a5b19138aaec456e14545515b23b.png)
    
4.  这里只需按下`Next >`。

    ![](https://archive.fo/p1ZHn/6857a035f4111c1148ad0429bcbd72dc212c23f0.png)
    
5.  选择`VDI (VirtualBox Disk Image)`，并按下`Next >`。

    ![](https://archive.fo/p1ZHn/05d67f9169ff8f40bc61f4c6000871d37c3a71e4.png)
    
6.  选择`Dynamically allocated`，并按下`Next >`。

    ![](https://archive.fo/p1ZHn/e7caa74cadc60c7b829e0dd85de35018dcfe0b2f.png)
    
7.  在`Location`中输入`vm1`，并按下`Next >`。

    ![](https://archive.fo/p1ZHn/ba205c73a6f0954f9a567423da18fdb36d7b1319.png)
    
8.  点击`Create`。

    ![](https://archive.fo/p1ZHn/5661251d5a943496f199307c52ad84a06c38078f.png)
    
9.  选择`vm1`并点击`Start`。

    ![](https://archive.fo/p1ZHn/e387da8cc25aa78a8432e21ea7664b3b961f60a0.png)
    
0.  点击`Next >`。

    ![](https://archive.fo/p1ZHn/eade1e093fb56f77774d57a6f6c8bff6a37f3ff8.png)
    
1.  点击`folder button`。

    ![](https://archive.fo/p1ZHn/ff276e5704d18417a1b0581dd425e40e8909d1b6.png)
    
2.  浏览并选择你的`Debian 6 Squeeze CD-image`，点击`Open`。

    ![](https://archive.fo/p1ZHn/ef12fa3b92eef1c7001c071acb24d6c6f7622a7a.png)
    
3.  点击`Next >`。

    ![](https://archive.fo/p1ZHn/5f4dcbce772f1d9b6ff84316769a2cec48139503.png)
    
4.  点击`Start`。

    ![](https://archive.fo/p1ZHn/b139ce5a811d31c98ed0cb81573aa9f2b52b5b78.png)
    
5.  关闭烦人的 VirtualBox 窗口。点击 VirtualBox 窗口内部并按下`<ENTER>`。
    ![](https://archive.fo/p1ZHn/6979a652355a3d20019983500f3a22a92d1968a6.png)
    
6.  按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/cd1b9c49b9015a9d37c09e8b91d0a7251aeffa6c.png)
    
7.  按下`<ENTER>`。

    > 译者注：这里你可以选“中文（简体）”。
    
    ![](https://archive.fo/p1ZHn/9d4256a54e6e34d36aed8373fe074f3e6439c6c2.png)
    
8.  按下`<ENTER>`。

    > 译者注：这里你可以选“HongKong”。
    
    ![](https://archive.fo/p1ZHn/e1fd69a847e9321383f87dbc9977cb8b9a0fb4a3.png)
    
9.  按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/65b272c4e228ddcf365f61d44d5256a596935455.png)
    
0.  输入`vm1`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/4801b99ce3260d1f90fee587763f2594bad2c2e7.png)
    
1.  输入`site`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/5eb0e36cdf4e3ddaf10db2fb4fb2b0834c7c2bfa.png)
    
2.  输入`123qwe`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/34ffc67b2618df734bcbd6f5306c2860254ce60f.png)
    
3.  输入`123qwe`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/57dbc33555e2e2ae61517be0aa83cfbb1f9bfc34.png)
    
4.  输入`user1`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/e0f9977d67c15b3b02e1229c7f0b454204161340.png)
    
5.  按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/1d2d158cb7bc6dae359fb1e02214136e4a6e3e2d.png)
    
6.  输入`123qwe`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/7e8415ff24fed2c2b155622889e226e92a3e51ea.png)
    
7.  输入`123qwe`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/03c870e0b698e28f3654f94c751547a25cbc04e1.png)
    
8.  如果你不知道这里做什么，只需按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/04707609537bfd69cc50fc33867a15a7320e832c.png)
    
9.  选择`Guided partitioning`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/4af1a582de294fba7b8bd3aa4aca2b6d7534268f.png)
    
0.  选择`Guided – use entire disk`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/d7bfde3632cf431672e448ed0f4e59da792a8401.png)
    
1.  再次按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/a3bb62cd87b0e9359e08b3a8a2aa694258959b35.png)
    
2.  选择`eparate /home, /usr, /var, and /tmp partitions`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/fc92f1246ab697a70fb40982988226f412853883.png)
    
3.  选择`Finish partitioning and write changes to disk`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/a2b293906dc62fde7064426b59ad880b362d0f6a.png)
    
4.  选择`<Yes>`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/ef0d5da56d4aad96dc711f0a527c0183e3a27269.png)
    
5.  选择`<No>`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/e9cfc3142b4d63f1d217da3d85dc1140ab09d845.png)
    
6.  选择`<Yes>`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/65f4aa627d6a40e7071cb2ac1ae68d04d09c8824.png)
    
7.  选择`ftp.egr.msu.edu`并按下`<ENTER>`。如果出现错误，选择其它的东西。

    ![](https://archive.fo/p1ZHn/546cf834885a1e587938ea7cf5dc6e07d246e65b.png)

8.  再次按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/b2685c27c688d34ff6e504ffe186dd80c99c084d.png)
    
9.  选择`<No>`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/fc6ebaa84a254c1ad045d64526b1a2326a37b237.png)
    
0.  使用`<SPACE>`选择`SSH server and Standard system utilities`，并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/713e8e08d2e059d156824e33da39e48abd9de65b.png)
    
1.  选择`<Yes>`并按下`<ENTER>`。

    ![](https://archive.fo/p1ZHn/aeb99f9cf80d45d2015fa9b1f318a9b19cec2db4.png)
    
2.  选择`<Continue>`并按下`<ENTER>`。你新安装的 Debian 会重启。

    ![](https://archive.fo/p1ZHn/c912182ce24a01767fa1ffde3b08b176a3fa35f3.png)
    
3.  点击`Devices`并选择`Network adapters`。

    ![](https://archive.fo/p1ZHn/37877d8aacfd1da161dc8bdb06a2c2883dae22ca.png)
    
4.  点击`Port Forwarding`。

    ![](https://archive.fo/p1ZHn/bbcbf2ef6b2cfd5786201318978b5f9bd5bb32fa.png)
    
5.  点击`Plus`按钮。

    ![](https://archive.fo/p1ZHn/befa3bdfeebb864af78501c3d4c395293c2bc030.png)
    
6.  在`Host Port`中输入`22`，`Guest Port`中输入`22`，点击`OK`。

    ![](https://archive.fo/p1ZHn/89218bd92e2e590456bae0a7ac8d1c2168213318.png)
    
7.  再次点击`OK`。

    ![](https://archive.fo/p1ZHn/df2515bcd3fa375f8cf17f4b86a85f08d5cf47ef.png)
    
8.  让你的 Debian 系统运行一会儿。

    ![](https://archive.fo/p1ZHn/8648c8f7806c19ac2b3500adeb4bcef0f1dcd313.png)
    
9.  启动`putty`，在`Host Name`中输入`localhost`（或 IP 地址），在`Port`字段中输入`22`。点击`Open`。

    ![](https://archive.fo/p1ZHn/b4c54192c06c066444897554eb37f8693ca17578.png)
    
0.  点击`Yes`。

    ![](https://archive.fo/p1ZHn/895745a41f1b557b6befcb3a73450747e815f8c3.png)
    
1.  输入`user1`，点击`<ENTER>`。输入`123qwe`，并再输入一次，来真正享受你的作品吧。

    ![](https://archive.fo/p1ZHn/15936cef6402ab07e7982d18d4302b869b8136ab.png)
    
你以为这就完了吗？现在将这些输入`putty`，通过按下`<ENTER>`结束每个命令：

```
1: su
2: 123qwe
3: sed -i '/^deb cdrom.*$/d' /etc/apt/sources.list
4: aptitude update
5: aptitude install vim sudo parted
```

询问时，输入`y`并按下`<ENTER>`。

```
6: update-alternatives --config editor
```

询问时，输入`3`并按下`<ENTER>`。

```
7: sed -i 's/%sudo ALL=(ALL) ALL/%sudo ALL=(ALL) NOPASSWD:ALL/' /etc/sudoers
8: usermod user1 -G sudo
```

关闭`putty`，再次打开它，并作为`user1`登入`vm1`，输入这个：

```
9: sudo -s
```

如果你得到了`root@vm1:/home/user1#`，那么一切正常，开瓶啤酒奖励自己吧。

## 你会看到什么

```
user1@vm1:~$ su
Password:
root@vm1:/home/user1# sed -i '/^deb cdrom.*$/d' /etc/apt/sources.list
root@vm1:/home/user1# aptitude update
Hit http://security.debian.org squeeze/updates Release.gpg
Ign http://security.debian.org/ squeeze/updates/main Translation-en
Ign http://security.debian.org/ squeeze/updates/main Translation-en_US
Hit http://security.debian.org squeeze/updates Release
Hit http://ftp.egr.msu.edu squeeze Release.gpg
Hit http://security.debian.org squeeze/updates/main Sources
Hit http://security.debian.org squeeze/updates/main amd64 Packages
Ign http://ftp.egr.msu.edu/debian/ squeeze/main Translation-en
Ign http://ftp.egr.msu.edu/debian/ squeeze/main Translation-en_US
Hit http://ftp.egr.msu.edu squeeze-updates Release.gpg
Ign http://ftp.egr.msu.edu/debian/ squeeze-updates/main Translation-en
Ign http://ftp.egr.msu.edu/debian/ squeeze-updates/main Translation-en_US
Hit http://ftp.egr.msu.edu squeeze Release
Hit http://ftp.egr.msu.edu squeeze-updates Release
Hit http://ftp.egr.msu.edu squeeze/main Sources
Hit http://ftp.egr.msu.edu squeeze/main amd64 Packages
Get:1 http://ftp.egr.msu.edu squeeze-updates/main Sources/DiffIndex [2,161 B]
Hit http://ftp.egr.msu.edu squeeze-updates/main amd64 Packages/DiffIndex
Hit http://ftp.egr.msu.edu squeeze-updates/main amd64 Packages
Fetched 2,161 B in 3s (603 B/s)
root@vm1:/home/user1# aptitude install vim sudo parted
The following NEW packages will be installed:
  libparted0debian1{a} parted sudo vim vim-runtime{a}
0 packages upgraded, 5 newly installed, 0 to remove and 0 not upgraded.
Need to get 8,231 kB of archives. After unpacking 29.8 MB will be used.
Do you want to continue? [Y/n/?] y
Get:1 http://security.debian.org/ squeeze/updates/main sudo amd64 1.7.4p4-2.squeeze.3 [610 kB]
Get:2 http://ftp.egr.msu.edu/debian/ squeeze/main libparted0debian1 amd64 2.3-5 [341 kB]
Get:3 http://ftp.egr.msu.edu/debian/ squeeze/main parted amd64 2.3-5 [156 kB]
Get:4 http://ftp.egr.msu.edu/debian/ squeeze/main vim-runtime all 2:7.2.445+hg~cb94c42c0e1a-1 [6,207 kB]
Get:5 http://ftp.egr.msu.edu/debian/ squeeze/main vim amd64 2:7.2.445+hg~cb94c42c0e1a-1 [915 kB]
Fetched 8,231 kB in 1min 18s (105 kB/s)
Selecting previously deselected package libparted0debian1.
(Reading database ... 34745 files and directories currently installed.)
Unpacking libparted0debian1 (from .../libparted0debian1_2.3-5_amd64.deb) ...
Selecting previously deselected package parted.
Unpacking parted (from .../parted_2.3-5_amd64.deb) ...
Selecting previously deselected package sudo.
Unpacking sudo (from .../sudo_1.7.4p4-2.squeeze.3_amd64.deb) ...
Selecting previously deselected package vim-runtime.
Unpacking vim-runtime (from .../vim-runtime_2%3a7.2.445+hg~cb94c42c0e1a-1_all.deb) ...
Adding 'diversion of /usr/share/vim/vim72/doc/help.txt to /usr/share/vim/vim72/doc/help.txt.vim-tiny by vim-runtime'
Adding 'diversion of /usr/share/vim/vim72/doc/tags to /usr/share/vim/vim72/doc/tags.vim-tiny by vim-runtime'
Selecting previously deselected package vim.
Unpacking vim (from .../vim_2%3a7.2.445+hg~cb94c42c0e1a-1_amd64.deb) ...
Processing triggers for man-db ...
Setting up libparted0debian1 (2.3-5) ...
Setting up parted (2.3-5) ...
Setting up sudo (1.7.4p4-2.squeeze.3) ...
No /etc/sudoers found... creating one for you.
Setting up vim-runtime (2:7.2.445+hg~cb94c42c0e1a-1) ...
Processing /usr/share/vim/addons/doc
Setting up vim (2:7.2.445+hg~cb94c42c0e1a-1) ...
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vim (vim) in auto mode.
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vimdiff (vimdiff) in auto mode.
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rvim (rvim) in auto mode.
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rview (rview) in auto mode.
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vi (vi) in auto mode.
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/view (view) in auto mode.
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/ex (ex) in auto mode.
root@vm1:/home/user1# update-alternatives --config editor
There are 3 choices for the alternative editor (providing /usr/bin/editor).
 
  Selection    Path                Priority   Status
------------------------------------------------------------
* 0            /bin/nano            40        auto mode
  1            /bin/nano            40        manual mode
  2            /usr/bin/vim.basic   30        manual mode
  3            /usr/bin/vim.tiny    10        manual mode
 
Press enter to keep the current choice[*], or type selection number: 3
update-alternatives: using /usr/bin/vim.tiny to provide /usr/bin/editor (editor) in manual mode.
root@vm1:/home/user1# sed -i 's/%sudo ALL=(ALL) ALL/%sudo ALL=(ALL) NOPASSWD:ALL/' /etc/sudoers
root@vm1:/home/user1# usermod user1 -G sudo
root@vm1:/home/user1#
```

## 解释

1.  使你成为超级用户或`root`用户。
1.  你在安装过程中输入的`root`密码。
1.  修改仓库文件，因此 Debian 将尝试仅仅从互联网安装新软件。
1.  更新可用软件数据库。
1.  安装`vim`，`sudo`和`parted`包。
1.  将默认系统文本编辑器更改为`vim`。
1.  允许你通过修改`sudo`配置文件成为超级用户，而不输入密码。
1.  将你添加到`sudo`组，以便你可以通过`sudo`成为`root`。
1.  检查你是否能够成为`root`。 
