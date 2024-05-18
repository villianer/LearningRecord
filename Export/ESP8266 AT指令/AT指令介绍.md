### AT指令分类
1. |测试命令|AT+<命令名称>=?|查询设置命令的内部参数及其取值范围|
2. |查询命令|AT+<命令名称>?|返回当前参数值|
3. |设置命令|AT+<命令名称>=<…>|设置用户自定义的参数值，并运行命令|
4. |执行命令|AT+<命令名称>|运行无用户自定义参数的命令|
5. <>内的参数可以省略 []内的参数不可以省略
6. 特殊字符需要做转义处理 eg:\\ \, \"
7. AT命令默认波特率115200
8. 每条AT指令长度不超过256字节
9. AT指令以新行CR+LF结束
10. ESP8266返回消息类型有两种：ESP-AT响应和ESP-AT消息报告
	1. AT响应 被动
		1.  OK
		2. ERROR
		3. SEND OK
		4. SEND FAIL
	2. AT 消息报告 主动汇报系统中重要的状态变化或消息
		1.  ready
		2. busy
		3. Wil force to restart!!!
### 基础AT命令
### WIFI AT命令
1. AT+CWMODE? //
2. AT+CWMODE=<mode>[,<auto_connect>]
3. AT+CWJAP?查询与ESP连接的AP信息
4. AT+CWJAP=[<ssid>],[<pwd>][,<bssid>][,<pci_en>][,<reconn_interval>][,<listen_interval>][,<scan_mode>][,<jap_timeout>][,<pmf>]设置 ESP station 需要连接的AP
5. AT+CWJAP 将ESP station连接至上次Wifi配置中的AP
6. AT+CWQAP 列出当前可用AP
7. AT+CWSAP 断开与AP连接
8. AT+ CWSAP ？查询ESP SoftAP 的配置参数
9. AT+CWSAP=<ssid>,<pwd>,<chl>,<ecn>[,<max conn>][,<ssid hidden>]设置ESP SoftAP 的配置参数
10. DHCP 动态主机配置协议
11. AT+CWDHCPS查询/设置 ESP SoftAP DHCP 分配的 IP 地址范围
12. AT+CWAUTOCONN=<enable> 上电是否自动连接到AP
13. AT+CWAPPROTO： 查询/设置 SoftAP模式下802.11b/g协议标准

