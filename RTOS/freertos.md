### freertos 规范
1. www.freertos.org 官网
2. freertos 数据类型 freertos 自己的数据类型 与c标准库类型相对应
	1. portCHAR    pc  char
	2. portSHORT  ps  short
	3. portLONG   pl   long
	4. portFLOAT   pf   FLOAT
	5. portDpuble pd Double
	6. portSTACK_TYPE uint32_t
	7. portBASE_TYPE long
	8. portTickType  用于定义系统时基计数器的值和阻塞时间的值当 FreeRTOSConfig.h 头文件中bconfigUSE_16_BIT_TICKS 为 1 时则为 16 位,为0时是32位整型
	9. portBASE_TYPE x 根据系统处理器的架构确定 系统处理器多少位他就是多少位
	10. 

4. 变量名：把变量类型作为前缀加在变量上usigned char  uc     指针加上p
5. 函数名：包含函数返回值类型 函数所在的文件名 和函数功能 私有函数会加prv
	1. vTaskPrioritySet()函数的返回值为 void 型， 在task.c 这个文件中定义
	2. xQueueReceive()函数的返回值为 portBASE_TYPE 型， 在 queue.c 这个文件中定义
5. 宏：宏均是由大写字母表示，并配有小写字母的前缀， 前缀用于表示该宏在哪个头文件定义
	1. |port (举例, portMAX_DELAY)|portable.h|
6. 格式 ： 一个tab 4个空格
### 裸机系统 时间片轮询系统 前后台系统 任务系统
### 双向链表
1. freertos中的列表项 以双向链表的形式 主要作用为存储任务
### FreeRtosConfig.h FreeRtos的功能配置裁剪文件
1. INCLUDE_开始的宏 用于使能或失能各个API函数
2. config 开始的宏
	1. configAPPLICATION_ALLOCATED_HEAP 自行设置堆内存
	2. configASSERT 类似c中的断言
	3. configCHECK_FOR_STACK_OVERFLOW 设置堆栈溢出检测
	4. configCPU_CLOCK_HZ 设置CPU频率
	5. configSUPPORT_DYNAMIC_ALLOCATION
	6. configENABLE_BACKWARD_COMPATIBILITY
	7. configGENERATE_RUN_TIME_STATS
### SCB system control block
### SCS system control space

### Freertos 任务基础知识
1. 什么是多任务系统
2. freertos任务与线程
3. 初次使用
4. 任务状态
	1. runing :正在运行中
	2. ready：已经准备就绪但是有cpu此时有其他同级别或更高几倍任务正在执行中
	3. blocked:阻塞状态 等待某个事件或信号再执行  有限制等待时间
	4.suspend暂定 ：
	5. delete![[Pasted image 20230625115810.png]]
5. 任务优先级
6. 任务实现
7. 任务控制块
8. 任务堆栈
9. 空闲任务：清除自杀的任务实体
### Freertos 配置文件 freertosconfig.h
优先级的取值范围是： 0~(configMAX_PRIORITIES – 1)
configUSE_PORT_OPTIMISED_TASK_SELECTION \==1 使用汇编实现
configTICK_RATE_HZ  表示系统滴答定时器的频率
/* 获得当前的 Tick Count */xTaskGetTickCount();
/* 使用空闲任务钩子函数*/configUSE_IDLE_HOOK
/*配置任务调度算法*/ 
/是否使用抢占优先级/configUSE_PREEMPTION 
/是否使用时间片轮转/configUSE_TIME_SLICING
/关闭tick来实现省电/configUSE_TICKLESS_IDLE
轮转调度  "(Round Robin Scheduling)
/空闲任务是否让步/configIDLE_SHOULD_YIELD
/使用软件定时器/configUSE_TIMERS
/时钟守护任务的优先级/configTIMER_TASK_PRIORITY
/定时器命令队列的长度/configTIMER_QUEUE_LENGTH
### 函数
动态创建任务：creatTask(handle)
删除任务：deleteTask(handle)
任务阻塞延时：vTaskDelay(pdMS_TO_TICKS(100)); // 等待 100ms
任务阻塞延时指定的时间间隔：BaseType_t xTaskDelayUntil( TickType_t * const pxPreviousWakeTime,  
const TickType_t xTimeIncrement );
获取任务优先级：UBaseType_t uxTaskPriorityGet( const TaskHandle_t xTask ); 
设置任务优先级： vTaskPrioritySet( TaskHandle_t xTask,  
UBaseType_t uxNewPriority );  
任务挂起：void vTaskSuspend( TaskHandle_t xTaskToSuspend );//任务进入暂停状态
要退出暂停状态，只能由别人来操作：  
 别的任务调用： vTaskResume  
 中断程序调用： xTaskResumeFromISR
