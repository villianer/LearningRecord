## 内核简介
1. 8位CPU 线 6个内部寄存器 80条基本指令 20种寻址模式 可以寻址6个内部寄存器和16Mbytes 内存和外部寄存器
## CPU寄存器
6个CPU内部寄存器
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-27%20215148.png)
	1. accumulator: 累加器 8位 用于保存算术运算 逻辑运算以及数据操作的操作数及结果
	2. index X/Y：引索寄存器 16位 可实现高效率的寻址模式，也可用作数据操作的暂存器。在大多数情况下，交叉汇编器会在使用了Y寄存器的指令代码中生成precod指令，用于和使用了X寄存器的指令相区别。
	3. PC:程序寄存器 24位 用于存储CPU下一条要执行指令的地址。在每次指令执行后自动刷新，由于他有24位，所以STM8内核有16Mbyte (2^24)byte的寻址空间
	4. stack Pointer(SP):堆栈寄存器 16位 包含堆栈下个空位的地址，堆栈用于在子程序调用或中断时保存CPU内容
	   ![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-27%20224910.png)
	5. 全局配置寄存器 CFG_GCR  低功耗CPU专有的寄存器  用于控制低功耗模式  是一个内存映射寄存器 他控制处理器的配置 他包含AL控制位 AL：激活等级 如果AL位置哦（主要），IRET指令将会使CPU寄存器的值从堆栈中恢复，然后main程序继续执行在WFI指令之后。如果AL位置1，IRET会导致CPU回到WFI/HALT模式。 在低功耗应用场合 MCU在大多数时间在WFI/HALT模式，在冲断发生时再去执行一些特定的任务，有一些重复的任务非常简单可以直接在ISR中断服务子程序中处理完成，在执行前将AL置1就可以在程序执行完ISR 后直接回到WFI/HALT模式。
	6. 条件代码寄存器 condition code register条件代码寄存器是一个8位寄存器，用于指示刚被执行的指令结果以及处理器的状态。寄存器的第7位MSB使保留位，这些位可以被用户的程序或者代码单独的测试，测试结果可用于指示程序或代码执行后的状态。
	 ![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-27%20234744.png)
	  V: 置1表示最后一次数据运算溢出
	  I1 I0 中断屏蔽等级 
	  ![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-27%20235246.png)
	  H:半进位标志 在ALU第四位发生借位时或溢出时置1 在BCD码运算子程序中很有用
	  N: 置1 指示最后一次运算的结果为负
	  Z：置1表示最后一次数学运算 逻辑 数据操作的结果为零
	  C：进位标志位 表示ALU有借位或者进位
	  程序空间

## 数据空间

![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-28%20003639.png)
记忆体结构
STM8 使用的是哈佛结构  程序总线与内存总线是分开的 可是逻辑地址空间是一致的 所有的内存共享16Mbyte空间 由两个总线组成 数据总线和地址总线 R/W 读/写控制信号 STALL记忆体应答信号
STALL使得CPU可以与一些低速的串行或并行记忆体接口兼容，当记忆体接口慢的时候CPU在执行指令前等待STALL
程序存储总线为32位，允许在1个周期内拿到大多数程序指令
由于地址空间是一致的，这种结构允许数据被存储到Flash中，程序可从RAM中获取
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-28%20005534.png)

# STM8 指令流水线

STM8家族使用三级流水线
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-28%20010350.png)![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-04-28%20010540.png)	