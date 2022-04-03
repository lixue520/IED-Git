##### 1. 匹配特定字符串并输出到文件夹

- 创建file.txt

- 将 `/etc/passwd` 文件里面包含 `shiyanlou` 字符串的内容输出到刚刚创建的 `file.txt` 文件里面。

  ```shell
  ls /etc/passwd >file.txt
  grep shiyanlou /etc/passwd
  
  ls grep shiyanlou /etc/passwd > file.txt 
  ```

##### 2. 文本编辑器vim

- 终端输入进入vim编辑器

  ```shell
  vim testfile
  ```

- vim模式：

  - 默认编辑模式：光标移动
  - 插入模式：编辑内容，按下i键进入
  - 退出：输入完成，按下ESC键返回编辑模式，输入:wq 保存退出。

##### 3. 文件压缩与解压

- 下载：

- 打包与解压

  - `-c` 建立新的打包文件
  - `-f` 指定包文件
  - `-v` 显示打包/释放的过程
  - `-z` 支持 gzip 处理文件
  - `-j` 支持 bzip2 处理文件
  - `-t` 显示打包的文件的内容
  - `-r` 添加文件到已经打包的文件
  - `-x` 从打包的文件中释放文件
  - `-C` 释放到指定目录

  ![image-20220403200851395](../../AppData/Roaming/Typora/typora-user-images/image-20220403200851395.png)

- 解压 zip和unzip:
  - `-o` 输出的文件名称，需在其后紧跟需要打包输出的文件名
  - `-q` 安静模式，不显示指令执行过程
  - `-r` 递归处理，将指定目录下的所有文件和子目录一起打包
  - `-l` 压缩文件时，把 LF 字符替换为 LF+CR 字符
  - `-e` 创建加密的压缩包

```shell
zip -r -q -o shiyanlou.zip /home/shiyanlou/Desktop
```

- 设置压缩级别为 9 和 1（9 最大，1 最小），重新打包：

```bash
zip -r -9 -q -o shiyanlou_9.zip /home/shiyanlou/Desktop
zip -r -1 -q -o shiyanlou_1.zip /home/shiyanlou/Desktop
```

- 创建加密 zip 包

使用 `-e` 参数可以创建我们经常见的需要密码打开的加密压缩包：

```bash
zip -r -e -o shiyanlou_encryption.zip /home/shiyanlou/Desktop
```

执行后输入两次密码即可创建。

`zip` 没有 `tar` 那么强大，打包和提取可以用一个命令搞定，解压的时候需要用到另一个命令 `unzip` 来解压 `.zip` 包。

`unzip` 也有几个常用的参数，如下：

- unzip常用参数：
  - `-q` 安静模式，执行时不显示任何信息
  - `-d [dir]` 指定文件解压缩后存放的目录，如果不存在将自动创建
  - `-l` 显示压缩文件内所包含的文件
  - `-O` 指定压缩包编码类型
  
- 解压其他格式 rar、7Z压缩文件：

  - 手动安装：

    ```shell
    sudo apt update
    sudo apt install p7zip-full -y
    
    //执行方法：
    7z x [filename].7z
    ```

  - 解压rar文件：免费的unrar-free应用：

    ```shell
    sudo apt install unrar-free -y
    //解压命令
    unrar -x [filename].rar
    ```

    

##### 4. 用户及文件权限

- 使用root命令：可以让当前用户以特权级别 root 运行指定的命令，但是需要当前用户属于sudo组，且需要输入当前用户的密码。

- 切换root用户：切换到root用户需要知道root密码，使用passwd为用户设置密码，普通用户则需要sudo

  ```shell
  sudo passwd root
  ```

- 设置完密码后，需要用su命令来切换。可输入man su 查看参数，这里只需要-l

  ```shell
  su -l root
  ```

- 切回shiyanlou用户，可以输入exit命令退出登录，或者直接输入 su -l shiyanlou

