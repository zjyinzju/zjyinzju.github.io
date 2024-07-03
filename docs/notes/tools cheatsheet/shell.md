
> 用户和系统交互的接口


> 主要是bash的一些命令


## 路径相关
命令提示符会显示一些必要的信息诸如当前路径、用户权限，带有`~`的路径是当前用户的home目录

在Windows下路径分隔符是`\`，在*nix下则是`/`；
Windows下有多个根目录如`C:\`、`D:\`。

+ `pwd`：显示当前路径
+ `cd`：切换路径
    + `cd ~`：切换到home目录
    + `cd ..`：切换到上一级目录
    + `cd /path/to/dir`：切换到指定目录
    + 按`Tab`键可以自动补全路径

## 文件相关
+ `ls`：列出当前目录下的文件
    + `ls -l`：列出详细信息
        - `ls-lh`：以人类可读的方式显示文件大小
    > -本用户-本组其他用户-其他组用户-
    + `ls -a`：列出所有文件（包括隐藏文件）
+ `touch`：创建文件
+ `mkdir`：创建文件夹
+ `rm`：删除文件
    + `rm -r`：删除文件夹及其内容
    + `rm -f`：强制删除
+ `cp` src dst：复制src到dst
    + `cp -r` src dst：复制文件夹
+ `mv` src dst：移动src到dst，效果是重命名
+ `find`path -name filename：在path下查找文件名为filename的文件
+ `cat`：查看文件内容
    + `-n`：显示行号
        - `-n x`: 查看第x行
+ `tail`: 查看文件末尾内容
    + `-n x`：显示最后x行
+ `more`/`less`：分页显示文件内容
    + `q`：退出
    + `空格`：下一页
    + `b`：上一页

## 其他一些
+ `echo`：输出
    + 和重定向配合使用可以写入文件
+ `date`：显示当前时间
+ `clear`：清屏
+ `man`：查看命令的帮助文档
    + `man -k`：搜索命令
+ `whoami`：显示当前用户

## 重定向
> 即文件流重定向，在shell中有三种流：stdin、stdout、stderr(标准输入、标准输出、标准错误)

+ `>`：重定向输出，无则创建，有则覆盖；追加则使用`>>`
+ `<`：重定向输入
+ `2>`：重定向错误输出
+ `&>`：重定向所有输出

### 常见用法
+ `echo "hello" > file.txt`：将"hello"写入file.txt
+ `cat file.txt > file2.txt`：将file.txt的内容写入file2.txt
    + `cat > file.txt`：从键盘输入内容写入file.txt，按`Ctrl+D`结束流
+ `diff file1.txt file2.txt > diff.txt`：将file1.txt和file2.txt的差异写入diff.txt
+ `./a.out < input.txt `：将input.txt的内容作为程序的输入

## 管道
> 将一个命令的输出作为另一个命令的输入

+ `|`：将左侧stdout的内容传递给右侧stdin

一些常见用法如：
+ `some command | tail -n lines`: 输出最后lines行
+ `some command | grep keyword`: 查找包含keyword的行
+ `some command | less`: 分页显示输出
+ 和cut、sort、uniq等命令配合使用处理文本数据

复杂命令可以查看[这个网站](https://explainshell.com/)

## 环境变量
> 用于存储系统的配置信息

+ `env`：显示所有环境变量
+ `echo $var`：显示某个环境变量
+ `export var=value`：设置环境变量
+ `unset var`：删除环境变量，或者用`export var= `清空