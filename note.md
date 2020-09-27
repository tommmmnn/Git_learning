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
Git对删除文件的改动和修改文件是一样的，使用`rm`命令删除文件之后，需要再使用`add`和`commit`来彻底删除文件。或者使用`git rm`直接将文件放入暂存区。若只是想让文件脱离Git控制而不删除，可以添加`--cached`
```sh
$ git rm <filename>
$ git rm <filename> --cached
```
- 文件改名
使用`mv`命令改名时，对于Git来说是先删除原文件，再新建一个文件，因此新文件是**untracked**状态。需要使用`add`命令重新添加至暂存区,其状态会变成**renamed**。或者使用`git mv`一步将其改名并移至暂存区。
```sh
$ git mv <filename> <new filename>
```







