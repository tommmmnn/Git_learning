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
```
$ git status
```
- 追踪文件
将文件存到暂存区，文件状态从**untracked**变成**new file**。若在编辑之后没使用`add`则在暂存区的还是之前的版本。
```
$ git add <filename>        将文件加入暂存区
$ git add *.<filetype>      将该后缀名的文件全部加入暂存区
$ git add --all             将文件全部存入暂存区
```




