程序运行流程：

1. 根据CortexM4架构，复位后，使用向量表中的地址来开始运行，该向量表在startup_stm32f407xx.s文件中，内容如下：
<br>g_pfnVectors:<br>
  .word  _estack<br>
  .word  Reset_Handler<br>
  ...<br>

2. CPU将以_estack为堆栈指针，来执行Reset_Handler位置的代码。其中_estack定义在STM32F407IGHx_FLASH.ld
_estack = 0x2001FFFF;    /* end of RAM */
堆栈定义在RAM的结尾处
3. Reset_Handler做必要的C语言的环境初始化后，调用SystemInit，然后调用libc相关初始化，最后调用main函数。
4. SystemInit函数定义在CMSIS/system_stm32f4xx.c，该函数初始化RCC，如果有外部RAM，则初始化RAM接口。包括GPIO设置和FSMC设置。最后重新定位向量表。
5. main函数首先调用HAL_Init。该函数初始化了cache，设置中断组优先级，初始化systick。最后调用HAL_MspInit。该函数被设置为__weak，可由用户实现，默认什么也没有。
6. 调用SystemClock_Config函数初始化时钟。该函数根据各个板子不同，自行实现。
7. 初始化BSP部分。根据不同的板子有不同的初始化要求。例如初始化LED、设置中断什么的。
8. 调用lwip_init初始化lwip的各个数据结构。
9. 调用Netif_Config，该函数主要调用了netif_add函数，其中将ethernetif_init函数作为参数传递给了lwip，lwip将调用该函数初始化以太网接口。
10. ethernetif_init定义在ethernetif.c文件中。它调用了low_level_init初始化硬件。
11. low_level_init，初始化了ETH_InitTypeDef结构体，HAL_ETH_Init函数使用该结构体设置来初始化Eth接口。该函数定义在stm32f4xx_hal_eth.c文件中，HAL_ETH_Init一开始就会调用HAL_ETH_MspInit。该函数可以由用户实现。本程序中，该函数实现在ethernetif.c文件中。
12. HAL_ETH_MspInit函数主要负责设置以太网用到的GPIO口的设置，以及对STM32以太网接口时钟的启动。
13. stm32f4xx_hal_eth.c封装所有STM32所有Eth接口寄存器的操作。