- chmod 改变文件或目录权限

  - 在 `chmod` 命令参数中，`u` 代表所有者，`g` 代表所属组，`o` 代表其他用户，`a` 代表所有人。

    修改 `install.log` 文件的权限设置成全部为可读、可写、可执行。

    ```bash
    chmod u=rwx,g=rwx,o=rwx install.log
    ```

  - 使用**操作符形式**修改权限

    操作符形式是在字符形式的基础上对文件或目录使用 `+/-` 操作符来设置权限。

    通过 `+` 符号增加相应的权限，`-` 符号减去相应的权限。

    ```bash
    chmod u+x,g+x,o-rw install.log
    ```

  - 使用**数字形式**修改权限
    
    ```bash
    # 修改文件权限为 `rwxrwxrwx`
    chmod 777 install.log
    # 修改文件权限为 `rwx------`
    chmod 700 install.log
    # 修改文件权限为 `rwxr-xr-x`
    chmod 755 install.log
    ```

- chown改变归属关系: 命令是change owner(改变所有人)的缩写，主要用来改变文件或者目录的所有权，语法与 chmod类似。不过需要超级用户root的权限才能执行此命令。

  ```shell
  mkdir chowntest
  ls -lh
  sudo chown root chowntest   # 修改文件的所属者为 `root`，因为是 root 用户所以需要用到 `sudo`。
  ls -lh
  ```

  ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210220-1613812959199)

  如果需要同时改变所属组，也可以直接执行：

  ```bash
  sudo chown root:root chowntest
  ```

  ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210220-1613813293827)

  这个命令和 `chmod` 命令在后续配置 JDK 和其他软件环境的时候用的比较多。

##### 5. Linux查找文件

- find

- locate、which、whereis

  - locate:

  - `locate` 命令来实现快速查找。但是与 `find` 命令直接在磁盘中查找不同，`locate` 命令会是在一个保存有系统中所有文件名和目录名的数据库 `/var/cache/locate/locatedb` 中去查找。

    注意这个数据库中的内容并不是实时更新的，一般由系统自己维护更新，所以一些当天产生的最新文件可能不能被查找到。因此我们在使用 `locate` 命令前可以手动更新该数据库，更新操作可使用 `updatedb` 命令来执行。

    `locate` 命令不属于内置的命令，所以第一次使用前需要手动安装。

    ```bash
    # 安装 locate
    sudo apt install locate
    # 更新数据库，加上 `sudo` 是为了使一些普通用户无权访问的文件可以被读取。
    sudo updatedb
    ```

    使用方式也很简单，只需要执行 `locate [filename]` 即可。如下，我们查找文件名中含有 `java` 的文件：

    ```bash
    locate java
    ```

    `locate` 还有其他常用的参数，如：

    - `-i` 忽略文件名大小写

    `locate` 命令是严格区分大小写的，可以通过一个示例来理解：

    ```bash
    touch ABCDEFG
    sudo updatedb
    locate abcdefg
    locate -i abcdefg
    ```

    可以看到，不添加 `-i` 参数是无法使用小写字母匹配大写字母的。

    ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210222-1613959077182)

  - which:

  - `which` 命令一般用于查找终端里执行的命令的文件完整路径。该命令在环境变量 `PATH` 列出的目录中搜索可执行文件或脚本进行匹配查找。

    通常我们可以用 `which` 命令来检查一个命令在系统中是否存在，以及当一个命令存在多个版本时我们执行的到底是哪个版本。

    例如在系统里存在多个 Java 的时候，我们就可以使用该命令来确认当前执行的是哪个版本的 Java。在终端里输入：

    ```bash
    which java
    ```

    就会输出当前执行的 Java 到底是哪个路径下的。

    ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210222-1613977042736)

    该命令默认只会输出当前正在使用的这个命令的路径，如果要列出所有匹配的查找结果，而不仅仅是第一个，则需要使用 `-a` 参数。

    ```bash
    which -a java
    ```

    ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210222-1613977266817)

  - whereis:

  - `whereis` 命令同 `locate` 类似，也是在一个保存有系统中所有文件名和目录名的数据库中去查找。不同的在于 `whereis` 命令只能用于程序名的搜索，而且只搜索二进制文件、man 说明文件和源代码文件。

    执行命令默认会返回上述存在的所有的内容，或者通过执行下面的参数来返回指定内容：

    - `-f`：定义搜索范围。
    - `-b`：仅搜索二进制文件。
    - `-m`：仅搜索 `man` 手册。
    - `-s`：仅搜索源。

    例如我们查看 Java 命令相关的文件：

    ```bash
    whereis java
    ```

    可以看到返回的内容里除了可执行文件外，还返回了一个 man 说明文件。

    ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210222-1613977955925)

    如果只查看 man 说明文件，则可以使用 `whereis -m java`：

    ![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210222-1613978140985)

    注意，并非所有的程序都会有输出。例如在当前环境的系统上，`cd` 命令是一个 Shell 内置的命令，执行 `whereis cd` 就不会有任何输出。

