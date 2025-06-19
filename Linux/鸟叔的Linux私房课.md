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
