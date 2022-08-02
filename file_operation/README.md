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
  - 第 3 列 这行告诉我们谁是这个文件/文件夹的所有者（所属用户）。
  - 第 4 列 这行告诉我们谁是这个文件/文件夹的所有组（所属用户组）。
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

文件元属性：

- regular file: 普通文件
- Size: 文件大小
- Links: 文件硬链接个数
- Access Mode: 文件访问模式
- Access: atime, 文件访问时间
- Modify: mtime, 文件修改时间（在 HTTP 服务器中，常以此作为 last-modified 响应头）
- Change: ctime, 文件修改时间(包括属性，比如 mode 和 owner，也包括 mtime)
- Birth: 某些操作系统其值为 -

ctime 描述文件状态更新更准确，mtime 是只文件内容有变化就会更新

指令：

- -c: 指定文件某个属性进行输出
  - %a access rights in octal
  - %A access rights in human readable form
  - %f raw mode in hex
  - %F file type
  - %g group ID of owner
  - %G group name of owner
  - %h number of hard links
  - %i inode number
  - %n file name
  - %s total size, in bytes
  - e.g. `stat -c '%F' whoami.yaml`

判断文件是一个软链接：stat -c "%F" test-soft 输出 symbolic link

查询硬链接个数：stat -c "%h" test1

Linux 中一切皆文件，展示其中一些（~/traefik/）：

- regular file: `stat -c '%F' whoami.yaml`
- directory: `stat -c '%F' blog`
- symbolic link
- socket
- character special file
- block special file

使用 ls -lah 第一个字符也表示文件类型：

- -，regular file。普通文件。
- d，directory。目录文件。
- l，symbolic link。符号链接。
- s，socket。套接字文件，一般以 .sock 作为后缀。（可把 .sock 理解为 API，我们可以像 HTTP 一样对它请求数据）
- b，block special file。块设备文件。
- c，character special file。字符设备文件。

### 3.2 link

- `ln file file-hard`: 在两个文件间创建链接，默认为硬链接。
  - file 是源文件，file-hard 是硬链接文件
  - 两者通过 stat 命令查看信息，可以发现：硬链接个数 Links 都变为 2 ，所有属性都相同，特别注意： Inode 相同。
- `ln -s file file-soft`: 在两个文件间创建软链接
  - stat file-soft 发现，cat 内容相同
  - 特别注意：Inode 不同，这说明两者是独立的文件
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
4. w: 同上，更强大，可代替 who
5. last: 打印该服务器的历史登录用户
   1. -h: 不存在的命令，可查看支持的命令
   2. -n \_number: 列出最近 number 个
   3. -t：查看指定时间之前的用户登录历史

## 5. chmod/chown

chmod：改变文件的读写权限

%a：获得数字的 mode: stat -c %a README.md

644

%A：获得可读化的 mode: stat -c %A README.md

-rw-r--r--

- user 文件当前用户
- group 文件当前用户所属组
- other 其它用户

rw-：当前用户可写可读，110，即十进制 6
r--：当前用户组（当前用户所在用户组）可读，100，即十进制 4
r--：其它用户可读，100，即十进制 4

加起来就是 644: -rw-r--r--

777，即所有用户都有 rwx 权限，即可读可写可执行

以可读化的形式添加权限：

```sh
# u: user
# g: group
# o: other
# a: all
# +-=: 增加减少复制
# perms: 权限
$ chmod [ugoa...][[+-=][perms...]...]

# 为 yarn.lock 文件的用户所有者添加可读权限
$ chmod u+r yarn.lock

# 为所有用户添加 yarn.lock 的可读权限
$ chmod a+r yarn.lock

# 为所有用户删除 yarn.lock 的可读权限
$ chmod a-r yarn.lock

```

chown -R shanyue:shanyue . ：将 . 目录下的所属用户:所属用户组更改为 shanyue ，遍历子文件进行

## 6. cat/less/head/tail

cat，concatenate 缩写，concatenate and print files 连接文件并打印至标准输出（stdout）。

cat 多个文件：cat test1 test2

原理：

我们在打开一个文件，读取内容时，在操作系统底层实际上做了两步操作。

- open：open("package.json")，并返回文件描述符，即 file descriptor，简写 fd，一个非负整数，通过文件描述符可用来读写文件。
- read：read(3)，通过 fd 读取文件内容，其中的 3 为文件描述符。
- 在 Node.js 中，你会发现它有一个 API 为 fs.readFile，它实际上是 fs.open 与 fs.read 的结合体。

