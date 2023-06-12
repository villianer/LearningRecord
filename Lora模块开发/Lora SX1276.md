### 产品简化区块图
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20134511.png)
从中大概可以知道芯片可以分为Lora 模式和FSK/OOK模式，两个模式都有独立的调制解调器，产品通过SPI口通信，SPI可以访问FIFO以及confige register.信号经调制后通过功率放大器PA经PA_BOOST RF0_LF RFO_HF输出，信号经RFI_IF口 RFI_OF输入，经过滤波 ADC装换后解调最后存储到FIFO中。

### SX1276/77/78区别
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20134738.png)
### PIN Diagram
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20135605.png)

### PIN 描述
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20135800.png)
这里我们只需要掌握 SPI 接口 SCK MISO MOSI NSS GND 用于与单片机通信，中断输入接口 D1O0 D1O1 D1O2 D1O3 D1O4 D1O5 在消息发送完毕或接收完毕时会产生中断信号告诉单片机，具体作用后面介绍  NRESET 软件复位接口 拉低电平100ms可以使芯片复位 ，这些接口要与单片机相连。
XTA XTB 为外部晶振接口 32MHz  其余为供电口，以及射频信号输入输出口

### Lora 模式芯片默认参数
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20140752.png)

### 扩频因子 
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20141222.png)
关于 扩频因子SF 带宽BW 符号速率$R_s$ 比特速率 $R_b$ 纠错码Cr之间的关系Lora 脉宽调制中有详细讲解 数据手册没有过多讲解

### Lora 数据包结构
Lora 数据包分为 显式explicit和隐式implicit ，隐式数据包包括前导码preamble 和 有效荷载 payload 以及payload  CRC校验码，显式数据包多了一个header header 包括有效荷载长度 CR CRC是否开启等信息
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20142440.png)

#### 前导码
前导码用于将接收端与传入数据流同步。缺省情况下，报文的长度为12个符号 即12个symbol。这是一个可编程的变量，因此序言长度可以延长，例如在接收密集应用中减少接收码的占比。但是，最小长度足以满足所有通信。传输的序文长度可以通过设置寄存器preamblelth从6到65535来改变，总序文长度为6+4到65535+4个符号的总序文长度。

#### 报头
隐式或者显式报头可以通过 RegModemConfig1寄存器中的ImplicitHeaderModeOn位来设置
显式报头模式为默认 报头信息包括 有效负载长度 纠错率 是否打开16CRC 报头发送最大纠错码(4/8)。它也有自己的CRC，允许接收方丢弃无效的标头
##### 隐式报头模式 
在特定情况下 如果有效负载长度 编码率 以及CRC是否以及CRC是打开等已知，可以使用隐式报头来缩短发送时间
在隐式报头模式下，有效负载的CRC是否启用只取决于发送方 RegModemConfig1寄存器中的RxPayloadCrcOn位 一旦接收方接收到数据，因该立即检查RegHopChannel寄存器中的CrcOnPayload，他表示了发送方是否使用了CRC,一旦发送方使用了CRC那接收方就应该保持PayloadCrcError中断开启
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20145425.png)

##### 显式报头模式
接收方以及发送方的RxPayloadCrcOn位都需要配置
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20145642.png)

##### 低速率优化
在低速率时可设置LowDataRateOptimize 进行优化，提高信号的鲁棒性，在符号持续时间超过16ms时必须使用