##### 6. Linux环境变量

- 环境变量的概念和种类

- env、export、source

  - 显示当前的环境变量：env 命令 

    ```shell
    env
    env | grep JAVA_HOME
    ```

  - 导出变量：export 命令

  - 使环境变量生效：source 命令

- 设置自定义环境变量：

  - 为当前用户定义一个自定义变量NEW,变量值为lanqiaoyunke（export NEW=lanqiaoyunke）

    ```bash
    export NEW										//变量
    echo $lanqiaoyunke						//值
    ;执行每次都生效,将这个命令写入shell环境变量文件里即可，这样每次开启终端都会自动读入
    tail ~/.zshrc
    ```

  - 需要写入环境变量配置文件使变量可以一直生效

    ```bash
    添加-p溴铵是当前生效的环境变量
    export -p
    ```

  - 使环境变量生效：source:

    当我们需要添加新的环境变量的时候，首先需要编辑 `~/.zshrc` ，然后在编辑完成后执行 `source ~/.zshrc` 命令而不用注销后重新登陆。

    ```bash
    
    # 注意是追加 >>，且不可写成了覆盖 `>`
    echo "export MYENV=shiyanlou" >> ~/.zshrc
    source ~/.zshrc
    echo $MYENV
    
    //source 命令还有另一个名称，一个点 .，和 source 是一样的作用，所以上面的命令也可以写成 . ~/.zshrc。
    ```

    

##### 7. Linux软件安装

- Linux下的软件安装

  - 安装：

    ```bash
    sudo apt install filename.exe
    ```

  - 卸载：

    ```bash
    sudo apt remove filename.exe
    ```



## 目录操作

- `cd`：返回自己 $HOME 目录
- `cd {dirname}`：进入目录
- `pwd`：显示当前所在目录
- `mkdir {dirname}`：创建目录
- `mkdir -p {dirname}`：递归创建目录
- `pushd {dirname}`：目录压栈并进入新目录
- `popd`：弹出并进入栈顶的目录
- `dirs -v`：列出当前目录栈
- `cd -`：回到之前的目录
- `cd -{N}`：切换到目录栈中的第 N 个目录，比如 cd -2 将切换到第二个

## 文件操作

- `ls`：显示当前目录内容，后面可接目录名：ls {dir} 显示指定目录
- `ls -l`：列表方式显示目录内容，包括文件日期，大小，权限等信息
- `ls -1`：列表方式显示目录内容，只显示文件名称，减号后面是数字 1
- `ls -a`：显示所有文件和目录，包括隐藏文件（.开头的文件/目录名）
- `ln -s {fn} {link}`：给指定文件创建一个软链接
- `cp {src} {dest}`：拷贝文件，cp -r dir1 dir2 可以递归拷贝（目录）
- `rm {fn}`：删除文件，rm -r 递归删除目录，rm -f 强制删除
- `mv {src} {dest}`：移动文件，如果 dest 是目录，则移动，是文件名则覆盖
- `touch {fn}`：创建或者更新一下制定文件
- `cat {fn}`：输出文件原始内容
- `any_cmd > {fn}`：执行任意命令并将标准输出重定向到指定文件
- `more {fn}`：逐屏显示某文件内容，空格翻页，q 退出
- `less {fn}`：更高级点的 more，更多操作，q 退出
- `head {fn}`：显示文件头部数行，可用 head -3 abc.txt 显示头三行
- `tail {fn}`：显示文件尾部数行，可用 tail -3 abc.txt 显示尾部三行
- `tail -f {fn}`：持续显示文件尾部数据，可用于监控日志
- `nano {fn}`：使用 nano 编辑器编辑文件
- `vim {fn}`：使用 vim 编辑文件
- `diff {f1} {f2}`：比较两个文件的内容
- `wc {fn}`：统计文件有多少行，多少个单词
- `chmod 644 {fn}`：修改文件权限为 644，可以接 -R 对目录循环改权限
- `chgrp group {fn}`：修改文件所属的用户组
- `chown user1 {fn}`：修改文件所有人为 user1, chown user1:group1 fn 可以修改组
- `file {fn}`：检测文件的类型和编码
- `basename {fn}`：查看文件的名字（不包括路径）
- `dirname {fn}`：查看文件的路径（不包括名字）
- `grep {pat} {fn}`：在文件中查找出现过 pat 的内容
- `grep -r {pat} .`：在当前目录下递归查找所有出现过 pat 的文件内容
- `stat {fn}`：显示文件的详细信息

