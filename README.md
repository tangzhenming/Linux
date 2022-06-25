# Linux

Linux 日志

## [tmux](https://github.com/tmux/tmux)

[tmux 基本操作](https://blog.csdn.net/sui_152/article/details/121650341)

ssh

登录云服务器

```shell
# root：用户名
# 127.0.0.1 格式的地址： 云服务器 IP 地址（公网地址）
$ ssh root@127.0.0.1
```

> ssh 总结：
>
> Secure Shell（安全外壳协议，简称 SSH）是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。SSH 通过在网络中创建安全隧道来实现 SSH 客户端与服务器之间的连接。SSH 最常见的用途是远程登录系统，人们通常利用 SSH 来传输命令行界面和远程执行命令。
> https://zh.wikipedia.org/wiki/Secure_Shell

> ssh 协议默认端口号：
>
> 当 SSH 应用于 STelnet，SFTP 以及 SCP 时，使用的默认 SSH 端口都是 22。当 SSH 应用于 NETCONF 时，可以指定 SSH 端口是 22 或者 830。
> https://info.support.huawei.com/info-finder/encyclopedia/zh/SSH.html

IP 地址比较难记，可以通过配置 ssh 的 config 来对服务器起别名，通过别名来登录服务器。

- /etc/ssh/ssh_config
- ~/.ssh/config

> 配置文件说明：
>
> ~/.ssh/config（用户专用）和 /etc/ssh/ssh_config（全局共享）
>
> 要按照该顺序读取这些文件，对于给定的某个参数，它使用的是读取过程中发现的第一个配置。用户可以通过以下方式将全局参数设置覆盖掉：在自己的用户配置文件中设置同样的参数即可。在 ssh 或 scp 命令行上给出的参数的优先级要高于这两个文件中所设置的参数的优先级。
>
> 用户的~/.ssh/config 文件必须由该用户所有（他是目录"~/"的所有者），并且除了所有者之外任何人都不能写入该文件。否则客户端就会给出一条错误消息然后退出。这个文件的模式通常被设为 600，这是因为除了它的所有者之外任何人没有理由能够读取它。
>
> http://www.80vps.com/new6763.htm

免密登录远程服务器：

把本地文件 ~/.ssh/id_rsa.pub 中内容复制粘贴到远程服务器 ~/.ssh/authorized_keys ，即把自己的公钥放在远程服务器的 authorized_keys 中。

相关命令行工具：

```
# 在本地环境进行操作

# 提示你输入密码，成功之后可以直接 ssh 登录，无需密码
$ ssh-copy-id shanyue

# 登陆成功，无需密码
$ ssh shanyue
```