#### 有效荷载
有效负载是一个长度不固定的字段，实际长度和纠错率 CRC是否开启由显式模式下报头或者隐式模式下寄存器决定。
##### 空中速率
LoRa报文的持续时间是前导码的持续时间和传输报文的持续时间之和，前导码持续时间由下面的公式计算
$$ T_{preamble} = (n_{preamble}+4.25)T_s$$
$T_{preamble}$ 是设定的前导码长度  取自寄存器RegPreambleMsb和RegPreambleLsb  有效载荷持续时间取决于所启用的报头模式
下面的公式给出了有效载荷符号的数量
$$ n_{payload} = 8 + max(ceil(\frac{8PL-4SF+28+16CRC-20IH}{4(SF-2DE)}(CR+4),0)$$

PL : Payload长度 单位byte （1-256）
SF : spreading factor （6-12）
当有报头时IH=0 当没报头时IH=1
DE=1 when LowDataRateOptimize=1 , DE=0 otherwise
CR (4/5-4/8)
有效荷载的持续时间等于符号周期乘以$n_{payload}$
$$T_{payload} = n_{payload} *T_s$$
空中时间或者说数据包持续时间简单地说就是前导码和有效负载持续时间的总和
$$T_{packet} = T_{preamble} + T_{payload}$$
### Lora跳频
当单个数据包时间可能超过相关法规定的最大信道停留时间，一般采用跳频扩频技术FHSS 。要使用FHSS 技术，可以设置寄存器RegHopPerio的FreqHoppingPeriod位来开启跳频模式
#### 操作原理
FHSS方案背后的原理是，每个LoRa®数据包的一部分从由主机微控制器管理的频率查找表中在每个跳频信道上传输。在预定的跳频周期之后，发送器和接收器切换到预定义的跳频列表中的下一个信道，以继续发送和接收数据包的下一部分。传输在任何给定信道中停留的时间由 FreqHoppingPeriod确定，。是符号周期的整数倍
$$HoppingPeriod = Ts * FreqHoppingPeriod$$
跳频发射和接收过程从信道0开始。前导码和报头首先在信道0上传输。在每次传输开始时，通道计数器fhsspretchannel(位于寄存器中)  （RegHopChannel)增加，并产生中断信号FhssChangeChannel。必须在跳频周期内设定新频率，以保证下次跳频时会覆盖该新的频率，之后再写入“1”从而将中断信号（ChangeChannelFhss)清除
FHSS接收总是从通道0开始。接收器在开始如上所述的跳频过程之前等待有效的前导检测。注意，在报头CRC损坏的情况下，接收方将自动请求通道0并重新开始有效的前导检测过程
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-25%20154604.png)

