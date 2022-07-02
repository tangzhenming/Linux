# 文件操作

## 1. rsync

指令：

- -l: --links，拷贝符号链接
- -a: --archive，归档模式
- -h: --human-readable，可读化格式进行输出
- -z: --compress，压缩传输
- -v: --verbose，详细输出
- -r: --recursive，递归拷贝

### 1.1 拷贝至远程

`rsync [options] source(local) destination(remote)`

### 1.2 拷贝目录

不以 / 结尾，代表将该目录连同目录名一起进行拷贝。

以 / 结尾，代表将该目录下所有内容进行拷贝。

## 2. cd/ls
