#软件模拟PWM输出
##分支8051：51上的12路舵机PWM输出试验
芯片为STC15F2K系列，其定时器**可以自动重装载**，计数器有类似STM32的“影子寄存器”的存在，可以放心地在定时器运行时改写计数器。<br>

###控制策略：
>1、将整个周期分成三份：0.6ms + 1.8ms + 17.5ms = 19.9ms，分别设置定时器值<br>
2、设定两个数组：12个PWM定时器变量以及它们的重装变量<br>
3、在前0.6ms部分，将所有输出都拉高，然后延时<br>
4、在1.8ms部分，每隔10us中断一次，遍历所有PWM定时器并递减，如果为零则拉低该通道的输出值<br>
5、最后17.5ms部分是单纯的延时


**为了最大程度提高效率，PWM部分用汇编编写，并且遍历PWM的部分只用顺序结构而不用循环**
