# Linux

Linux 日志

## Catalog

- [远程连接](https://github.com/tangzhenming/Linux/tree/main/remote_connection)
- [apps](https://github.com/tangzhenming/Linux/tree/main/apps)
- [cvm_ecs](https://github.com/tangzhenming/Linux/tree/main/cvm_ecs)
- [文件操作](https://github.com/tangzhenming/Linux/tree/main/file_operation)
- ...

## Reference

[Linux 技能实战](https://q.shanyue.tech/command/)

[Linux rsync 命令用法详解](http://c.biancheng.net/view/6121.html)

[Linux 教程](https://www.runoob.com/linux/linux-tutorial.html)

## 未分类知识点

### Linux 服务器语言设置问题

有时候会发现在服务器中使用 stat 查看文件信息时，显示的是中文，使用 `echo $LANG` 命令查看系统语言设置，也是中文 `zh_CN.UTF-8` （还可以使用 `locale` 命令查看安装的语言包），但我们印象中并没有设置过中文。

这是因为 Linux 中的环境变量 LANG 的值（使用 env 查看 Linux 环境变量），会在 ssh 连接服务器的时候，使用客户端传递过来的值。我们可以使用 `ssh -v tang` 或者 `ssh -vvv tang`（不会退出）的调试模式来远程登录，可以看到登录期间执行的动作，我们发现 ssh 执行了这个操作：`debug1: channel 0: setting env LANG = "zh_CN.UTF-8"` ，这里就是在设置环境变量 LANG 为中文。

P.S 我们还能发现这个操作 `debug1: identity file /Users/tangzhenming/.ssh/id_rsa type 0` ，这里就是在验证我们的私钥。

我们可以通过 `LANG=“en_US.UTF-8”` 临时设置系统语言。

### [linux 中，&和&&,|和||](https://blog.csdn.net/ccoran/article/details/84727034)
