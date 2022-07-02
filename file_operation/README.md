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

## 2. cd/pwd/ls

### 2.1 cd

change directory 切换目录

- . : 当前目录
- .. : 上级目录
- / : 根目录
- ~ : 当前用户目录，也叫做 home 目录，可用 $HOME 表示
- \- : 上一次工作目录

可使用 autojump 工具跳转任意目录；原理：将每次打开的目录维护到自身的数据库中，同名则根据跳转的次数设置权重。

### 2.2 pwd

print working directory 打印当前工作目录

### 2.3 ls

列出当前工作目录的内容

- -a: --all，列出所有文件，包括隐藏文件
- -l: --long，列出详细信息，包括文件大小、权限、创建时间、修改时间等
- -h: --human-readable，可读化格式进行输出，比如 -l 时文件大小不带单位，-h 就会补充人类可读的单位进行显示

### 2.4 exa

ls 的一个更强大的版本，支持更多的选项。

指令：

- -T: --tree，以树状图的形式列出文件
- -L: --level，指定层级

### 2.5 tree

```shell
# macos
$ brew install tree

# centos
$ yum install tree
```

指令：

- -L: 指定层级
- -F: 对目录末尾添加 /，对可执行文件末尾添加 \*
- -a: --all，显示所有文件，包括隐藏文件
