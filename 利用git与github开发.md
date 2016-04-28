# git版本管理

## 安装git
* [下载](https://git-scm.com/)git安装文件
* 具体安装步骤都是下一步的工作了

## git常用命令介绍
### git init 初始化本地仓库	
### git add <file> 添加文件到暂存区
* file 文件可以是具体的文件或者文件夹（可多个）
* file 如何是点号（.）表明添加所有文件

### git commit
* git commit -m '提交的信息'
* git commit --amend => 修改提交的信息
* git commit  => 这个时候直接打开一个vi的编辑状态，正常第一行键入提交的信息 空一行添加具体的提交细节。如果想终止提交，可将信息提交信息留空退出即可

### git status 查看当前仓库的状态
### git log 查看仓库提交的日志
* git log --pretty=short 减少日志的打印
* git log <file>  打印file文件(夹)的日志
* git log -p 打印提交前后的改动，一般结合具体的文件查看
* git log --graph 图标形式查看分支

### git diff 查看更改前后的差异
* git diff 查看工作区与最新提交的区别
* git diff HEAD 查看暂存区与最新提交的区别

### git branch 查看分支列表
* master 一般表示主分支
* master前面的 * 表示当前所在的分支
* git branch -d | -D branchname 删除branchname分支
* git branch -d -r branchname 删除远程branchname分支
	
```bash
$ git branch
* master
```

### git checkout 创建分支与切换分支
* git checkout -b 创建并且切换到新分支下
* git checkout branchname 切换到branchname分支下

```bash
$ git checkout -b newbranchname
```
### git merge 合并分支
* 切换到合并的分支(一般是master)
* git merge --no-ff branchname
* 一般要主要是否有冲突

### git reset --hard <hashHEAD>
### git reflog 查看git的操作日志,方便于`git rest --hard`合作进行版本穿梭
### git rebase -i 压缩历史

```bash
// 最新的2次提交
$ git rebase -i HEAD~2
// 将需要被合并的记录pick修改成fixup
```
### git pull 从远程仓库更新到本地
```bash
$ git pull orgin [branchname]
```

### git push 推送到远程仓库
```bash
// 添加远程仓库
＄ git remote add origin git@github.com:[username]/[reposityname].git
// 推送信息
$ git push -u orgin master
// 推送其他仓库
￥ git push -u origin [branchName]
```
## 结合github的使用
### 注册github账号

### 维护自己的仓库
* git clone https://github.com/[username]/[reposityname].git
* 修改代码
* git add
* git commit
* git push
* ...

### 维护别人的仓库
1. fork目标仓库到自己仓库下
2. clone 自己仓库代码到本地
3. 创建分支,主要用于修改或者添加的别人仓库中没有的功能
4. 获取原始仓库最新代码并合并到新分支下
5. push到自己的仓库下的分支
6. 切换分支发送PR等待通知

```bash
$ git remote add [newName] [远程仓库地址]
$ git fetch [newName]
$ git merge [newName]/[branchname]
```

## 相关资料
* [https://github.com/xirong/my-git/blob/master/why-git.md](https://github.com/xirong/my-git/blob/master/why-git.md)
* [http://stackoverflow.com/questions/871/why-is-git-better-than-subversion](http://stackoverflow.com/questions/871/why-is-git-better-than-subversion)
* [http://www.worldhello.net/2012/04/12/why-git-is-better-than-svn.html](http://www.worldhello.net/2012/04/12/why-git-is-better-than-svn.html)
