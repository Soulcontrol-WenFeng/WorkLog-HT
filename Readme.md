# 姑且叫你学习记录吧  

# 2023-10-10  
## 代码随笔  
```  
typedef struct    
{  
  uint8_t *buff;		//缓冲指针  
  volatile uint16_t nMaxLen;  //缓冲大小  
  volatile uint16_t nUseLen;  //使用大小  
  volatile uint16_t nReadPos; //读位置  
  volatile uint16_t nWritePos;//写位置  
  volatile uint16_t nTmpUseLen; //临时使用  
  volatile uint16_t nTmpWPos;   //临时写位置  
  volatile uint16_t nTmpWSize;  //临时读位置  
}CycleBuff_T;    
```  

# 2023-10-11  
## 记录  
关于去耦电容和旁路电容的一些知识  

### 一、定义 
`在电子电路中，去耦电容和旁路电容都是起到抗干扰的作用，电容所处的位置不同，称呼就不一样了。对于同一个电路来说,旁路（bypass）电容是把输入信号中的高频噪声作为滤除对象，把前级携带的高频杂波滤除，而去耦（decoupling）电容也称退耦电容，是把输出信号的干扰作为滤除对象。去耦电容用在放大电路中不需要交流的地方，用来消除自激，使放大器稳定工作`  

`(1)旁路（bypass）电容：是把输入信号中的高频成分当作滤除对象`  
`(2)去耦（decoupling））电容：去耦电容是电路中装设在元件的电源端的电容，此电容可以提供较稳定的电源，同时也可以降低元件耦合到电源端的噪声，间接可以减少其他元件受此元件噪声的影响`    
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/f76f103c-b5bd-45b9-ae76-83cd87c91dae)  


### 二、功能  
#### 1、去耦电容作用  
`（1）去除高频信号干扰；`  
`（2）蓄能作用；`    
在电子电路中，去耦电容和旁路电容都是起到抗干扰的作用，电容所处的位置不同，称呼就不一样了。对于同一个电路来说，旁路（bypass）电容是把输入信号中的高频噪声作为滤除对象，把前级携带的高频杂波滤除，而去耦（decoupling）电容也称退耦电容，是把输出信号的干扰作为滤除对象。去耦电容用在放大电路中不需要交流的地方，用来消除自激，使放大器稳定工作。  







# 2023-12-26  
##  记录  
关于FOC的一些知识点  
###   一、定义  
`FOC(Field-Oriented Control）,直译是磁场定向控制，也被称为矢量控制(VC,Vector Control),是目前无刷直流电机（BLDC)和永磁同步电机（PMSM)高效控制的最优方法之一。FOC旨在通过精确地控制磁场大小与方向，使得电机运动转矩平稳、噪声小、效率高，并且有高速的动态响应。`   
在此将其理解为电机驱动的一部分，或者电机驱动的全部，因为电路上是可以将MCU和FOC板子合在一起，省去了“主控”的部分。说到电机驱动，就不得不说到电子调速器（ESC，Electronic Speed Control)，简称电调，二者虽然都可以理解为电机的“驱动”部分，但有较大的区别。  
###   二、电调定义  
####   1、电调的功能    
`主要控制电机的启停和转速，有些电调还可以用来给接收机供电，即BEC`   
####   2、电调原理  
`电调具有“电流控制”作用，在其内部电路中有一套MOSFET管（功率管），电流输入电调，内部电路接收来自接收机的启停及转速，根据信号把电流做出合适的控制，然后把控制后的电流输出给马达，从而控制电机的启停及转速。针对电机的不同，也分为有刷电调和无刷电调。有刷电调就是简单的直流输出，无刷电调就是把直流电转换成3相交流电。电调输入是直流，可以接稳压电源或者锂电池。`  
```  
FOC的优势：  
低转速下控制  
由于控制原理的区别，无刷电调只能控制电机工作在高转速下，低速下无法控制；而FOC控制器则完全没有这个限制，不论在什么转速下都可以实现精确控制。  
电机换向  
同上面的理由，由于电调无法反馈转子位置，因此很难实现电机正反转的换向（当然有感电调可以实现）；而FOC驱动器的换向性能极其优秀，最高转速下正反转切换可以非常顺畅；此外FOC还可以以能量回收的形式进行刹车控制。  
力矩控制  
普通电调都只能控制电机转速，而FOC可以进行电流（力矩）、速度、位置三个闭环控制。  
噪音  
FOC驱动器的噪音会比电调小很多，原因是普通电调采用方波驱动，而FOC是正弦波。  
电调的优势：  
兼容性  
电调驱动不同的BLDC不需要进行参数整定，而FOC需要。  
算法复杂度  
电调的算法实现更简单，运算量少，很适合需要提高带宽的超高转速电机。  
成本  
电调的成本比FOC低很多。  
```


