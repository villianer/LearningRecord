1. 创建本地仓库
	- 直接在本地创建新的仓库  git init
	- 从server clone  == git clone [url]==
	- 添加远程仓库 首先配置SSH  添加到远程仓库中  git remote add [远程仓库地址] SHH地址
1. 文件的四种状态
	- untracked :此文件在文件当中，但是未经add ,只存在工作区不存在其他地方 add 后变为Staged
	- unmodified : 文件同时存在与服务端与工作区且完全一致 ，如果被修改就变为modified，如果本地库 git rm 删除 则变为untracked
	- modified 文件已经修改 ，仅仅是修改过，未进行任何其他动作，本地库存在同名但是内容未修改过的文件 git add 进入 Staged   Git checkout则放弃 修改变为unmodified
	- Staged  暂存状态 所有add后的文件进入   
	- git status command 查看文件状态
2.  git 常用指令
	- git add . 提交所有工作区文件到暂存区当中
	- git commit -m 提交暂存区文件到本地repository     - m提交的消息
3. 忽略文件
	- 如有部分文件不想上传 ，可以使用忽略，在主目录下建立.gitignore 文件
	- 忽略文件中以#号开头的行会被忽略
	- 可以使用Linux通配符 /*  /? [abc]  
	- !表示除外
	- 名称随后一个是路径分隔符（/）表示护士此目录下 子目录不忽略
	- /在前 表示只忽略 当前目录下的文件而不管子目录下的
4. 设置SHH密匙 实现本机免密登录
5. git stash
6. git@gitee.com:orange-huang/gitpractice2.git