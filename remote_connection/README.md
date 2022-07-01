# 远程连接

## SSH

### 1. SSH 登录

#### 1.1 使用公网 IP 登录

```shell
# root：用户名
# 0.0.0.0 格式的地址： 云服务器 IP 地址（公网地址）
$ ssh root@0.0.0.0
```

> ssh 总结：
>
> Secure Shell（安全外壳协议，简称 SSH）是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。SSH 通过在网络中创建安全隧道来实现 SSH 客户端与服务器之间的连接。SSH 最常见的用途是远程登录系统，人们通常利用 SSH 来传输命令行界面和远程执行命令。
> https://zh.wikipedia.org/wiki/Secure_Shell

> ssh 协议默认端口号：
>
> 当 SSH 应用于 STelnet，SFTP 以及 SCP 时，使用的默认 SSH 端口都是 22。当 SSH 应用于 NETCONF 时，可以指定 SSH 端口是 22 或者 830。
> https://info.support.huawei.com/info-finder/encyclopedia/zh/SSH.html

#### 1.2 公网 IP 配置别名登录

IP 地址比较难记，可以通过配置 ssh 的 config 来对服务器起别名，通过别名来登录服务器。

- /etc/ssh/ssh_config
- ~/.ssh/config

```vim
# ssh 配置文件 ~/.ssh/config

Host tang
    HostName 0.0.0.0
    User root
Host otheruser
    HostName 0.0.0.0
    User otheruser
```

> 配置文件说明：
>
> ~/.ssh/config（用户专用）和 /etc/ssh/ssh_config（全局共享）
>
> 要按照该顺序读取这些文件，对于给定的某个参数，它使用的是读取过程中发现的第一个配置。用户可以通过以下方式将全局参数设置覆盖掉：在自己的用户配置文件中设置同样的参数即可。在 ssh 或 scp 命令行上给出的参数的优先级要高于这两个文件中所设置的参数的优先级。
>
> 用户的~/.ssh/config 文件必须由该用户所有（他是目录"~/"的所有者），并且除了所有者之外任何人都不能写入该文件。否则客户端就会给出一条错误消息然后退出。这个文件的模式通常被设为 600，这是因为除了它的所有者之外任何人没有理由能够读取它。
>
> http://www.80vps.com/new6763.htm

#### 1.3 免密登录

1. 把公钥（`~/.ssh/id_rsa.pub`）放到远程服务器的 `~/.ssh/authorized_keys` 文件中。
2. 使用 `ssh-copy-id` 命令来将公钥拷贝到远程服务器，等同于 1 的操作。

#### 1.4 禁用密码登录

目的：最大限度保障服务器的安全性。

修改云服务器的 sshd 配置文件：/etc/ssh/sshd_config。其中 PasswordAuthentication 设置为 no，以此来禁用密码登录。

```shell
# 编辑服务器端的 /etc/ssh/sshd_config
# 禁用密码登录

Host *
  PasswordAuthentication no
```

#### 1.5 保持连接

ssh 到一个远程主机，一段时间未操作就会断开连接。

编辑 ~/.ssh/config

```shell
Host *
  ServerAliveInterval 30
  TCPKeepAlive yes
  ServerAliveCountMax 6
  Compression yes
```

ServerAliveInterval: Sets a timeout interval in seconds after which if no data has been received from the server, ssh(1) will send a message through the encrypted channel to request a response from the server. The default is 0, indicating that these messages will not be sent to the server.

ServerAliveCountMax: Sets the number of server alive messages (see below) which may be sent without ssh(1) receiving any messages back from the server. If this threshold is reached while server alive messages are being sent, ssh will disconnect from the server, terminating the session. It is important to note that the use of server alive messages is very different from TCPKeepAlive (below). The server alive messages are sent through the encrypted channel and there‐ fore will not be spoofable. The TCP keepalive option enabled by TCPKeepAlive is spoofable. The server alive mechanism is valu‐ able when the client or server depend on knowing when a connec‐ tion has become inactive.
The default value is 3. If, for example, ServerAliveInterval (see below) is set to 15 and ServerAliveCountMax is left at the default, if the server becomes unresponsive, ssh will disconnect after approximately 45 seconds.

Compression: Specifies whether to use compression. The argument must be yes or no (the default).

TCPKeepAlive: Specifies whether the system should send TCP keepalive messages to the other side. If they are sent, death of the connection or crash of one of the machines will be properly noticed. However, this means that connections will die if the route is down temporarily, and some people find it annoying. The default is yes (to send TCP keepalive messages), and the client will notice if the net‐ work goes down or the remote host dies. This is important in scripts, and many users want it too. To disable TCP keepalive messages, the value should be set to no.

#### 1.6 [重装 ecs/cvm 后公钥变更，导致 SSH 远程登录服务器报错：WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!](https://blog.csdn.net/ltstud/article/details/83011125)

1.` ~/.ssh/known_hosts` 是记录远程主机的公钥的文件 2. 手动清除文件中对应远程主机的公钥 3. 或者使用命令：`ssh-keygen -R [远程主机的 IP 地址]` 进行清除（推荐）

### 2. SSH 隧道

通过 SSH 将远程主机端口号映射到宿主机本地。

- -N: 端口转发
- -L: 端口映射，支持远程的端口在本地进行访问
  - `ssh -L [bind_address:]port:host:hostport`
  - 左侧为本地 IP 和端口
  - 将服务器中的 8080 端口映射到本地 5000 端口：`ssh -NL 5000:localhost:8080 tang`
- -R: 端口映射，支持本地的端口在远程进行访问
  - `ssh -R [bind_address:]port:host:hostport`
  - 左侧为远程 IP 和端口