挂起所有任务：vTaskSuspendAll() 实际上是挂起任务调度器
恢复所有任务： xTaskResumeAll()
任务删除函数：vTaskDelete()
获取当前系统时间：xTaskGetTickCount();
### 消息队列常用函数
1. 创建消息队列：xQueueCreate
2. 删除队列：vQueueDelete( xQueue );
3. 写队列：xQueueSend()；xQueueSendToBack()；xQueueSendFromISR()；xQueueSendToBackFromISR()；用于在中断函数中使用 xQueueSendToFront()；插入队列首 xQueueSendToFrontFromISR()
5. 读队列:xQueueReceive() xQueuePeek() //接收完消息后不会删除消息队列中的消息 xQueueReceiveFromISR () xQueuePeekFromISR()
6. 删除队列:void vQueueDelete( QueueHandle_t xQueue )
7. 队列应用场景：用于任务之间安全传递数据 
### 信号量
1. Semaphore 信号量是一个非负整数，所有获取它的任务都会将该整数减一（获取它  当然是为了使用资源），当该整数值为零时，所有试图获取它的任务都将处于阻塞状态。  通常一个信号量的计数值用于对应有效的资源数，表示剩下的可被占用的互斥资源数。其值的含义分两种情况
2. 二值信号量 多用于任务间的同步 信号量在建立后置为空，任务A获取信号量进入阻塞，当事件发生后，任务B释放信号量，此时任务获得信号量进入就绪状态
3. 计数信号量： 用于表示临界资源 与事件计数 每当某个事件发生时，任务或者中断将释放一个信号量（信号量  计数值加 1），当处理被事件时（一般在任务中处理），处理任务会取走该信号量（信号量计数值减 1），信号量的计数值则表示还有多少个事件没被处理，此外，系统还有很多  资源，我们也可以使用计数信号量进行资源管理，信号量的计数值表示系统中可用的资源  数目，任务必须先获取到信号量才能获取资源访问权，当信号量的计数值为零时表示系统  没有可用的资源，但是要注意，在使用完资源的时候必须归还信号量，否则当计数值为 0 的时候任务就无法访问该资源了
4. 互斥信号量 mutex具有优先级继承机制 从而使他更适合用于保护临界资源 临界资源是指任何时刻只能被一个任务访问的资源
5. 递归互斥量 可以重复获取的临界资源 对于已经获取递归互斥量的任务可以重复获取该递归互斥量， 该任务拥有递归信号量的所有权。 任务成功获取几次递  归互斥量， 就要返还几次，在此之前递归互斥量都处于无效状态， 其他任务无法获取， 只  有持有递归信号量的任务才能获取与释放
6. 常用信号量接口函数
	1. 创建信号二值信号量  xSemaphoreCreateBinary()
	2. 创建计数信号量 xSemaphoreCreateCounting()
	3. 删除信号量vSemaphoreDelete()用于删除一个信号量，包括二值信号量，计数信号量，互斥量和递  归互斥量
	4. 信号量释放：xSemaphoreGive()（任务）xSemaphoreGiveFromISR()（中断）
	5. 信号量获取 xSemaphoreTake() xSemaphoreTakeFromISR()（中断）
7. 互斥量的优先级继承机制：暂时提高某个占有某种资源的低优先级任务的优先级，使之与在所有等待该  资源的任务中优先级最高那个任务的优先级相等，而当这个低优先级任务执行完毕释放该 资源时，优先级重新回到初始设定值。互斥量不能在中断服务函数中使用。
8. 互斥量接口函数
	1. 互斥量创建函数 xSemaphoreCreateMutex()
	2. 递归互斥量创建函数xSemaphoreCreateRecursiveMutex()
	3. 互斥量删除函数 vSemaphoreDelete()
	4. 互斥量获取函数 xSemaphoreTake()
	5. 递归互斥量获取函数 xSemaphoreTakeRecursive()
	6. 互斥量释放函数 xSemaphoreGive()
	7. 递归互斥量释放函数 xSemaphoreGiveRecursive()
### 事件组
1. 事件控制结构体 EventGroup_t 包含1个32位eventbits 1个xTaskwaitingForBits 事件等待列表
2. 创建事件函数 xEventGroupCreate() 动态的创建内存返回一个句柄指向事件控制结构体 初始化给eventbits全部置零 给事件等待列表初始化。
3. 事件删除函数vEventGroupDelete() 
4. 事件位置位函数 xEventGroupSetBits() 中断中使用|BaseType_t xEventGroupSetBitsFromISR(EventGroupHandle_t xEventGroup,  <br>const EventBits_t uxBitsToSet,  <br>BaseType_t \*pxHigherPriorityTaskWoken);|
### 软件定时器







1. xcreatTimer()
2. x
### 任务通知
1. BaseType_t xTaskGenericNotify( TaskHandle_t xTaskToNotify, uint32_t ulValue, eNotifyAction eAction, uint32_t 、\*pulPreviousNotificationValue )
2. 任务有三种状态 等待通知 不等待通知  已经接收通知
3. eNotifyAction 有五种种动作 setbits  noaction  eincrement  eSetValueWithOverwrite eSetValueWithoutOverwrite
4. xTaskNotifyGive() =xTaskGenericNotify( ( xTaskToNotify ), ( 0 ), eIncrement, NULL )  此时可以作为二值信号量 或 计数信号量的替代 等待任务通知使用ulTaskNotifyTake()
5. vTaskNotifyGiveFromISR()在中断中使用
6. xTaskNotify()  \xTaskNotifyFromISR()
7. xTaskGenericNotifyFromISR()
8. xTaskNotifyAndQuery() xTaskNotifyAndQuery()FromISR
9. 获取任务通知的函数
10. ulTaskNotifyTake() 和  xTaskNotifyWait ()
### 内存管理
![[Pasted image 20230711125739.png]]
### 任务栈大小
1. 先分配一个比较大的栈内存，所有功能进行测试后，使用uxTaskGetStackHighWaterMark(NULL)运行过程中最小的剩余栈空间。一般把栈空间设置为最大的1.5倍。
2. #define configCHECK_FOR_STACK_OVERFLOW 1 任务切换时检测任务指针是否溢出
3. #define configCHECK_FOR_STACK_OVERFLOW 2 初始化时将内存初始化为0x05 任务切换时进行任务栈检测的时候会检测末 尾的 16 个字节是否都是 0xa5 
4. 当检测到内存溢出时，可以调用钩子函数，输出有用信息。
