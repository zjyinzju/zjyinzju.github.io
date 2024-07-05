# Git
> 分布式版本控制系统

> 官网：[https://git-scm.com/](https://git-scm.com/)

Git包含本地（ local ）仓库和远程（ remote ）仓库。

本地仓库分为工作目录（ working directory ）和 git reposity。工作目录实际上就是项目文件等（ 由`ls`命令可以直接查询到 ）；而git仓库则是由`git init`命令创建的（ 亦即 *.git* 文件夹 ），其中包含了暂存区（ stage ）和一个版本库（ commit history ）等（ 由`ls -a`命令可以查询到 ）；


Git 的基本工作流程如下：

1. 在工作目录中修改文件；
2. 将修改的文件添加到暂存区（通过 `add` ）；
3. 将暂存区的文件提交到版本库（通过 `commit` ）；
4. 将版本库的文件推送到远程仓库（通过 `push` ）。
5. 对方可以拉取远程仓库的文件到自己的本地仓库（通过 `pull` ）。

## 基础配置
+ 创建本地git版本库：
    + `git init`：将当前路径下的文件夹变成git版本库（在当前文件夹内创建一个 *.git* 文件夹，`ls -a`可以看到）
    + `git init [project-name]`：创建一个新文件夹，将其初始化为git版本库
+ 配置用户信息：
    + `git config --global user.name "Your Name"`：设置用户名
    + `git config --global user.email "email"`：设置邮箱
    > `--global`参数表示所有的git仓库都会使用这个配置，如果想要针对某个仓库使用不同的用户名和邮箱，可以去掉`--global`参数，然后在该仓库下执行相同的命令
+ 查看配置信息：
    + `git config --list`：查看所有配置信息
    + `git config user.name`：查看用户名
    + `git config user.email`：查看邮箱

## 基础用法
+ 将工作目录中的文件加入暂存区：
    + `git add [file]`：将指定文件加入暂存区
    + `git add .`：将所有文件加入暂存区
    > 只会添加修改过的文件
+ 删除文件：
    + `rm`只是将文件从工作目录中删除，但是不会从版本库中删除
    + `git rm [file]`会将文件从版本库和工作目录中删除
    + `git rm --cached [file]`会将文件从版本库中删除，但是保留在工作目录中（即：将一个已经暂存的新文件取消暂存
+ 重命名文件：
    + `git mv [old-file] [new-file]`：重命名文件（等价于 `mv` + `git rm` + `git add`）
+ 查看文件状态：`git status`
    + *Untracked files*：工作目录中新建或修改过但是没有加入暂存区的文件
    + *Tracked files*：已经加入暂存区的最新文件
    + *Ignored files*：本地有但是不会加入版本库的文件
??? note ".gitignore"
    存放在工作目录中的`.gitignore`文件可以指定不需要加入版本库的文件。

    语法规则不详述，可以参考[官方文档](https://git-scm.com/docs/gitignore)以及[github/gitignore](https://github.com/github/gitignore)

    通过`git check-ignore -v [file]`可以查看文件是否被`.gitignore`忽略。

+ 提交文件到本地版本库：
    + `git commit` : 进入默认编辑器（如vim）编辑提交信息
    + `git commit -m "message"`：不开启编辑器，直接提交
    + `git commit -a -m "message"`：跳过暂存区，直接提交
+ 查看提交历史：
    + `git log`：查看提交历史
    + `git log --oneline`：分行
    + `git log --graph`：查看提交历史的分支图
    + `git log --pretty=oneline`：查看提交历史的详细信息
    + `git log --stat`：查看提交历史的简略信息
    + `git log -p`：显示详细的修改内容
    > 每个提交都有一个唯一的SHA-1哈希值（40位 16进制数），可以通过`git show [SHA-1]`查看提交的详细信息，如果不重复的话，可以只写前几位
    + `git checkout [SHA-1]`：切换到指定的提交
??? note "commit message"
    提交信息的意义在于记录更改原因和内容，方便定位和回溯（尤在合作项目中）

    有如下的 *Angular* 规范 (源自于[angular/angular:CONTRIBUTING.md](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md))：

    ```html
    <type>([scope]): <summary>
    [body]
    [footer]
    ```
    其中：
    
    + `type`：表示更改的类型，如`feat`（新功能）、`fix`（修复bug）、`docs`（文档）、`style`（格式）、`refactor`（重构）、`test`（测试）、`chore`（构建工具、库或辅助工具的变动）、`perf`（性能优化）、`ci`（CI配置）等
        + 重大更改可以写`BREAKING CHANGE`或`DEPRECATED`
    + `scope`：表示更改的范围，如`core`、`compiler`、`common`、`forms`、`http`、`router`、`upgrade`等
    + `summary`：简要描述更改的内容，英文一般现在时，首字母小写，结尾不加句号
    + `body`：详细描述更改的内容
    + `footer`：解决了哪个issue，或者是一些关于更改的链接等

