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

可使用 autojump 工具跳转任意目录。

原理：将每次打开的目录维护到自身的数据库中，同名则根据跳转的次数设置权重。

指令：

- -h : 查看帮助
- -a DIRECTORY, --add DIRECTORY add path
- -i [WEIGHT], --increase [WEIGHT] increase current directory weight
- -d [WEIGHT], --decrease [WEIGHT] decrease current directory weight
- --complete used for tab completion
- --purge remove non-existent paths from database
- -s, --stat show database entries and their key weights
- -v, --version show version information

安装：

MacOS: `brew install autojump`

Add the following line to your ~/.bash_profile or ~/.zshrc file: `[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh`, then `source ~/.zshrc`

### 2.3 pwd

print working directory 打印当前工作目录

### 2.4 ls

列出当前工作目录的内容

指令：

- -a: --all，列出所有文件，包括隐藏文件
- -l: --long，列出详细信息，包括文件大小、权限、创建时间、修改时间等
  - 第 1 列
  - 第一个字母 d 意味着内容是目录或者文件。在上面的截图中，Desktop、 Documents、 Downloads 和 lynis-1.3.8 是目录。如果是'-'(减号)，这意味着它的内容是文件。当它是 l(小写 l 字符)，意味这内容是链接文件。下面的 9 个字符是关于文件权限。前 3 个 rwx 字符是文件的拥有者的权限，第二组 3rwx 是文件的所有组的权限，最后的 rwx 是对其他人访问文件的权限。
  - 第 2 列 这行告诉我们有多少链接指向这个文件。
  - 第 3 列 这行告诉我们谁是这个文件/文件夹的所有者。
  - 第 4 列 这行告诉我们谁是这个文件/文件夹的所有组。
  - 第 5 列 这行告诉我们这个文件/文件夹的以字节为单位的大小。 目录的大小总是 4096 字节。
  - 第 6 列 这告诉我们文件最后的修改时间。
  - 第 7 列 这告诉我们文件名或者目录名。
- -h: --human-readable，可读化格式进行输出，比如 -l 时文件大小不带单位，-h 就会补充人类可读的单位进行显示

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

### 2.7 如何列出某个目录下的所有 js 文件与 html 文件

`ls -alh *js *html`

连同子目录的也一起列出来：`find . -name "*.js" -o -name "*.html" | xargs ls -alh` 或者 `find . -regextype posix-extended -regex ".*\.(js|html)" | xargs ls -alh`

## 3. stat/link

### 3.1 stat

查看文件系统信息。

- regular file: 普通文件
- Size: 文件大小
- Links: 文件硬链接个数
- Access Mode: 文件访问模式
- Access: atime, 文件访问时间
- Modify: mtime, 文件修改时间
- Change: ctime, 文件修改时间(包括属性，比如 mode 和 owner，也包括 mtime)
- Birth: 某些操作系统其值为 -

### 3.2 link

- `ln file file-hard`: 在两个文件间创建链接，默认为硬链接。
  - file 是源文件，file-hard 是硬链接文件
  - 两者通过 stat 命令查看信息，可以发现：硬链接个数 Links 都变为 2 ，所有属性都相同。
- `ln -s file file-soft`: 在两个文件间创建软链接
  - stat file-soft 发现，cat 内容相同
  - Inode 不同，这说明两者是独立的文件
  - 源文件中的 regular file 标记变成了 symbolic link 标记

pnpm 中广泛使用了硬链接和软链接: [浅谈 pnpm 软链接和硬链接](https://blog.csdn.net/weixin_43990363/article/details/121757838)。

### 3.3 修改文件的 mode 和 mtime ，git 中是否会有更改操作？

1. 使用 chmod 修改文件的 mode ，使用 ls -l 或 stat 查看文件或文件夹的权限
   1. `sudo chmod 777 README.md`
   2. git 中会有更改操作
2. 使用 touch 修改文件的 mtime ，使用 stat 查看文件的 mtime
   1. `touch README.md`: 默认就是修改一个文件的 atime 和 mtime 为当前系统的时间
   2. 指令
      1. -a : 仅修改 access time。
      2. -c : 如果文件不存在，则不创建新文件。
      3. -d : 后面可以接日期，也可以使用 –date=”日期或时间”
      4. -m : 仅修改 modify time。
      5. -t : 后面可以接时间，格式为 [YYMMDDhhmm]
   3. 仅修改 mtime: `touch -c -m -t 202207030000.01 README.md`
   4. git 中不会有更改操作
