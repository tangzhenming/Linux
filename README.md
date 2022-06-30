# Linux

Linux 日志

## [tmux](https://github.com/tmux/tmux)

[tmux 基本操作](https://blog.csdn.net/sui_152/article/details/121650341)

# ECS（Elastic Compute Service）& CVM（Cloud Virtual Machine）

云服务器日志

[ECS 控制台](https://ecs.console.aliyun.com/#/home)

[将 ECS 重置为初始状态 —— 初始化磁盘](https://help.aliyun.com/document_detail/97749.html?spm=5176.22414175.sslink.4.32d1d6b2Y829Q3)

[更换 ECS 系统盘](https://help.aliyun.com/document_detail/50134.htm?spm=a2c4g.11186623.0.0.6a649f3cx3PnO0#concept-n4k-x3j-ydb)

[重置 ECS 后公钥失效，需要清理本地公钥记录](https://blog.csdn.net/ltstud/article/details/83011125)

## 公钥变更导致 SSH 远程登录服务器报错：WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

1. ~/.ssh/known_hosts 是记录远程主机的公钥的文件
2. 手动清除文件中对应远程主机的公钥
3. 或者使用命令：ssh-keygen -R [远程主机的 IP 地址] 进行清除（推荐）

他喵的阿里云真贵，转腾讯云 CVM

[CVM 实例控制台](https://console.cloud.tencent.com/cvm/instance/index?rid=4)



[使用 rsync 同步/上传文件](http://c.biancheng.net/view/6121.html)
