�����������̣�

1. ����CortexM4�ܹ�����λ��ʹ���������еĵ�ַ����ʼ���У�����������startup_stm32f407xx.s�ļ��У��������£�
<br>g_pfnVectors:<br>
  .word  _estack<br>
  .word  Reset_Handler<br>
  ...<br>

2. CPU����_estackΪ��ջָ�룬��ִ��Reset_Handlerλ�õĴ��롣����_estack������STM32F407IGHx_FLASH.ld
_estack = 0x2001FFFF;    /* end of RAM */
��ջ������RAM�Ľ�β��
3. Reset_Handler����Ҫ��C���ԵĻ�����ʼ���󣬵���SystemInit��Ȼ�����libc��س�ʼ����������main������
4. SystemInit����������CMSIS/system_stm32f4xx.c���ú�����ʼ��RCC��������ⲿRAM�����ʼ��RAM�ӿڡ�����GPIO���ú�FSMC���á�������¶�λ������
5. main�������ȵ���HAL_Init���ú�����ʼ����cache�������ж������ȼ�����ʼ��systick��������HAL_MspInit���ú���������Ϊ__weak�������û�ʵ�֣�Ĭ��ʲôҲû�С�
6. ����SystemClock_Config������ʼ��ʱ�ӡ��ú������ݸ������Ӳ�ͬ������ʵ�֡�
7. ��ʼ��BSP���֡����ݲ�ͬ�İ����в�ͬ�ĳ�ʼ��Ҫ�������ʼ��LED�������ж�ʲô�ġ�
8. ����lwip_init��ʼ��lwip�ĸ������ݽṹ��
9. ����Netif_Config���ú�����Ҫ������netif_add���������н�ethernetif_init������Ϊ�������ݸ���lwip��lwip�����øú�����ʼ����̫���ӿڡ�
10. ethernetif_init������ethernetif.c�ļ��С���������low_level_init��ʼ��Ӳ����
11. low_level_init����ʼ����ETH_InitTypeDef�ṹ�壬HAL_ETH_Init����ʹ�øýṹ����������ʼ��Eth�ӿڡ��ú���������stm32f4xx_hal_eth.c�ļ��У�HAL_ETH_Initһ��ʼ�ͻ����HAL_ETH_MspInit���ú����������û�ʵ�֡��������У��ú���ʵ����ethernetif.c�ļ��С�
12. HAL_ETH_MspInit������Ҫ����������̫���õ���GPIO�ڵ����ã��Լ���STM32��̫���ӿ�ʱ�ӵ�������
13. stm32f4xx_hal_eth.c��װ����STM32����Eth�ӿڼĴ����Ĳ�����