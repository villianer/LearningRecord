### 主要特征
1. 最高波特率 fMASTER/16
2. Programmable data word length (8 or 9 bits)
3. Configurable STOP bits - support for 1 or 2 STOP bits
4. 传输侦测标志FLAGS
	- 接收缓存满
	- 发送缓存空
	- 传输结束标志
5. 奇偶校验
	- 发送奇偶校验位
	- 检查接收数据的奇偶校验
6. 4 个错误检测flag
	- overrun error//一个RDR中的数据未读取下一个数据被传送到RDR中，数据覆盖
	- noise error
	- frame error
	- parity error
7. 5个带有中断标志的中断源
	- Transmit data register empty
	- Transmission complete
	- Receive data register full
	- Idle line received
	- Parity error
8. 两个中断向量
	- 接收中断
	- 发送中断