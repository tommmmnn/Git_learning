# Git学习笔记
------
### Git Bash终端常用命令
Git Bash是模仿linux系统的终端，因此使用的命令与linux系统相同。
```
cd      切换目录
pwd              获取当前所在位置
cd ..         上层目录
ls           列出当前的文件列表
mkdir <dir>       创建新目录
cp <filename> [filename]      复制文件[重命名]
mv <filename> <filename>      文件更名
rm <filename>        删除文件
echo "code" > <filename>        添加code到文件中
clear         清屏
Ctrl+P       上一条命令
vim <filename>      用vim打开文件
#vim中
i       进入编辑模式
ESC     退出编辑模式
:wq     保存并退出（命令模式下）
```

### 配置Git
可以通过以下命令修改配置：
```sh
$ git config --global user.name "Tom"                   配置用户名
$ git config --global user.email "hwhuang00@163.com"    配置邮箱
$ git config --list         查看配置
$ git config --global core.editor <editor>      更换编辑器
$ git config --global alias.co checkout     设置缩写（co代替checkout）
$ git config --global alias.<string> ""    string命令代替""中的命令
```
或者进入`~/.gitconfig`里编辑。

### Git使用
- 初始化
进入目录后，使用如下代码让Git开始对目录进行版本控制，若想取消版本控制只需删除`.git`文件即可。
```sh
$ git init
```
- 查看状态
```sh
$ git status
```
- 追踪文件
将文件存到暂存区，文件状态从**untracked**变成**new file**。若在编辑之后没使用`add`则在暂存区的还是之前的版本。
```sh
$ git add <filename>        将文件加入暂存区
$ git add *.<filetype>      将该后缀名的文件全部加入暂存区
$ git add --all             将文件全部存入暂存区
```
- 文件存档
将暂存区的内容存储到存储库里，成为一个版本。信息描述用于说明改动。
```sh
$ git commit [--allow-empty] -m "msg"    存档且添加msg说明，加[]中的内容则允许不添加信息
$ git commit -a -m "msg"        一步commit已经存在存储库中的文件
```
- 查看记录
可以查看`commit`的作者以及什么时间做了什么事。并且显示其识别码。
```sh
$ git log
$ git log --oneline --graph     只显示单行
$ git log --oneline --author="Tom"      查找某人的commit
$ git log -S "Here"       从commit的内容中查找
$ git log --oneline --since="9am" --until="12am"        查找某一段时间的commit
```
- 删除文件
Git对删除文件的改动和修改文件是一样的，使用`rm`命令删除文件之后（若想删除目录则在rm后加入-r），需要再使用`add`和`commit`来彻底删除文件。或者使用`git rm`直接将文件放入暂存区。若只是想让文件脱离Git控制而不删除，可以添加`--cached`。
```sh
$ git rm <filename>
$ git rm <filename> --cached
```
- 文件改名
使用`mv`命令改名时，对于Git来说是先删除原文件，再新建一个文件，因此新文件是**untracked**状态。需要使用`add`命令重新添加至暂存区,其状态会变成**renamed**。或者使用`git mv`一步将其改名并移至暂存区。
```sh
$ git mv <filename> <new filename>
```
- 修改commit记录
```sh
$ git commit --amend -m "msg"       修改最后一次commit的信息
$ git commit --amend --no-edit      将文件并入上一次的commit（要先放到暂存库里）
```
- 新目录交给git管理
在创建新目录时，若里面没有文件则git会无法识别，若想让git识别可以加个`.keep`或`.gitkeep`的空文件。
```
$ touch <dir>/.keep
```
- 让文件不受Git管理
可以通过在目录里添加`.gitignore`文件，并在文件中配置哪些文件需要git忽略。
```
# 忽略secret.yml 文件
secret.yml
# 忽略config目录下的database.yml文件
config/database.yml
# 忽略db目录下所有后缀是 .sqlite3的文件
/db/*.sqlite3
# 忽略所有后缀是 .tmp的文件
*.tmp
```
```
$ git clean -fx     清除被忽略的文件
```
- 查看文件的commit记录
```sh
$ git log <filename>        查看文件的commit记录
$ git log -p <filename>        查看文件的commit时做了什么
$ git log --graph         以图表形式输出日志
$ git reflog            查看所有历史日志
```
- 查看修改者做的事
```
$ git blame <filename>      查看某一行代码是谁写的
$ git blame -L 5,10 <filename>      查看5-10行代码是谁写的
```
- 找回删除的文件
```sh
$ git checkout <filename>       找回rm的文件
$ git checkout .                找回所有rm的文件
$ git checkout HEAD~2 <filename>        用上两个版本的文件覆盖当前目录中的文件
```
- 拆掉重做
`reset`命令可以搭配`--mixed`、`--soft`、`--hard`，分别把拆出来的文件放回工作目录、放回暂存区和直接删除。相当于去到某个`commit`，但是会把拆出来的文件放到不同位置保存。SHA-1可以通过`git log`查找，也可以通过`git reflog`查找，通过后者可以把之前`--hard`删除的找回。
```
$ git reset <SHA-1>^     回到前n次commit（n为^的数量）
$ git reset master^
$ git reset HEAD^
$ git reset <SHA-1>      回到编码指向的commit，可以往前也可以往后
```
- commit某部分内容
```
$ git add -p <filename>
$ e                         弹出命令时输入，把不想commit的部分删除
```
- SHA-1解析
**SHA-1**是由算法计算出的编号，针对相同的输入有相同的输出，不同的输入有不同的输出，例如：
```sh
$ printf "Hello, 5xRuby" | git hash-object --stdin
```
该命令会输出字符串所对应的SHA-1编码，该编码只与输入有关，与时间等无关。Git中有四种对象：`Blob`、`Tree`、`Commit`、`Tag`，不同对象编码方式略有差异。
```sh
$ git cat-file -t <SHA-1>     输出SHA-1代表对象的类型
$ git cat-file -p <SHA-1>     输出SHA-1指向对象的内容
```