### Lora 调制解调器有三种数字接口
1. 静态配置寄存器
2. 状态寄存器
3. FIFO数据缓存
### Lora 配置寄存器
配置寄存器可通过SPI访问，寄存器在任何设备模式下均可读，但是只有在睡眠和待机模式下可写。
### 状态寄存器
状态寄存器在接收机运行过程中提供状态信息
### Lora 模式FIFO数据缓存
除了睡眠模式其他操作模式均可读
只有在待机模式下才允许写入
详情见Datasheet
器件被设置为睡眠模式时 FIFO被清空
器件被切换到Lora 其他操作模式时FIFO数据缓存中的数据则保持不变
当一组新的数据被写入已被占用的缓存单元时，FIFO数据缓存不能自行清空（除非器件被设置成睡眠模式）原有的数据只是覆盖这些数据
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-24%20142447.png)
FIFO大小256byte
RegFifoTxBaseAddr 要发送信息的起始位置 默认0X80
RegFifoRxBaseAddr 接收操作中写入缓存的起始位置 默认0x00
通过SPI接口读取或写入当前数据的FIFO数据缓存单元由指针RegFifoAddrPtr 定义因此在写入或读取操作前，必须将该指针初始化为对应的基地址。FIFO数据缓存（RegFifo)读取数据或向FIFO数据缓存写入数据后地址将自动增值
在成功完成数据接收操作时，寄存器RegRxNbBytes 会定义待写入数据的所占缓存单元的大小
RegPayloadLength 则显示待发送数据所占用的缓存单元的大小。
在隐式报头模式下，寄存器RegRxNbBytes是无效的，因为此时由效负载字节数必须是已知的，而显式包头模式下，寄存器RegFifoRxCurrentAddr显示最后接收的数据包在FIFO数据缓存中的存储位置，因此通过将寄存器RegFifoAddrPtr指向寄存器RegFifoRxCurrentAddr即可轻松读取该数据包
### Lora 调制解调器操作模式
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-24%20145456.png)
### 数据发送序列
#### 发送模式
在发送模式下，仅在需要发送数据包数据的时候才会启动射频模块、PLL模块、以及PA模块 这样可以优化功率消耗
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-24%20151443.png)
#### 发送模式
数据接收序列
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-24%20151655.png)
1. 单一模式
在这种模式下调制解调器在给定的时间窗口内搜索前导码。如果在该时间窗口结束时还未找到前导码，则芯片会产生Rxtimeout中断信号并切换回待机模式。时间窗口长度由RegSymbTimeout寄存器定义，必须为4（调制解调器获取preamble锁的最短时间）到1023个symbol ,缺省值为5。如果窗口时间未发现前导码，则会产生RxTimeout中断信号，同事芯片自动切换回待机模式。
在有效负载结束时，如果负载CRC无效，则会产生RxDone中信号及PayloadCrcError中断信号。然而，即使CRC无效任然可以在FIFO数据缓存中写入数据，以便后续进行处理。RXDone中断产生后芯片切换回待机模式。
3. 连续接收模式
RegPreambleMsb和RegPreambleLsb设置前preamble的长度
为了从FIFO数据缓存检索接收数据，用户必须保证状态寄存器RegirqFlags中VaildHeader PayloadCrcError RxDone 以及Rxtimeout等中断信号未意外生效，以确保数据包接收成功终止（即不应设置任何标志位）
每次接收到RxDone中断信号时，锁存变量start_address中的RegFifoRxByTeAddr寄存器的内容，寄存器实时显示由Lora接收调制解调器写入数据缓存的最后一个字节的地址+1通过这种方式可以保证变量start_address总是包含下一个数据包的起始地址
### [SX1278中断详解](https://blog.csdn.net/weixin_44481398/article/details/89397743?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-89397743-blog-89597513.235%5Ev36%5Epc_relevant_default_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-89397743-blog-89597513.235%5Ev36%5Epc_relevant_default_base3&utm_relevant_index=10)
### CAD 信道活动检测模式[](https://blog.csdn.net/qq_15391889/article/details/81050643?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168492584916800192298992%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168492584916800192298992&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-81050643-null-null.142^v87^koosearch_v1,239^v2^insert_chatgpt&utm_term=cad%E4%BF%A1%E9%81%93%E6%B4%BB%E5%8A%A8%E6%A3%80%E6%B5%8B&spm=1018.2226.3001.4187)

1.在无线传感器网络设计中，无线收发机的节能是一个非常关键的问题。为进一步减小功耗，只有通过减少无用的工作时间。无线通信时，射频大部分处于接收状态，也是主要的能量消耗所在。当无线传感器网络中信息量较小，而节点必须随时准备接数据。理想的状态是当有信息需要接收时，节点处于接收状态，无信息接收时，节点处于睡眠状态。这就需要无线唤醒技术，这里的无线唤醒从现象上看，好像发射机把接收机从睡眠中唤醒，其实接收机是周期性得自动醒来，在醒来的极短时间内若没有发现呼叫信号，则马上睡眠，若正好有呼叫信号，则被唤醒而进入接收状，而跳频扩频使用的是CAD信道检测

### 温度测量
除了睡眠和待机模式之外，均可采用独立的温度测量模块对温度进行量测。该模块默认开启，在TempMonitorOff被设置为1时会关闭。温度测量的结果存储在RegTemp和TempValue中。

### 芯片复位
芯片上电后便会触发上电复位POR，还可以通过引脚7进行手动复位
POR 断开VDD 重新上电后等待10ms在POR周期结束后再开始通过SPI总线进行通信
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-24%20213146.png)

### 手动复位
引脚7的电平拉低100微妙然后释放，接着用户等待5ms再使用芯片![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-05-24%20213527.png)






