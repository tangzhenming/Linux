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

## 2. cd/autojump/pwd/ls/exa/tree

### 2.1 cd

change directory 切换目录

- . : 当前目录
- .. : 上级目录
- / : 根目录
- ~ : 当前用户目录，也叫做 home 目录，可用 $HOME 表示
- \- : 上一次工作目录

### 2.2 autojump

可使用 autojump 工具跳转任意目录；原理：将每次打开的目录维护到自身的数据库中，同名则根据跳转的次数设置权重。

- -h : 查看帮助
- -a DIRECTORY, --add DIRECTORY add path
- -i [WEIGHT], --increase [WEIGHT] increase current directory weight
- -d [WEIGHT], --decrease [WEIGHT] decrease current directory weight
- --complete used for tab completion
- --purge remove non-existent paths from database
- -s, --stat show database entries and their key weights
- -v, --version show version information

### 2.3 pwd

print working directory 打印当前工作目录

### 2.4 ls

列出当前工作目录的内容

#### 2.4.1 指令

- -a: --all，列出所有文件，包括隐藏文件
- -l: --long，列出详细信息，包括文件大小、权限、创建时间、修改时间等
- -h: --human-readable，可读化格式进行输出，比如 -l 时文件大小不带单位，-h 就会补充人类可读的单位进行显示

#### 2.4.2 列出指定文件类型的文件

`ls -lah *html *png`

### 2.5 exa

ls 的一个更强大的版本，支持更多的选项。

指令：

- -T: --tree，以树状图的形式列出文件
- -L: --level，指定层级

### 2.6 tree

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

## 3. stat/link

### 3.1 stat

查看文件系统信息。

### 3.2 link

- `ln file file-hard`: 在两个文件间创建链接，默认为硬链接。
  - file 是源文件，file-hard 是硬链接文件
  - 两者通过 stat 命令查看信息，可以发现：硬链接个数 Links 都变为 2 ，所有属性都相同。
- `ln -s file file-soft`: 在两个文件间创建软链接
  - stat file-soft 发现，cat 内容相同
  - Inode 不同，这说明两者是独立的文件
  - 源文件中的 regular file 标记变成了 symbolic link 标记

pnpm 中广泛使用了硬链接和软链接。

### 3.3
