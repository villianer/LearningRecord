       ### 第零章计算机概述
1.  CPU架构分为精简指令集 RISC 与复杂指令集CISC
	1. RISC：指令精简 执行时间短 复杂的指令事情需要多个指令来完成 eg：ARM
	2. CISC:CISC在微指令集的每个小指令可以执行一些较低阶的硬件操作，指令数目多而且复杂， 每条指令的长度并不相同。因为指令执行较为复杂所以每条指令花费的时间较长， 但每条个别指令可以处理的工作较为丰富。常见的CISC微指令集CPU主要有AMD、Intel、VIA等的x86架构的CPU。
2. 个人电脑常被称为x86架构的电脑
3. cpu位数代表算数逻辑单元一次可以读取的最大数据位数
4. cpu 超线程 Hyper-Threading 在每一个 CPU 内部将重要的寄存器 （register） 分成两群， 而让程序分别使用这两群寄存器。
5. 内存双通道功能：数据是同步写入/读出这一对内存中，如此才能够提升整体的带宽啊
6. 随机静态存贮器 SRAM：计算机高速缓存  DRAM:动态随机存储器 ： 计算机内存 只读存储器：ROM   READ ONLY MEMORY硬盘
7. CMOS记录主板上显卡 网卡等的启动参数
8. Kernel::系统的资源分配与管理
	1. 系统调用接口
	2. 程序管理
	3. 内存管理
	4. 文件管理系统
	5. 设备驱动
### Linux是什么与如何学习
1. 开机流程 上电 BIOS启动 检测设备cpu gpu 内存等是否正常 然后去读取设备第一个硬盘第一个扇区的MBR
2. 开机管理程序可以安装在MBR及Boot Sector两处
3. UEFI （Unified Extensible Firmware Interface）polling 轮询
4. DHCP 自动取得IP
5. linux系统种类： RHEL 收费版  Fedora 红帽子测试版 CentOs免费版 Deepin中国版 Debian Ubuntu
### Linux 磁盘与文件管理系统
1. 认识Linux文件系统
	1. 扇区Sector 为最小物理储存单位 主要有512BYTES 和4k两种
	2. 磁盘分区标注要有两种格式，一种是MBR一种是GPT
	3. 将扇区组成一个圆就是柱面Cylinder
		1. 磁盘分区后还需进行格式化format，将分区格式化为操作系统能够利用的文件系统 FAT16 NTFS  lINUX ：Ext2
		2. 一个filesystem就是一个partition
		3. LVM software raid 可以将一个分区格式化为多个文件及系统，也能够将uoge分区合成一个文件系统
		4. linux的文件系统 包括实际数据 文件权限 和文件属性（拥有者 群组  创建时间birth 访问时间access time atime  修改属性时间 mtime  修改内容时间ctime)
			1. superblock :记录此filesystem的整体信息，包括inode/block的总量 使用量 剩余量 以及文件格式相关的信息
			2. inode 记录文件属性 一个文件一个inode 同时记录此文件的数据所在的block号码
			3. block 实际记录文件的内容
			4. UEFI 
		5. df   dumpe2fs du lsblk gdisk/fdisk mkfs mount parted
### linux 文件压缩
1. tar  -c  生成新的打包文件   -f 后紧接打包文件名 -v显示处理文件的详细信息 --exclude=PATTERN 排除符合规则的文件或目录 -z 使用gzip 压缩成.tar.gz   - `j`: 使用 `bzip2` 压缩**成 `.tar.bz2` 文件**   `J`: 使用 `xz` 压缩**缩成 `.tar.xz` 文件：** -t 列出归档文件  -x 解压缩 **解压到指定目录 (推荐做法)** tar -xzvf project.tar.gz -C my_new_directory/
2. gzip zcat/zmore/zless/zgrep 
3. **xz, xzcat/xzmore/xzless/xzgrep**
4. **bzip2, bzcat/bzmore/bzless/bzgrep**
5. zip  是一个跨平台（Windows、Linux、macOS 等）的归档和压缩工具，它可以直接处理多个文件或目录，并生成 `.zip` 格式的压缩包。
	- `-r`: 递归压缩目录 (recursive)
	- `-e`: 创建加密的 zip 文件 (encrypt)
	- `-password`: 指定密码 (不推荐直接在命令行输入密码，会有安全风险)
	- unzip 解压缩