文件描述符：内核（kernel）利用文件描述符（file descriptor）来访问文件。 文件描述符是非负整数。 打开现存文件或新建文件时，内核会返回一个文件描述符。 读写文件也需要使用文件描述符来指定待读写的文件。
[文件描述符到底是啥？](https://zhuanlan.zhihu.com/p/143847169)

使用 less 也可以查看文件内容：less -N 显示行号 less -N whoami.yaml

使用 head 也可以查看文件内容，并且还能筛选行数和字节数

读取文件或者标准输入的前 N 行或者前 N 个字节：

前 3 行：head -3 whoami.yaml

前 3 个字节：head -c 3 whoami.yaml

tail，同 head ，读取文件或者标准输入的最后 N 行或最后 N 个字节

与 head 最大不同的一点是：--follow，简写为 -f。它可以实时打印文件中最新内容。

在调试日志时非常有用：日志一行一行追加到文件中，而 tail -f 可以实时打印追加的内容。

## 7. pipe/redirection/heredoc/日志重定向

### pipe

| 构成了管道，它将前边命令的标准输出（stdout）作为下一个命令的标准输入（stdin）。

读取 package.json 内容，读取前十行，再读取最后三行：`cat package.json | head -10 | tail -3`

在上边提到标准输入（stdin）与标准输出（stdout），其实，stdin/stdout 就是特殊的文件描述符。

- stdin，fd = 0（fd file descriptor），直接从键盘中读取数据（一般指的是键盘输入到缓冲区里的内容）
- stdout，fd = 1，直接将数据打印至终端
- stderr，fd = 2，标准错误，直接将异常信息打印至终端

### redirection

- \> 将文件描述符或标准输出中内容写入文件
- \>> 将文件描述符或标准输出中内容追加入文件

```bash
# echo 命令会输出【作为参数传递给标准输出 stdout】的字符串
echo "Hello World" > hello.txt
echo more >> hello.txt
```

### heredoc

```bash
<<EOF
# 在这里写入文本内容
EOF

# 写入的文本内容作为标准输入，最终写入结束后作为标准输出输出到终端
```

```bash
cat <<EOF > hello.txt
# 在这里写入文件内容
EOF

# 写入的文本内容作为标准输入，在写入结束之后作为标准输出，而 > 会将标准输出中的内容写入到文件 hello.txt 中
```

### 日志重定向

/dev/null 是一个空文件，对于所有的输入都统统吃下，化为乌有，有时，为了不显示日志，可将所有标准输出重定向至 /dev/null。

但此时，stderr 仍然会打印至屏幕。如果后边跟一个 2>&1，表示将 stderr (fd 为 2) 重定向至 &1 (fd===1 的文件，及 stdout)，同标准输出一同重定向至 /dev/null，也就是标准输出日志与标准错误日志都不显示。

```bash
# 不显示 stdout 内容
$ echo hello > /dev/null

# 既不显示 stdout，也不显示 stderr
# 此时 hello 文件不存在，如果没有后边的 2>&1，仍然会有日志打印至屏幕，如果加上 2>&1，则 stderr 也不显示
$ cat hello
$ cat hello > /dev/null
$ cat hello > /dev/null 2>&1
```

## 8. glob/extglob

### glob

glob，global 的简写，使用通配符来匹配大量文件。比如 rm \*.js 就可以删除当前目录所有 js 文件。

glob 拥有以下基本语法

- \*：匹配 0 个及以上字符
- ?：匹配 1 个字符
- [...]：range，匹配方括号内所有字符
- \*\*：匹配 0 个及多个子目录（在 bash 下，需要开启 globstar 选项，见下 shopt 命令）

```bash
# 列出当前目录下所有的 js 文件
$ ls -lah *.js
-rw-r--r--  1 tangzhenming  staff   4.5K Jul  7 17:04 787.ee4aa10d.chunk.js
-rw-r--r--  1 tangzhenming  staff   141K Jul  7 17:04 main.f0b18748.js

# 列出当前目录及所有子目录的 js 文件
$ ls -lah **/*.js

# 列出当前目录及所有子目录的后缀名为两个字母的文件
$ ls -lah **/*.??

# 列出当前目录中，以 2 或者 5 或者 8 开头的文件
$ ls -lah [258]*
```

### extglob

还有一些扩展的 glob 模式

- ?(pattern-list)，重复 0 次或 1 次的模式
- \*(pattern-list)，重复 0 次或多次
- +(pattern-list)，重复 1 次或多次
- @(pattern-list)，重复 1 次
- !(pattern-list)，非匹配

列出所有以 js/json/md 结尾的文件 `$ ls -lah *.*(js|json|md)`

在 bash 中， extglob 需要通过 shopt 命令手动开启。

```bash
$ shopt | grep glob
dotglob         off
extglob         on
failglob        off
globasciiranges off
globstar        off
nocaseglob      off
nullglob        off

$ shopt -s extglob
```

在 zsh 中，extglob 需要通过 setopt 命令手动开启。

```zsh
$ setopt extendedglob
$ setopt kshglob
```

判断当前终端是哪个 shell，可以使用 `$ echo $SHELL` / `echo $0`

## brace 语法

brace，用以扩展集合、数组等，有以下语法。

- set：{a,b,c}
- range：{1..10}，{01..10}
- step：{1..10..2}

```bash
$ echo {a,b,c}
a b c

# range: 输出 01 到 10
$ echo {01..10}
01 02 03 04 05 06 07 08 09 10

# step: 输出 1 到 10，但是每一步需要自增 2
$ echo {1..10..2}
1 3 5 7 9

# step: 输出 1 到 10，但是每一步需要自增 3
$ echo {1..10..3}
1 4 7 10

# step: 输出 10 到 1，但是每一步需要自减 2
$ echo {10..1..2}
10 8 6 4 2

$ echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z
```

结合其他指令：

```bash
# 列出当前目录下所有的 json 与 md 文件
$ ls -lah {*.json,*.md}

# 创建 a.js 到 z.js 26个文件
$ touch {a..z}.js

$ ls *.js
a.js  c.js  e.js  g.js  i.js  k.js  m.js  o.js  q.js  s.js  u.js  w.js  y.js
b.js  d.js  f.js  h.js  j.js  l.js  n.js  p.js  r.js  t.js  v.js  x.js  z.js
```
