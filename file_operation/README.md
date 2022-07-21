# 目录文件操作

## 1. rsync

一个文件拷贝工具，支持断点续传、按需拷贝、远程服务器拷贝。

options:

- -l: --links，拷贝符号链接
- -a: --archive，归档模式
  - 可以拷贝元属性，如 ctime/mtime/mode
- -h: --human-readable，可读化格式进行输出
- -z: --compress，压缩传输
- -v: --verbose，详细输出
- -r: --recursive，递归拷贝
- --exclude: 拷贝忽略项

### 1.1 拷贝至远程

`rsync [options] source(local) host:/destination(remote)`

### 1.2 拷贝目录

源目录结尾：

- 不以 / 结尾，代表将该目录连同目录名一起进行拷贝
- 以 / 结尾，代表将该目录下所有内容进行拷贝

## 2. cd/autojump/pwd/ls/exa/tree

### 2.1 cd

change directory 切换目录

- . : 当前目录
- .. : 上级目录
- / : 根目录
- ~ : 当前用户目录，也叫做 home 目录，可用 $HOME 表示
- \- : 上一次工作目录（扩展：git checkout - : 切换到上一次的分支）

### 2.2 [autojump](https://github.com/wting/autojump)

可使用 autojump 工具跳转任意目录。

原理：将每次打开的目录维护到自身的数据库中，同名则根据跳转的次数设置权重。

#### 2.2.1 安装

- MacOS: `brew install autojump`
  - Add the following line to your ~/.bash_profile or ~/.zshrc file: `[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh`, then `source ~/.zshrc`
- Manual: download then run `./install.py or ./uninstall.py`

#### 2.2.2 指令

- -h : 查看帮助
- -a DIRECTORY, --add DIRECTORY add path
- -i [WEIGHT], --increase [WEIGHT] increase current directory weight
  - e.g. `j -i 10`
- -d [WEIGHT], --decrease [WEIGHT] decrease current directory weight
- --complete used for tab completion
- --purge remove non-existent paths from database
- -s, --stat show database entries and their key weights
- -v, --version show version information

#### 2.2.3 用例

1. 跳转到一个包含 foo 的目录：`j foo`
2. 跳转到当前目录下的包含 bar 的子目录：`jc bar`
3. 在资源管理器（文件管理窗口等）打开目录 foo : `jo foo` （仅支持存在资源管理器的系统）
4. 合并 2 和 3 : `jco bar`
5. 假设数据库中存在两个目录：30 /home/user/mail/inbox 和 10 /home/user/work/inbox ，使用 `j in` 会跳转到 /home/user/mail/inbox ，使用 `j w in` 会跳转到优先级较低的 /home/user/work/inbox

### 2.3 pwd

print working directory 打印当前工作目录

### 2.4 ls

列出当前工作目录的内容

#### 2.4.1 指令

常用指令：`ls -lah`

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

#### 2.4.2 配置颜色区分目录和文件

实际生效指令：`ls --color=auto`

可以在环境变量文件中配置别名（alias）：

1. `vim ~/.bashrc`
2. `alias ls='ls --color=auto'`
3. `source ~/.bashrc`

### 2.5 [exa](https://github.com/ogham/exa)

ls 的一个更强大的版本（替代品），支持更多的选项，更友好的输出展示。

#### 2.5.1 [manual install](https://the.exa.website/install/linux#manual)

1. download && rsync to /usr/local/ (or just curl -O [exa link])
2. put it in /usr/local/, or any of the directories listed in the $PATH environment variable
3. soft link
   1. ln -s /usr/local/exa-linux-x86_64-v0.10.0/bin/exa /usr/local/bin/
   2. 如果需要重新建立，直接加 -f ，即 -sf
4. modify the $PATH variable, /etc/profile, add the directory where exa's bin directory is located

   ```bash
   export EXA_HOME=/usr/local/exa-linux-x86_64-v0.10.0

   export PATH=$EXA_HOME/bin:$PATH
   ```

5. source /etc/profile
6. 直接执行文件夹中的 exa 会报错缺少依赖，下面进行安装
7. glibc
   ```bash
   curl -O http://ftp.gnu.org/gnu/glibc/glibc-2.18.tar.gz
   tar -zxvf glibc-2.18.tar.gz
   cd glibc-2.18
   mkdir build
   cd build
   ../configure --prefix=/usr
   ```
8. yum install -y gcc
9. make -j4
10. make install

#### 2.5.2 command and options

- -L: --level，指定层级
- -1, --oneline: display one entry per line
- -G, --grid: display entries as a grid (default)
- -l, --long: display extended details and attributes
- -R, --recurse: recurse into directories
- -T, --tree: recurse into directories as a tree 以树状图的形式列出文件
- -x, --across: sort the grid across, rather than downwards
- -F, --classify: display type indicator by file names
- --colo[u]r: when to use terminal colours
- --colo[u]r-scale: highlight levels of file sizes distinctly
- --icons: display icons
- --no-icons: don't display icons (always overrides --icons)
- -- git 输出文件的 Git 状态
  - N：新文件
  - M：文件有变动
  - I：该文件被忽略

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

## 4. user

相关命令：

1. whoami: 打印当前用户名字
2. id: 打印当前用户 ID 以及用户组 ID
3. who: 打印当前处于登录状态的用户列表
   1. -H: 显示表头
   2. -u: 显示 IDLE 和 PID
   3. 结果中的 LINE pts/0 表示 在 pts/0 的 tty 登录
   4. IDLE 表示当前用户已经处于不活跃状态多长时间，. 代表当前仍在活跃状态
4. w: 同上
5. last: 打印该服务器的历史登录用户
   1. -h: 不存在的命令，可查看支持的命令
   2. -n \_number: 列出最近 number 个
   3. -t：查看指定时间之前的用户登录历史