### vim
1. 一般模式 编辑模式 命令模式  i o  :  esc  wq
2. 当我们在使用 vim 编辑时， vim 会在与被编辑的文件的目录下，再创建一个名为 .filename.swp 的文件 用于意外时备份还原
3. 一般模式下G 尾部gg 跳到开头 u撤回上一步操作 ctrl+r 重做被撤销的操作
4. 删除剪切 x 删除光标字符 dd剪切/删除当前行  
5. 复制 yy 复制当前行  - 在可视模式下v 选中，然后按 `y`)：复制选中的文本。p d删除
6. `p`：粘贴到光标之后 (如果是行，则粘贴到下一行)
7. 查找与替换 (在命令模式下):
8. vim 1.txt 2.txt 同时打开两个文件：files :n :N
9. sp [filename] ctrl+w两个界面中切换
10. 自动补全
11. .viminfo记录你曾经的行为
### Basg shell
1. 变量
	1. 变量的定义与赋值 ：变量名=值 等号=两边不能有空格 变量名通常由字母、数字和下划线组成，且不能以数字开头 变量名是区分大小写的
	2. 变量的引用 `$变量名` 或 `${变量名}`
	3. 变量的类型和范围
		1.  Shell 变量 (局部变量/会话变量)
			1. 在当前 shell 会话中定义和使用
			2. 只对当前 shell 进程及其子进程（如果子进程不执行新命令，只是沿用父进程环境）可见
			3. 默认情况下，子进程不会继承父进程的 shell 变量
		2.  环境变量 (全局变量) 
			- **通过 `export` 命令定义。**、
			- 环境变量不仅在当前 shell 会话中可见，还会被继承到所有由当前 shell 启动的子进程中。
			- **用途：** 存储系统级别的配置信息，如 `PATH` (命令搜索路径), `HOME` (用户主目录), `USER` (当前用户), `LANG` (语言设置) 等。
			- env 列出目前shell环境下的所有环境变量
		3  局部变量 (函数内)
			- 在函数内部使用 `local` 关键字定义的变量，只在该函数内部有效。
			- 如果没有 `local` 关键字，在函数内部定义的变量默认为全局变量（即 shell 变量），会影响到函数外部的同名变量
		- 变量操作与扩展
			- **变量长度：** `${#变量名}`
			- **子字符串：** `${变量名:偏移量:长度}`
			- **替换字符串：**- `${变量名/旧字符串/新字符串}`：替换第一个匹配项。`${变量名//旧字符串/新字符串}`：替换所有匹配项。
		- ### 数组 (Arrays)
			- #### 索引数组
				- **定义：**COLORS=("Red" "Green" "Blue") FILES[0]="file1.txt" FILES[1]="file2.txt"
				- **访问元素：** `${数组名[索引]}`
				- **访问所有元素** `${数组名[@]}` 或 `${数组名[*]}`
				- **数组长度 (元素个数)：** `${#数组名[@]}`
		- ###  其他变量属性 (declare 命令)
			- `declare` 命令可以用来声明变量并设置其属性，它比 `readonly` 或 `export` 更通用。
			- `declare -r VAR_NAME`: 声明只读变量。
			- `declare -i VAR_NAME`: 声明整型变量。
			- `declare -x VAR_NAME`: 声明并导出变量（同 `export`）。
			- `declare -a VAR_NAME`: 声明索引数组。
			- `declare -A VAR_NAME`: 声明关联数组。
			- `declare -l VAR_NAME`: (Bash 4.0+) 自动将变量值转为小写。
			- `declare -u VAR_NAME`: (Bash 4.0+) 自动将变量值转为大写。
		- ### 删除变量 unset 变量名
		- $RANDOM） 来随机取得乱数值 0~32767
		- read
		- login, non-login shell
			- /etc/profile ~/.bash_profile 或 ~/.bash_login 或 ~/.profile
		- 万用字符 wildcard 
			- \* 代表0-无数个任意字符
			- `?`：匹配一个任意字符。
			- `[]`：匹配括号内列出的任意一个字符。[a-z]   [ ^1-9]
			- `[!...]`：匹配不在括号内列出的任意一个字符。
			- `{...,...}`：扩展成一个列表，不严格是通配符，但常用于类似的目的。
		- # \  | ; ~ $ &
		- cut -c 1,2,-3,3-,2-7  -b  -d -f --complement
		- grep -i -v -w -x -E
		- sort uniq  wc
		- tr col join paste expand
		- printf awk diff pr cmp patch pr
### Linux账号管理与ACL权限设置
- usermod    adduser   passwd userdel有效群组切换
- id groups  groupsdd  groupdel  newgrp groupmod groupwd
- ACL 允许你为任意数量的**特定用户**和**特定组**设置读(r)、写(w)、执行(x)权限。
- su 切换账户
### 磁盘配额与进阶文件系统管理
### 例行工作调度
- 自动化执行重复性任务的机制 eg:系统维护、数据备份、日志清理、报告生成、定时更新
- `cron` 是一个守护进程（`crond`），它在后台运行并检查文件，在预定的时间执行命令
	-  crontab 配置文件 `cron` 任务的配置存储在 `crontab` 文件中。主要有两种类型的 `crontab`
		- 用户corntab 
			- - **编辑：** `crontab -e`
			- - **列出：** `crontab -l`
			- - **删除：** `crontab -r` (慎用，会删除所有任务)
		- 系统 `crontab`
			- - `/etc/crontab`: 系统主配置文件，通常包含一些全局设定（如 `SHELL`, `PATH`, `MAILTO`）和定义了一些以 `root` 用户运行的系统级任务。它的格式比用户 `crontab` 多了一个字段，用于指定执行命令的用户。
			- - `/etc/cron.d/`: 目录，用于存放单个任务的 `crontab` 文件。这些文件通常由软件包自动创建，以避免冲突。格式与 `/etc/crontab` 相同 (包含用户字段)。
			- - `/etc/cron.hourly/`, `/etc/cron.daily/`, `/etc/cron.weekly/`, `/etc/cron.monthly/`: 这些目录中的脚本会分别每小时、每天、每周、每月自动执行一次。这些是方便快捷地添加周期性任务的方式，无需直接编辑 `crontab` 语法。
### 程序管理与SElinux
-  & 直接将指令丢到背景中“执行”的 &
-  将“目前”的工作丢到背景中“暂停”：[ctrl]-z
-  观察目前的背景工作状态： jobs
-  将背景工作拿到前景来处理：fg
-  让工作在背景下的状态变成运行中： bg
-  管理背景当中的工作： kill -9强制删除一个不正常的工作/-15 以正常的步骤结束一项工作 -1 重新启动一个进程 重新读取配置文件 
- ps -l   ps aux  top 想要找出最损耗 CPU 资源的那个程序时多使用的就是 top 这支程序啦！然后强制以 CPU 使用资源来排序 （在 top 当中按下 P 即可
-  pstree
- 进程优先级 新执行的指令立马给一个nice值nice nice [-n 数字] command   - renice ：已存在程序的 nice 重新调整：renice [number] PID
- free updatetime netstat