## 用户管理

- `whoami`：显示我的用户名
- `who`：显示已登陆用户信息，w / who / users 内容略有不同
- `w`：显示已登陆用户信息，w / who / users 内容略有不同
- `users`：显示已登陆用户信息，w / who / users 内容略有不同
- `passwd`：修改密码，passwd {user} 可以用于 root 修改别人密码
- `finger {user}`：显示某用户信息，包括 id, 名字, 登陆状态等
- `adduser {user}`：添加用户
- `deluser {user}`：删除用户
- `w`：查看谁在线
- `su`：切换到 root 用户
- `su -`：切换到 root 用户并登陆（执行登陆脚本）
- `su {user}`：切换到某用户
- `su -{user}`：切换到某用户并登陆（执行登陆脚本）
- `id {user}`：查看用户的 uid，gid 以及所属其他用户组
- `id -u {user}`：打印用户 uid
- `id -g {user}`：打印用户 gid
- `write {user}`：向某用户发送一句消息
- `last`：显示最近用户登陆列表
- `last {user}`：显示登陆记录
- `lastb`：显示失败登陆记录
- `lastlog`：显示所有用户的最近登陆记录
- `sudo {command}`：以 root 权限执行某命令

## 进程管理

- `ps`：查看当前会话进程
- `ps ax`：查看所有进程，类似 ps -e
- `ps aux`：查看所有进程详细信息，类似 ps -ef
- `ps auxww`：查看所有进程，并且显示进程的完整启动命令
- `ps -u {user}`：查看某用户进程
- `ps axjf`：列出进程树
- `ps xjf -u {user}`：列出某用户的进程树
- `ps -eo pid,user,command`：按用户指定的格式查看进程
- `ps aux | grep httpd`：查看名为 httpd 的所有进程
- `ps --ppid {pid}`：查看父进程为 pid 的所有进程
- `pstree`：树形列出所有进程，pstree 默认一般不带，需安装
- `pstree {user}`：进程树列出某用户的进程
- `pstree -u`：树形列出所有进程以及所属用户
- `pgrep {procname}`：搜索名字匹配的进程的 pid，比如 pgrep apache2
- `kill {pid}`：结束进程
- `kill -9 {pid}`：强制结束进程，9/SIGKILL 是强制不可捕获结束信号
- `kill -KILL {pid}`：强制执行进程，kill -9 的另外一种写法
- `kill -l`：查看所有信号
- `kill -l TERM`：查看 TERM 信号的编号
- `killall {procname}`：按名称结束所有进程
- `pkill {procname}`：按名称结束进程，除名称外还可以有其他参数
- `top`：查看最活跃的进程
- `top -u {user}`：查看某用户最活跃的进程
- `any_command &`：在后台运行某命令，也可用 CTRL+Z 将当前进程挂到后台
- `jobs`：查看所有后台进程（jobs）
- `bg`：查看后台进程，并切换过去
- `nohup {command}`：长期运行某程序，在你退出登陆都保持它运行
- `nohup {command} &`：在后台长期运行某程序
- `wait`：等待所有后台进程任务结束



