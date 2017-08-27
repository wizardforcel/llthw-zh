# 练习 27：安全 Shell，`ssh`，`sshd`，`scp`

> 原文：[Exercise 27. Networking: secure shell, ssh, sshd, scp](https://archive.fo/vzDDW)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

你可能已经知道，[SSH](https://en.wikipedia.org/wiki/Secure_Shell) 是一种网络协议，允许你通过网络登录到`vm1`。让我们详细研究一下。

> 安全 Shell（SSH）是一种网络协议，用于安全数据通信，远程 Shell 服务或命令执行，以及其它两个联网计算机之间的网络服务，它们通过不安全网络上的安全通道连接：服务器和客户端（运行 SSH 服务器和 SSH 客户端程序）。协议规范区分了两个主要版本，被称为 SSH-1 和 SSH-2。

> 协议最著名的应用是，访问类 Unix 操作系统上的 shell 帐户。它为替代 Telnet 和其他不安全的远程 shell 协议而设计，如 Berkeley rsh 和 rexec 协议，它们以明文形式发送信息，特别是密码，使得它们易于使用封包分析来拦截和暴露。SSH 使用的加密 旨在通过不安全的网络（如互联网）提供数据的机密性和完整性。

重要的 SSH 程序，概念和配置文件：

+   [OpenSSH](https://en.wikipedia.org/wiki/OpenSSH) - 开源的 ssh 程序实现。
+   `ssh` - 允许你连接到 SSH 服务器的客户端程序。Putty 就是这样的客户端程序。
+   `sshd` - 服务器程序，允许你使用`ssh`连接到它。
+   `/etc/ssh/ssh_config` - 默认的客户端程序配置文件。
+   `/etc/ssh/sshd_config` - 默认服务器程序配置文件。
+   [公钥密码系统](https://en.wikipedia.org/wiki/Public-key_cryptography) - 一种需要两个单独密钥的加密系统，其中一个密钥是私钥，其中一个密钥是公钥。虽然不同，密钥对的两个部分在数学上是相关的。一旦密钥锁定或加密了明文，另一个密钥解锁或解密密文。两个密钥都不能执行这两个功能。其中一个密钥是公开发布的，另一个密钥是保密的。
+   SSH 密钥 - SSH 使用公钥密码系统来认证远程计算机，并允许它对用户进行认证（如有必要）。任何人都可以生成一对匹配的不同密钥（公钥和私钥）。公钥放置在所有计算机上，它们允许访问匹配的私钥的所有者（所有者使私钥保密）。虽然认证基于私钥，但认证期间密钥本身不会通过网络传输。
+   `/etc/ssh/moduli` - 质数及其生成器，由`sshd(8)`用于 Diffie-Hellman Group Exchange 密钥交换方法中。
+   `/etc/ssh/ssh_host_dsa_key`, `/etc/ssh/ssh_host_rsa_key` - 主机 RSA 和 DSA 私钥。
+   `/etc/ssh/ssh_host_dsa_key.pub`, `/etc/ssh/ssh_host_rsa_key.pub` - 主机 RSA 和 DSA 公钥。

SSH 协议非常重要，因此被广泛使用，并且具有如此多的功能，你必须了解它的一些工作原理。这是它的一些用途：

+   `scp` - 通过 SSH 传输文件。
+   `sftp` - 类似 ftp 的协议，用于管理远程文件。
+   `sshfs` - SSH 上的远程文件系统。
+   SSH 隧道 - 一种通过安全连接，传输几乎任何数据的方法。这是非常重要的，因为它可以用于构建受保护系统的基础，以及许多其他用途。

为了了解这个协议，让我们看看，在 SSH 会话中会发生了什么。为此，我们将开始研究`vm1`到`vm1`的连接的带注解的输出（是的，这是可以做到的，也是完全有效的）。概述：

```
你
    输入 SSH VM1 
    控制权现在传递给 SSH 客户端
SSH 客户端
    进入明文阶段
        读取配置
        与 SSH 服务器进行协议协商
    进入 SSH 传输阶段
        与 SSH 服务器进行协商
            数据加密密码 
            数据完整性算法
            数据压缩算法
            使用 Diffie-Hellman 算法启动密钥交换
            所得共享密钥用于建立安全连接
    进入 SSH-userauth 阶段
    要求你输入密码
    控制权现在传递给你
你
    输入密码
    控制权现在传递给 SSH 客户端
SSH 客户端
    在 SSH 服务器对你进行认证
    进入 SSH 连接阶段
    为你分配伪终端
    为你启动 shell
    控制权现在传递给你
你
    在 vm1 上做一些（没）有用的事情 
    关闭 shell
    控制全现在传递给 SSH 客户端
SSH 客户端
    关闭伪终端
    关闭连接
```

现在阅读这个：

+   [SSH 协议揭秘](https://www.linuxjournal.com/article/9566)
+   <http://www.cs.ust.hk/faculty/cding/COMP581/SLIDES/slide24.pdf>

并研究 SSH 会话的真实输出：

```
user1@vm1:~$ ssh -vv vm1
 
Protocol version selection, plaintext
-------------------------------------
 
OpenSSH_5.5p1 Debian-6+squeeze2, OpenSSL 0.9.8o 01 Jun 2010
# Speaks for itself, I will mark such entries with -- below
debug1: Reading configuration data /etc/ssh/ssh_config
# Applying default options for all hosts. Additional options for each host may be
# specified in the configuration file
debug1: Applying options for *
debug2: ssh_connect: needpriv 0
debug1: Connecting to vm1 [127.0.1.1] port 22.
debug1: Connection established.
debug1: identity file /home/user1/.ssh/id_rsa type -1      # no such files
debug1: identity file /home/user1/.ssh/id_rsa-cert type -1
debug1: identity file /home/user1/.ssh/id_dsa type -1
debug1: identity file /home/user1/.ssh/id_dsa-cert type -1
debug1: Remote protocol version 2.0, remote software version OpenSSH_5.5p1 Debian-6+squeeze2
debug1: match: OpenSSH_5.5p1 Debian-6+squeeze2 pat OpenSSH*
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_5.5p1 Debian-6+squeeze2
debug2: fd 3 setting O_NONBLOCK
 
SSH-transport, binary packet protocol
-------------------------------------
 
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
# Key exchange algorithms
debug2: kex_parse_kexinit: diffie-hellman-group-exchange-sha256,diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
# SSH host key types
debug2: kex_parse_kexinit: ssh-rsa-cert-v00@openssh.com,ssh-dss-cert-v00@openssh.com,ssh-rsa,ssh-dss
# Data encryption ciphers
debug2: kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
debug2: kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
# Data integrity algorithms
debug2: kex_parse_kexinit: hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
debug2: kex_parse_kexinit: hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
# Data compression algorithms
debug2: kex_parse_kexinit: none,zlib@openssh.com,zlib
debug2: kex_parse_kexinit: none,zlib@openssh.com,zlib
debug2: kex_parse_kexinit:
debug2: kex_parse_kexinit:
debug2: kex_parse_kexinit: first_kex_follows
debug2: kex_parse_kexinit: reserved 0
# Messages back from server
debug2: kex_parse_kexinit: diffie-hellman-group-exchange-sha256,diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
debug2: kex_parse_kexinit: ssh-rsa,ssh-dss
debug2: kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
debug2: kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
debug2: kex_parse_kexinit: hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
debug2: kex_parse_kexinit: hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
debug2: kex_parse_kexinit: none,zlib@openssh.com
debug2: kex_parse_kexinit: none,zlib@openssh.com
debug2: kex_parse_kexinit:
debug2: kex_parse_kexinit:
debug2: kex_parse_kexinit: first_kex_follows 0
debug2: kex_parse_kexinit: reserved 0
# Message authentication code setup
debug2: mac_setup: found hmac-md5
debug1: kex: server->client aes128-ctr hmac-md5 none
debug2: mac_setup: found hmac-md5
debug1: kex: client->server aes128-ctr hmac-md5 none
# Key exchange
debug1: SSH2_MSG_KEX_DH_GEX_REQUEST(1024<1024<8192) sent
debug1: SSH2_MSG_KEX_DH_GEX_REQUEST(1024<1024<8192) sent
debug1: expecting SSH2_MSG_KEX_DH_GEX_GROUP
debug2: dh_gen_key: priv key bits set: 135/256
debug2: bits set: 498/1024
debug1: SSH2_MSG_KEX_DH_GEX_INIT sent
debug1: expecting SSH2_MSG_KEX_DH_GEX_REPLY
# Server authentication. vm1 host key is not known because it is our first connection
debug2: no key of type 0 for host vm1
debug2: no key of type 2 for host vm1
# Confirmation of host key acceptance
The authenticity of host 'vm1 '(127.0.1.1)' can't be established.
RSA key fingerprint is b6:06:92:5e:04:49:d9:e8:57:90:61:1b:16:87:bb:09.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'vm1' (RSA) to the list of known hosts.
# Key is added to /home/user1/.ssh/known_hosts and checked
debug2: bits set: 499/1024
debug1: ssh_rsa_verify: signature correct
# Based on shared master key, data encryption key and data integrity key are derived
debug2: kex_derive_keys
debug2: set_newkeys: mode 1
# Information about this is sent to server
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug2: set_newkeys: mode 0
debug1: SSH2_MSG_NEWKEYS received
# IP roaming not enabled? Not sure about this.
debug1: Roaming not allowed by server
 
SSH-userauth
------------
 
debug1: SSH2_MSG_SERVICE_REQUEST sent
debug2: service_accept: ssh-userauth
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug2: key: /home/user1/.ssh/id_rsa ((nil))
debug2: key: /home/user1/.ssh/id_dsa ((nil))
debug1: Authentications that can continue: publickey,password
debug1: Next authentication method: publickey
debug1: Trying private key: /home/user1/.ssh/id_rsa
debug1: Trying private key: /home/user1/.ssh/id_dsa
debug2: we did not send a packet, disable method
debug1: Next authentication method: password
user1@vm1''s password:
debug2: we sent a password packet, wait for reply
debug1: Authentication succeeded (password).
 
SSH-connection
--------------
 
debug1: channel 0: new [client-session]
debug2: channel 0: send open
# Disable SSH mutiplexing.
# More info: http://www.linuxjournal.com/content/speed-multiple-ssh-connections-same-server
debug1: Requesting no-more-sessions@openssh.com
debug1: Entering interactive session.
debug2: callback start
debug2: client_session2_setup: id 0
debug2: channel 0: request pty-req confirm 1
# Sending environment variables
debug1: Sending environment.
debug1: Sending env LANG = en_US.UTF-8
debug2: channel 0: request env confirm 0
debug2: channel 0: request shell confirm 1
# Set TCP_NODELAY flag: http://en.wikipedia.org/wiki/Nagle%27s_algorithm
debug2: fd 3 setting TCP_NODELAY
debug2: callback done
# Connection opened
debug2: channel 0: open confirm rwindow 0 rmax 32768
debug2: channel_input_status_confirm: type 99 id 0
# Pseudo terminal allocation
debug2: PTY allocation request accepted on channel 0
debug2: channel 0: rcvd adjust 2097152
debug2: channel_input_status_confirm: type 99 id 0
# Shell is started
debug2: shell request accepted on channel 0
# Loggin in is completed
Linux vm1 2.6.32-5-amd64 #1 SMP Sun May 6 04:00:17 UTC 2012 x86_64
 
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
 
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have mail.
Last login: Thu Jul 19 05:14:40 2012 from 10.0.2.2
user1@vm1:~$ debug2: client_check_window_change: changed
debug2: channel 0: request window-change confirm 0
user1@vm1:~$ debug2: client_check_window_change: changed
debug2: channel 0: request window-change confirm 0
user1@vm1:~$ logout
 
Ending ssh connection
---------------------
 
debug2: channel 0: rcvd eof   # end of file
debug2: channel 0: output open -> drain
debug2: channel 0: obuf empty
debug2: channel 0: close_write
debug2: channel 0: output drain -> closed
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
# signalling that channels are half-closed for writing, through a channel protocol extension
# notification "eow@openssh.com" http://www.openssh.com/txt/release-5.1
debug1: client_input_channel_req: channel 0 rtype eow@openssh.com reply 0
debug2: channel 0: rcvd eow
# Ending connection
debug2: channel 0: close_read
debug2: channel 0: input open -> closed
debug2: channel 0: rcvd close
debug2: channel 0: almost dead
debug2: channel 0: gc: notify user
debug2: channel 0: gc: user detached
debug2: channel 0: send close
debug2: channel 0: is dead
debug2: channel 0: garbage collecting
debug1: channel 0: free: client-session, nchannels 1
Connection to vm1 closed.
Transferred: sent 1928, received 2632 bytes, in 93.2 seconds
Bytes per second: sent 20.7, received 28.2
debug1: Exit status 0
user1@vm1:~$
```

现在，你将学习如何在调试模式下启动`sshd`，使用`scp`建立公钥认证和复制文件。

## 这样做

```
 1: mkdir -v ssh_test
 2: cd ssh_test
 3: cp -v /etc/ssh/sshd_config .
 4: sed -i'.bak' 's/^Port 22$/Port 1024/' sshd_config
 5: sed -i 's/^HostKey \/etc\/ssh\/ssh_host_rsa_key$/Hostkey \/home\/user1\/ssh_test\/ssh_host_rsa_key/' sshd_config
 6: sed -i 's/^HostKey \/etc\/ssh\/ssh_host_dsa_key$/Hostkey \/home\/user1\/ssh_test\/ssh_host_dsa_key/' sshd_config
 7: diff sshd_config.bak sshd_config
 8: ssh-keygen -b 4096 -t rsa -N '' -v -h -f ssh_host_rsa_key
 9: ssh-keygen -b 1024 -t dsa -N '' -v -h -f ssh_host_dsa_key
10: ssh-keygen -b 4096 -t rsa -N '' -v  -f ~/.ssh/id_rsa
11: cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
12: /usr/sbin/sshd -Ddf sshd_config > sshd.out 2>&1 &
13: ssh-keyscan -H vm1 127.0.0.1 >> ~/.ssh/known_hosts
14: /usr/sbin/sshd -Ddf sshd_config >> sshd.out 2>&1 &
15: ssh vm1 -v -p 1024 2>ssh.out
16: ps au --forest
17: logout
18: /usr/sbin/sshd -Ddf sshd_config >> sshd.out 2>&1 &
19: scp -v -P 1024 vm1:.bashrc . 2>scp.out
```

## 你会看到什么

```
user1@vm1:~$ mkdir -v ssh_test
mkdir: created directory 'ssh_test'
user1@vm1:~$ cd ssh_test
user1@vm1:~/ssh_test$ cp -v /etc/ssh/sshd_config .
'/etc/ssh/sshd_config' -> './sshd_config'
user1@vm1:~/ssh_test$ sed -i'.bak' 's/^Port 22$/Port 1024/' sshd_config
user1@vm1:~/ssh_test$ sed -i 's/^HostKey \/etc\/ssh\/ssh_host_rsa_key$/Hostkey \/home\/user1\/ssh_test\/ssh_host_rsa_key/' sshd_config
user1@vm1:~/ssh_test$ sed -i 's/^HostKey \/etc\/ssh\/ssh_host_dsa_key$/Hostkey \/home\/user1\/ssh_test\/ssh_host_dsa_key/' sshd_config
user1@vm1:~/ssh_test$ diff sshd_config.bak sshd_config
5c5
< Port 22
---
> Port 1024
11,12c11,12
< HostKey /etc/ssh/ssh_host_rsa_key
< HostKey /etc/ssh/ssh_host_dsa_key
---
> Hostkey /home/user1/ssh_test/ssh_host_rsa_key
> Hostkey /home/user1/ssh_test/ssh_host_dsa_key
user1@vm1:~/ssh_test$ ssh-keygen -b 4096 -t rsa -N '' -v -h -f ssh_host_rsa_key
Generating public/private rsa key pair.
Your identification has been saved in ssh_host_rsa_key.
Your public key has been saved in ssh_host_rsa_key.pub.
The key fingerprint is:
8c:0a:8d:ae:c7:34:e6:29:9c:c2:14:29:b8:d9:1d:34 user1@vm1
'The key's randomart image is:
+--[ RSA 4096]----+
|                 |
|    E            |
|. .. .           |
|oo o.  o         |
|.++.... S        |
|oo=...           |
|+=oo.            |
|o==              |
|oo               |
+-----------------+
user1@vm1:~/ssh_test$ ssh-keygen -b 1024 -t dsa -N '' -v -h -f ssh_host_dsa_key
Generating public/private dsa key pair.
Your identification has been saved in ssh_host_dsa_key.
Your public key has been saved in ssh_host_dsa_key.pub.
The key fingerprint is:
cd:6b:2a:a2:ba:80:65:71:85:ef:2e:6a:c0:a7:d9:aa user1@vm1
'The key's randomart image is:
+--[ DSA 1024]----+
|     ..          |
|    ..           |
|  . ..           |
|   o  .  o       |
|. o  .  S o      |
|o+ .  .    .     |
|o.=  .    o      |
|.o..o o  o       |
|E=+o o ..        |
+-----------------+
user1@vm1:~/ssh_test$ ssh-keygen -b 4096  -t rsa -N '' -v  -f ~/.ssh/id_rsa
Generating public/private rsa key pair.
Your identification has been saved in /home/user1/.ssh/id_rsa.
Your public key has been saved in /home/user1/.ssh/id_rsa.pub.
The key fingerprint is:
50:65:18:61:3f:41:36:07:4f:40:36:a7:4b:6d:64:28 user1@vm1
'The key's randomart image is:
+--[ RSA 4096]----+
|        =B&+*    |
|       oE=.&     |
|      .  .= +    |
|       . . +     |
|        S .      |
|                 |
|                 |
|                 |
|                 |
+-----------------+
user1@vm1:~/ssh_test$ cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
user1@vm1:~/ssh_test$ /usr/sbin/sshd -Ddf sshd_config > sshd.out 2>&1 &
[2] 26896
user1@vm1:~/ssh_test$ ssh-keyscan -H vm1 127.0.0.1 >> ~/.ssh/known_hosts
# 127.0.0.1 SSH-2.0-OpenSSH_5.5p1 Debian-6+squeeze2
# vm1 SSH-2.0-OpenSSH_5.5p1 Debian-6+squeeze2
[2]+  Exit 255                /usr/sbin/sshd -Ddf sshd_config > sshd.out 2>&1
user1@vm1:~/ssh_test$ /usr/sbin/sshd -Ddf sshd_config >> sshd.out 2>&1 &
[1] 26957
user1@vm1:~/ssh_test$ ssh vm1 -v -p 1024 2>ssh.out
Linux vm1 2.6.32-5-amd64 #1 SMP Sun May 6 04:00:17 UTC 2012 x86_64
 
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
 
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have mail.
Last login: Fri Jul 20 09:10:30 2012 from vm1.site
Environment:
  LANG=en_US.UTF-8
  USER=user1
  LOGNAME=user1
  HOME=/home/user1
  PATH=/usr/local/bin:/usr/bin:/bin:/usr/bin/X11:/usr/games
  MAIL=/var/mail/user1
  SHELL=/bin/bash
  SSH_CLIENT=127.0.1.1 47456 1024
  SSH_CONNECTION=127.0.1.1 47456 127.0.1.1 1024
  SSH_TTY=/dev/pts/0
  TERM=xterm
user1@vm1:~$ ps au --forest
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
user1    26224  0.0  1.2  23660  6576 pts/2    Ss   09:09   0:01 -bash
user1    27020  1.0  0.6  68392  3236 pts/2    S    09:50   0:00  \_ sshd: user1 [priv]
user1    27025  0.0  0.2  68392  1412 pts/2    S    09:50   0:00  |   \_ sshd: user1@pts/0
user1    27026  9.0  1.2  23564  6404 pts/0    Ss   09:50   0:00  |       \_ -bash
user1    27051  0.0  0.2  16308  1060 pts/0    R+   09:50   0:00  |           \_ ps au --forest
user1    27021  1.1  0.5  38504  2880 pts/2    S+   09:50   0:00  \_ ssh vm1 -v -p 1024
root      1107  0.0  0.1   5932   620 tty6     Ss+  Jul18   0:00 /sbin/getty 38400 tty6
root      1106  0.0  0.1   5932   616 tty5     Ss+  Jul18   0:00 /sbin/getty 38400 tty5
root      1105  0.0  0.1   5932   620 tty4     Ss+  Jul18   0:00 /sbin/getty 38400 tty4
root      1104  0.0  0.1   5932   620 tty3     Ss+  Jul18   0:00 /sbin/getty 38400 tty3
root      1103  0.0  0.1   5932   616 tty2     Ss+  Jul18   0:00 /sbin/getty 38400 tty2
root      1102  0.0  0.1   5932   616 tty1     Ss+  Jul18   0:00 /sbin/getty 38400 tty1
user1@vm1:~$ logout
user1@vm1:~/ssh_test$
[1]+  Exit 255                /usr/sbin/sshd -Ddf sshd_config > sshd.out 2>&1
user1@vm1:~/ssh_test$ /usr/sbin/sshd -Ddf sshd_config >> sshd.out 2>&1 &
[1] 27067
user1@vm1:~/ssh_test$ scp -v -P 1024 vm1:.bashrc . 2>scp.out
Environment:
  LANG=en_US.UTF-8
  USER=user1
  LOGNAME=user1
  HOME=/home/user1
  PATH=/usr/local/bin:/usr/bin:/bin:/usr/bin/X11:/usr/games
  MAIL=/var/mail/user1
  SHELL=/bin/bash
  SSH_CLIENT=127.0.1.1 47459 1024
  SSH_CONNECTION=127.0.1.1 47459 127.0.1.1 1024
.bashrc                                                                                                                     100% 3184     3.1KB/s   00:00
[1]+  Exit 255                /usr/sbin/sshd -Ddf sshd_config >> sshd.out 2>&1
```

## 解释

1.  创建`/home/user1/ssh_test`目录。
1.  使其成为当前工作目录。
1.  将`sshd_config`复制到此目录。
1.  将`sshd`监听端口从 22 更改为 1024，将副本命名为`sshd_config.bak`。
1.  替换 RSA 主机密钥位置。
1.  替换 DSA 主机密钥位置。
1.  显示`sshd_config`的旧版本和新版本之间的差异。
1.  生成具有空密码的，新的 4096 位 RSA 主机密钥对，将其保存到`/home/user1/ssh_test/ssh_host_rsa_key`和`/home/user1/ssh_test/ssh_host_rsa_key.pub`。
1.  同样的，但是对 DSA 密钥执行。
1.  生成新的认证密钥对，将其保存到`/home/user1/.ssh/id_rsa`和`/home/user1/.ssh/id_rsa.pub`。
1.  将`id_rsa.pub`复制到`/home/user1/.ssh/authorized_keys`，来允许无密码认证。
1.  在调试模式下，在端口 1024 上启动新的 SSH 服务器，将所有输出保存到`sshd.log`。
1.  提取 SSH 客户端的主机认证密钥，并将其提供给`/home/user1/.ssh/known_hosts`。
1.  在调试模式下，在端口 1024 上启动新的 SSH 服务器，将所有输出附加到`sshd.log`。这是因为在调试模式下， SSH 服务器只维护一个连接。
1.  使用`ssh`客户端连接到此服务器。
1.  以树形式打印当前正在运行的进程。你可以看到，你正在使用`sshd`启动的 bash，它服务于你的连接，而`sshd`又是由`sshd`启动，你在几行之前启动了你自己。。
1.  退出`ssh`会话。
1.  再次启动 SSH 服务器。
1.  将文件`.bashrc`从你的主目录复制到当前目录。

## 附加题

观看此视频，它解释了加密如何工作：<http://www.youtube.com/watch?v=3QnD2c4Xovk>
阅读：<http://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch03_04.htm>
阅读文件`ssh.out`，`scp.out`和`sshd.out`中的调试输出。向你自己解释发生了什么。
