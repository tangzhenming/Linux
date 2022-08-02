# Linux

Linux 日志

## Catalog 📚

- [1. 远程连接](https://github.com/tangzhenming/Linux/tree/main/remote_connection)
- [2. Linux apps](https://github.com/tangzhenming/Linux/tree/main/apps)
- [3. 目录文件操作](https://github.com/tangzhenming/Linux/tree/main/file_operation)
- ...

## 思考 🤔

1. [在前后端项目中都可能有一个 .env 文件，它是什么？有什么用处？如何工作的？]()
2. [在 pnpm 中，结合软链接与硬链接大幅降低了装包的体积，它是如何做到的？]()
3. [在 nodejs/python/go 中都可以开发全局可执行命令，开发一个全局命令的原理是什么？]()
4. [在 vue/react/next 等脚手架的开发环境，如果启动时发生端口冲突，则会重开一个端口，此种原理如何？]()
5. [在 nginx 中我们得知 last-modified 是根据 mtime 生成，那如何获得静态资源的 mtime 呢？]()
6. [在写 Node.js 时，他人常说 Node.js 写服务更消耗内存，那如何监听到某个 Node.js 服务的内存占用情况呢？]()
7. [当打开一个 URL 时发生了什么。此时有一些发生在 http/dns/tcp 协议的一些事情，我们如何通过相关命令监控这一过程呢？]()
8. [如何在个人服务器启动自己服务，过了一分钟，ssh 断开，服务挂掉，这应该如何处理？]()
9. [SSH 连接 Linux 服务器时的语言设置问题](https://github.com/tangzhenming/Linux/issues/1)
10. [如何列出某个目录下的所有 js 文件与 html 文件](https://github.com/tangzhenming/Linux/issues/2)
11. [修改文件的 mode 和 mtime ，git 中是否会有更改操作？](https://github.com/tangzhenming/Linux/issues/3)
12. [在服务器中安装了 mysql 数据库，我们如何更安全地链接数据库？](https://github.com/tangzhenming/Linux/issues/4)
13. [在 Node.js 或其它语言中如何实现 cp](https://github.com/tangzhenming/Linux/issues/5)
14. [为何说保留复制文件时的元属性，对静态资源服务器有益]()
15. [在使用 rsync 传输前端项目时，如何忽略 node_modules 目录]()
16. [使用 tree/exa 列出目录树时，如何忽略 .gitignore 中文件内容]()
17. [在 Node.js 或其它语言中如何获得 pwd]()
18. [在 Node.js 或其它语言中如何获得 ls 子文件列表。参考 fsp.readdir 及 readdir]()
19. [如何查出当前服务器上有多少个登录用户]()
20. [如何查出某天服务器上有多少个登录用户](https://umiinn9jie.feishu.cn/wiki/wikcn80sCv4n1VbfhABjgT6jfjg)
21. [在 pnpm 中，为什么不全部使用软链接]()
22. [你知道 npm install -g 全局安装的命令行为什么可以直接使用呢？]()
23. [如何设计一个可以切换 node 版本的命令行工具，比如 n 与 nvm](https://umiinn9jie.feishu.cn/wiki/wikcnNeiM2IGoASWVSwfhMtVtsf)

## Reference

[Linux 技能实战](https://q.shanyue.tech/)

[Linux 命令搜索引擎](https://wangchujiang.com/linux-command/)

[Linux 入门 —— Linux rsync 命令用法详解](http://c.biancheng.net/view/6121.html)

[菜鸟教程 —— Linux 教程](https://www.runoob.com/linux/linux-tutorial.html)

[命令行的艺术](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)

[explainshell](https://explainshell.com/)

## 未分类/未完成 知识点

[linux 中，&和&&,|和||](https://blog.csdn.net/ccoran/article/details/84727034)

---

关于 gnu linux debian

https://www.gnu.org/gnu/linux-and-gnu.html

https://www.gnu.org/

https://www.debian.org/releases/buster/amd64/ch01s03.en.html

https://blog.venatir.com/static/e827c73a4a5fb74e3df1c186712dcfd9/cc6fe/linux-timeline.png
![image](https://user-images.githubusercontent.com/28591906/179732116-fd77cbb0-71eb-4dd3-90e0-baa4fceeb21e.png)

uname -a 查看 linux 系统信息 uname -s 内核名称 uname -o 操作系统

完整的操作系统是 gnu

linux 是内核

基于 GNU 操作系统之上，我们再添加 linux 内核，就是我们现在所说的 linux

debian 也是基于 gnu 和 linux 内核再进一步封装和扩展出来的系统

然后 ubantu 再基于 debian 继续衍生

同理 red hat 也是，然后 centos 再基于 red hat 衍生

cat /etc/redhat-release 文件存在为 centos ，因为 centos 是从 red hat 衍生的

--- 关于 Linux 中的某些目录和配置文件
[linux 系统中的 /bin /sbin /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin 目录的区别](https://www.cnblogs.com/smallrookie/p/7089008.html)

[Linux /etc/profile 文件详解](https://www.cnblogs.com/lh03061238/p/9952659.html)

[关于 Linux 服务器里 /usr/bin 目录和 /usr/local/bin 目录](https://blog.csdn.net/LittlePoem/article/details/109510849)

[man 命令 – 查看帮助信息](https://www.linuxcool.com/man)

你知道 npm install -g 全局安装的命令行为什么可以直接使用呢？

山月 3:38 PM Jul 26

npm install -g pkg 时，通过 -g 会将 pkg 安装到全局目录下的 node_modules，在 linux 中假设为 /usr/local/lib/node_modules
​
如果该 pkg 拥有可执行文件，比如 webpack/vite/eslint 均有可执行文件。则在 package.json 中有一个字段是 bin。
​
node 将 bin 中字段指向的文件软链接至 $PATH 的某个目录，则该命令可以全局执行。
