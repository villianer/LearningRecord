git 是一个分布式版本管理工具  SVN 是一个集中式管理工具 不同点在SVN项目在server端 每个用户只能获取项目的一部分 git则是每个人都能够获取全部项目

[博客园](https://www.cnblogs.com/syp172654682/p/7689328.html)
git 下载安装 git 官网

Linux 命令

gitee github 远程仓库建立 使用SSH配置公钥.SSH文件

git config 配置

git config --global user.name "runoob"
git config --global user.email test@runoob.com

必须配置这两个
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-03-21%20145227.png)

## 建立本地git repository
1.  新建本地仓库并链接到远程仓库
	- git init 初始化本地仓库
	- git remote add [远程仓库名称]  [SSH地址]
	- git remote -v 查询链接到的远程仓库信息
	- git remote -d [remote repository name]删除远程链接到的远程仓库本地信息
	- git push [remote repository name] 将本地仓库推送到远程仓库  如果分支名不同则在远程仓库建立一个新的branch
	- git pull [远程仓库名称] <远程分支名称>:<本地分支名称> 将远程仓库分支与本地仓库分支合并
	- git push <远程主机名称> <本地分支名称>:<远程分支名>
	- 如果本地版本与远程版本有差异但是又要强制推送可以使用--force参数 
	- git fetch <远程仓库名称>  git merge <远程仓库名称>/<远程分支名称>将远程仓库分支合并到本地仓库分支
	
	

 2. 直接clone远程仓库 git clone [url]
 
## git 四个工作区
	1. workplace
	2. stage
	3. local repository
	4. remote repository
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-03-21%20104108.png)

## git 基本操作 还原误操作

	1. 删除workspace中的文件 git rm <文件名>
	2. add commit push pull fetch merge rm diff diff HEAD checkout  checkout HEAD
	3. git rm git rm --cached
	4. git diff --cached
	5. 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master
	分支会做相更新。即 master 指向的目录树就是提交时暂存区的目录树。

当执行 “git reset HEAD” 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。

当执行 “git rm –cached <file>” 命令时，会直接从暂存区删除文件，工作区则不做出改变。

当执行 “git checkout .” 或者 “git checkout — <file>” 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。

当执行 “git checkout HEAD .” 或者 “git checkout HEAD <file>” 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改 动。


## git branch
	1. 查看远程仓库分支 git branch -a 


## 撤销提交
git reset --hard HEAD~  将HEAD往上移
git stash 保存当前分支所有工作状态
git stash list
git stash pop恢复