#  2023-12-27  
##  记录   
关于FOC测试板的一些想法：  
###  一、硬件方案：  
####  1.1 主控及驱动芯片部分    
MCU的话打算选择STM32F103C8T6,不要问为什么，因为我手头上就有一块，图个方便。大体方案还是沿用立创开源社区当中的一个方案，以TI的DRV8313作为无刷电机的驱动芯片，以AS5600作为磁编码器，要搭配磁铁使用。  
####  1.2 电源部分  
电源部分需考虑MCU和驱动芯片的差异，驱动芯片的输入电压范围为8-60V，但具体最终驱动板的输入电压要为多少，还需考虑你所需要测试的电机的工作电压，以12V为例，则电源部分需要12转3.3V的DCDC降压芯片，立创开源方案中的电源部分采用的是LDO，虽然是降成了5V，但个人感觉效率不高，可能发热会严重  

![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/dc8a9a06-06b7-4be6-ad5c-0435338deb61)  



###  二、开源参考方案：  
####  1.1 参考方案一  
https://oshwhub.com/Knight_Sin/abcd  
该方案仍然使用了TI 的BLDC驱动芯片，不过是DRV8301DCAR（区别于参考方案二），但是同时也使用了KNY3406C（场效应管，N沟道，漏源电压为60V，连续漏极电流为80A）  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/c5f4e8d1-8d12-4fdc-9879-54be63bc3aca)  

![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/546f4e4a-c450-495f-8b07-24271d20e7f4)  

立创商城链接：  
https://item.szlcsc.com/100181.html  
https://item.szlcsc.com/458586.html  

#####  个人对于方案一的理解：  
方案一中使用的BLDC驱动DRV8301DCAR手册当中有一张原理图如下：  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/438827bb-e00d-4ed2-9b2c-2b03b8e15e7d)  
该图意味着可以用SPI或者PWM进行控制  
在该份芯片的手册当中，PWM输入有高边（HIGH SIDE)和低边（LOW SIDE)之分，关于高低边的解释，H桥的PWM控制等，有以下相关帖子：  
https://superigbt.github.io/2022/11/03/SMPS/SMPS-highside-lowside/  
https://blog.csdn.net/yyyyyrr5/article/details/106599525  

芯片手册链接：  
https://www.ti.com/cn/lit/ds/symlink/drv8301.pdf?ts=1703666240386&ref_url=https%253A%252F%252Fitem.szlcsc.com%252F  





#  2023-12-28  
##  记录  
####  1.2 参考方案二  
https://oshwhub.com/yourallo/youngfoc  
该方案使用的无刷电机驱动芯片为 DRV8313(同为TI的无刷电机芯片，区别于参考方案一)，磁编码器用的是AS5600（搭配磁铁使用）  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/ca16c107-1369-4019-b2b7-eb1044128f78)  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/ef2a26ed-5a0b-4471-a0cf-e94c9bc3524a)  

立创商城链接：  
https://item.szlcsc.com/93675.html  
https://item.szlcsc.com/80958.html  


PS:在学习过程中发现电路的电源部分均有多电容并联的设计存在，关于此设计找到的一些相关资料链接如下：  
https://zhuanlan.zhihu.com/p/597152452  
在开源项目 https://oshwhub.com/knight_sin/ji-yucw32f0-di-yi-ti-shifoc-qu-dong-qi  当中，有对多并联电容做了一个小解释，如下：  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/6ac02ee2-b247-4a80-a7f4-5d603482d81f)



####  1.2 参考方案三  














