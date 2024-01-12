# HT 工作日志    




# 2023-10-09  
关键词：NAS、循环缓冲、旋转矩阵、cubeide dark theme plug-in、GitHub link、PCB Backup、some stupid GitHub items   
--------------------------  
今日学习效率低下，真是辛苦自己了  





# 2023-10-10  
关键词：CUBEIDE DARK THEME、ECLIPSE MARKETPLACE……  
--------
描述：在公司电脑上下载并且使用了CUBE IDE，一开始软件的背景为白色，很刺眼，后来在Eclipse Marketplace中下载dark theme插件更改成功。但是在个人PC上重复步骤却一直出错，下载到46%左右的时候会报错，之前公司电脑也会出现该错误但是查看之前的工作日志发现是有解决的，但是复刻步骤在PC上却没有成功   
**公司电脑的当前IDE界面:**
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/c6614e58-6c2b-4ef3-a203-ab2bf3215b9c)
**查看曾经解决的工作日志：**
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/7c3935af-23a9-468f-90e3-a9638481e167)



# 2023-10-11  
今天还是尝试从官网下载cubeide的最新版本，竟然下载成功了，传回QQ晚上回去在PC上面再试一下   
`code`  
`printf("Hi World\r\n");`  
<font size=7>官网链接</font>：  
```  
https://www.st.com/zh/development-tools/stm32cubeide.html#st-get-software  
```
<font color=yellow>跳转链接</font>  
  **[CUBEIDEGET](https://www.st.com/zh/development-tools/stm32cubeide.html#st-get-software)**   
<https://www.st.com/zh/development-tools/stm32cubeide.html#st-get-software>  

![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/ce98ffc3-dd82-464a-a9df-804b4ce72e32)  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/e2b9432a-ffbc-4a0f-b464-b4de22675cdb)  
官网上的最新版本为1.13.2，公司电脑上的为1.12.1，可能是版本不够新的问题，PC上的1.5.0software升级每次也都不成功  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/e80a8425-4149-49b5-9c54-498028ddf473)  

回到住所后将官网下载的1.13.2版本下载在PC上，下载成功后软件默认为黑色主题背景，以为是已经下载好了DARK THEME插件，点进去发现是软件自带的黑色设置，不过这个版本的黑色背景确实还可以，但是代码高亮的部分比较一般，可以说有点清爽，于是又下载了一遍DARK THEME插件，这回成功下载并且重启，值得注意的是官网下载的这个版本需要登陆你的个人账号才能进行设备支持包等一系列插件或者软件的下载，以下：  
![KQ) TS`BY@I$4 B WNT2)I](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/984f9903-d74c-4495-bd97-e4a1c86b4b12)



# 2023-10-12


# 2023-10-13  
关键词：Eclipse、乱码、UFT-8、代码移植、CMakelist  
--------------

问题：今天在移植同事的一个工程（基于ESP32模组的代码）的时候编译不过，一般编译到一半就出错，显示某文件中索引不到某变量或者某定义（由于失败工程已经删除此处不放截图）。这里猜测可能是因为某些包含的头文件没有被索引到。同事用于代码编辑的工具是notepad++，编译的平台是乐鑫提供的ESP-IDF-CMD,不过他的版本为五点多，我电脑上面的则是4.4，可能由于版本不一样导致无法编译。但是在ESP提供的Espressif-IDE中编译也仍然报错，如下：  

![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/1c0e428f-464e-4456-9136-4e564c5908b6)  

主要为两种错误：  
`1、implicit declaration of function ……，did you mean  ……（隐式声明）`
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/59e500f5-be72-4684-bdca-9ef3bfb2e976)  

`2、symbol '……‘ could not be resolved(符号无法解析)`  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/a2b8b28b-86a3-4604-abc7-5047cc8aba4e)

根据我以往的经验，出现该两种错误有可能是其他错误造成的，比如头文件索引问题，或者是cmakelist里面的头文件和源文件路径问题，在该工程当中，很容易就可以发现符号CONFIG_I2C_MASTER_PORT等以下列I2C相关定义没有找到，而在main文件中却仍被调用，有可能同事发给我的工程少了一些文件，也有可能是某处的编译开关被关掉导致无法索引。  

针对第问题2，我们可以产看错误符号所在的位置，我们发现，代码段有一个编译开关，而很显然，在该工程中此编译开关应该是关闭的，因为没有I2C驱动，以下：  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/b23ecb1a-6d8e-44a2-b054-3b1c9c106a8d)
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/58f8cc1b-3a95-4231-a38c-553cdb377917)

然后注释掉，重新编译，又开始报莫名其妙的错误，于是我决定，建立新工程，这次我只拷贝.c和.h文件，可能有个sdkconfig的文件会影响到，所以只拷贝如下几个：    
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/6222983f-e9e6-4c22-a581-2a0514500dd4)



# 2023-10-16  
10月13日的问题最后还是没有发现问题所在，由于是移植同事的工程，太多事件状态要看，并且同事用的IDF版本跟我不一样，也存在可能调用的函数是否一致等问题，故不再处理这个问题，转向新画板子IACC的测试。  

测试中发现，程序无法进行烧写，将电路上的RS485（该485为5V供电，数据口输出电压也为5V，而ESP32模组则为3.3V）芯片拆下后烧写正常。  

后续打算更换3.3V供电的485芯片测试看看。  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/d01dc3f2-cef1-45c9-92e6-43fbebbed088)




# 2023-11-21  
上周结束了为期三周的工厂重复性组装工作，昨日又重新回到公司来。在工厂这段时间的工作中，让我感受到了工厂同事的热情于乐观，我觉得他们才是真正的工人阶级，不抱怨，发挥主观能动性，将好的情绪传递给周围的人，希望他们能早日迎来双休。  



 也让我知道了在当今如此内卷的大背景下，想要躺平意味着要有一定程序的“卷”，也许就是老生常谈的先苦后甜吧。在此也深刻意识到了个人核心竞争力的重要性。  

 最近忙里偷闲搞了张小板子，想要趁着空闲时间做一个温湿度可显的小玩物，目前原理图和PCB已经更新到第二个版本，添加了气压计和姿态传感芯片MPU6050,也将第一版的电源芯片TPS563201DDCR替换成了AP2112K-3.3TRG1，减少了电源部分器件数量，使得小板子里能塞下更多东西。  
 -------------------  

 `版本一`  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/760deda5-e2a7-4918-be20-a6b6a69c7c3b)
 ![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/315202ea-757f-44df-b843-222160400280)
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/2d5a81e9-133b-4eac-af32-13746dfb5cc1)



`版本二（尚未走线）`  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/1409d8ac-fa05-4025-88c6-052c4b36780c)
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/6f2b7a34-1788-4428-8780-1989d999a715)




# 2023-11-22  
同事在对ESP32-C3模组进行烧写程序的时候，会出现一些欠压复位等不在意料之内的情况以及重新上电后仍然进入等待SPI下载的情况，如下：  
`rst:0xc (RTC_SW_CPU_RST),boot:0xf (SPI_FAST_FLASH_BOOT)`  
`SPIWP:0xee`  
`mode:DI0, clock div:1`  

`rst:0xf (BROWNOUT_RST), boot:0x1(SPI_DOWNLOAD_BOOT)`  
`wait spi download`  

`rst:0x1 (POWERON), boot:0xf (SPI_FAST_FLASH_BOOT)`  
`SPIWP:0xee`  

`rst:0xf(BROWNOUT_RST),boot:0x7(DOWNLOAD(USB/UART0/1))`  
`waiting for download`    

在以上情况当中，出现了  **rst:0xf (BROWNOUT_RST)**  ,  **boot:0xf (SPI_FAST_FLASH_BOOT)**  意味着欠压复位,  
如果是 **rst:0x1 (POWERON)**  意味着上电复位  




  # 2023-12-12 
  Thinking:  想根据自己卧室的增高架尺寸做一个桌面控制台，刚好可以嵌进增高架下面的空间，遮挡住后面的排查和电线，具有电脑主机开关机，温湿度等参数显示，旋钮调节音量等参数等功能，留有冗余接口   
  ------------------



#  2023-12-13  
Thinking：这段时间还想要做一个跟FOC相关的项目，或者跟无刷电机相关的项目，正好可以增进一下相关方面的知识。  
-------------------------------------------------------  
跟FOC相关的理论知识可以先行补充，此前有在知乎找到两个帖子，写得蛮精细，有一个是稚晖君做的关于FOC开源的项目，可做参考或者复刻。  
稚晖君：  
https://zhuanlan.zhihu.com/p/147659820   
其他：  
https://zhuanlan.zhihu.com/p/364247816  


# 2023-12-21  
记录：关于HT-IACC-S V1.02板子，红外发射的时候MCU会进入低压重启，拆掉IR05模块上面的四个红外发射头的其中2-3个的通路电阻（断开）问题解决，同时，新板子仍然有电源上电后的噪音问题，只不过是从旧板V1.01的滋滋滋声音变成了现在类似与继电器开断的嗒嗒嗒声。  
----------------------------------------  

今日任务一：阅读FOC控制的相关代码
--------------
**阅读的是稚晖君开源的Ctrl-FOC-Lite-fw版本的代码，有一份是基于STM32写的，但是文件结构和之前的看到的有点不一样，这份代码中有.project文件，应该可以在STM32CUBEIDE当中打开和编译，但是会报错，该工程代码文件结构如下**   
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/0596d394-5964-4c96-a11f-061abced5d08)  

**相较于以往的工程文件，只不过是多了几个文件夹，主体文件结构没有变化，所以编译不过可能是配置方面的问题**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/54cd971b-d0ff-450b-bb2b-7aedf726b11a)    

**我们查看IDE报错的信息：**    
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/afe6986a-26ac-4e63-b111-3a43eac1c661)    
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/bbff49ce-6fde-4d2b-8172-359455e8d5fd)    

**这里面有个FATAL ERROR,找不到该文件，但很显然我还是索引到了该文件，这又是一个严重错误，排查中**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/346a67fe-9650-4914-a8c8-67b9da5acd7c)  
**在UserApp和Platform等文件夹中的文件应该是C++的文件，后缀是.cpp**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/b21a273b-1402-4585-b7ca-705ce7d7580e)  


**我们进入ide的添加头文件路径选项，将没有索引到的头文件添加到link路径当中**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/9c631fc0-a4f7-46f1-b8af-77bc034b2d94)  
**重新build，这个时候我们发现，找不到.h文件的错误没有了，但是有新的错误出现，如下：**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/53081ed2-2ff3-46c7-a850-b6f7684cbaed)  
**在console窗口我们可以看到这些错误的详情，我们发现，报错的路径中显示的是E 盘，但是我们的工程文件所在的workspace是在F盘，E盘中存在中文路径，可能有潜在的隐患，所以我决定删掉该工程文件，重新导入，这里我也意识到了错误的地方，一开始我是从下载的文件夹当中打开了工程文件，导致了工程并不在默认的WORKSPACE空间当中，这是一个低级错误**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/3d9d0a88-3673-4620-b65c-6a85ad8c6649)  
**重新打开WORKSPACE路径，然后导入该工程，修改头文件路径，出现其他错误**  
![image](https://github.com/Soulcontrol-WenFeng/Soulcontrol-WenFeng/assets/74033919/3152a0b9-d474-4c4b-8a72-046c2a578c6a)  

**在该份代码中，不是很确定UserApp当猴子那个的main.cpp会不会和Core/Src当中的main.c文件起冲突，并且二者都有一个main()函数只不过其中一个是Main()，检查一下CMakelist文件**  



#2023-12-22  
昨天的代码编译先不深究，重要的是先看懂FOC的驱动代码，可以自己移植到其他平台上面，可以先阅读一些相关的帖子，以下一些帖子可供参考：  
https://zhuanlan.zhihu.com/p/293470912  
https://zhuanlan.zhihu.com/p/364247816#Foc%E6%8E%A7%E5%88%B6%E6%A1%86%E6%9E%B6%E5%9B%BE  
https://www.zhihu.com/column/zhishixuediande  

#2023-12-24  
今天仍然阅读FOC相关资料以及一些开源项目的原理图和PCB，以下为相关链接：    
https://oshwhub.com/yourallo/youngfoc  
https://oshwhub.com/hvan/core_foc_mcupart  
https://oshwhub.com/a1804889557/xin-chun-kuai-le-ning-meng-FOCka  
https://oshwhub.com/Knight_Sin/abcd   


#2023-12-25  
今日仍然阅读FOC相关开源项目和资料，大概年底之前可以出一张 FOC的板子  












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
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/e2af3f7c-0e90-472e-8e78-659ea895f882)  

![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/fce89285-8880-417b-8e49-3eca778bb512)


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
方案三为超迷你FOC方案 ，该方案为叠板设计，分为控制板和驱动板（up主说该份代码是自己写的，那应该是有别前两种方案用ODrive的开源代码且是基于STM32的，感觉可以先读该方案的源码）    
控制板：  
https://oshwhub.com/hvan/core_foc_mcupart  
驱动板：  
https://oshwhub.com/hvan/h7_core_foc  
开源链接：  
https://github.com/hvan110/miniFOCctrl  
该方案有别于前两份的硬件设计，使用的电机驱动为MOS DRIVE加MOS管（前者在控制板，后者在驱动板），此外，控制板 增加了TFT屏幕驱动和CAN通信电路，还有一个AMP电路，应该是用于采样反馈信息用的（在驱动板上，AMP的采样信号为MOS管漏极的电压）
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/305d3aa5-0488-446e-a7ac-3a2d22d4e3d8)  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/8e1fb730-95c4-45c5-815e-e3379a4d25ff)  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/403e2307-b157-4117-9cb7-54d0b6418925)  
  
  
  
PS:关于米勒钳位的知识点，相关链接如下：  
https://zhuanlan.zhihu.com/p/554854088#:~:text=%E6%9C%89%E6%BA%90%E7%B1%B3%E5%8B%92%E9%92%B3%E4%BD%8D%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%20%E6%A0%85%E6%9E%81%E5%92%8C%E6%BA%90%E6%9E%81%E4%B9%8B%E9%97%B4%E7%9A%84%E9%99%84%E5%8A%A0%E5%BC%80%E5%85%B3%E5%9C%A8%E9%AB%98dV%2Fdt%E6%83%85%E5%86%B5%E4%B8%8B%E6%8E%A7%E5%88%B6%E7%B1%B3%E5%8B%92%E7%94%B5%E6%B5%81%EF%BC%8C%E5%B9%B6%E4%B8%94%E5%9C%A8%E8%BE%BE%E5%88%B0%E7%94%B5%E5%8E%8B%E6%B0%B4%E5%B9%B3%E5%90%8E%E7%9F%AD%E6%8E%A5%E6%A0%85%E6%9E%81%E8%87%B3%E6%BA%90%E6%9E%81%E4%BD%BFSiC%20MOSFET%E5%AE%8C%E5%85%A8%E5%85%B3%E9%97%AD%E3%80%82%20%E5%9C%A8%E5%85%B3%E6%96%AD%E5%BC%80%E5%85%B3%E6%9C%9F%E9%97%B4%EF%BC%8C%E5%BD%93%E6%A0%85%E6%9E%81%E7%94%B5%E5%8E%8B%E4%B8%8B%E9%99%8D%E5%88%B0%E4%BD%8E%E4%BA%8E%E7%9B%B8%E5%AF%B9%E4%BA%8EVEE%E7%9A%84%E9%98%88%E5%80%BC%E7%94%B5%E5%8E%8B%E5%80%BC%E6%97%B6%EF%BC%8C%E6%A3%80%E6%B5%8B%E5%88%B0%E6%A0%85%E6%9E%81%E7%94%B5%E5%8E%8B%E5%B9%B6%E6%BF%80%E6%B4%BB%E9%92%B3%E4%BD%8D%EF%BC%88%E8%A7%81%E5%9B%BE6%E3%80%82%20%E6%9C%89%E6%BA%90Miller%E9%92%B3%E4%BD%8D%EF%BC%89%E3%80%82,%E9%80%9A%E8%BF%87%E7%B1%B3%E5%8B%92%E7%94%B5%E5%AE%B9%E7%9A%84%E7%94%B5%E6%B5%81%E7%94%B1%E6%99%B6%E4%BD%93%E7%AE%A1%E5%88%86%E6%B5%81%EF%BC%8C%E8%80%8C%E4%B8%8D%E6%98%AF%E6%B5%81%E7%BB%8F%E8%BE%93%E5%87%BA%E9%A9%B1%E5%8A%A8%E5%99%A8VOUT%E3%80%82%20%E7%B1%B3%E5%8B%92%E9%92%B3%E4%BD%8D%E5%8A%9F%E8%83%BD%E4%BB%85%E5%9C%A8SiC%20MOSFET%E5%85%B3%E9%97%AD%E6%9C%9F%E9%97%B4%E6%9C%89%E6%95%88%EF%BC%8C%E4%B8%8D%E5%BD%B1%E5%93%8DSiC%20MOS%E5%BC%80%E5%90%AF%E3%80%82%20%E5%AE%83%E5%8F%AF%E4%BB%A5%E8%8A%82%E7%9C%81%E6%88%90%E6%9C%AC%EF%BC%8C%E6%B6%88%E9%99%A4%E5%AF%B9%E8%B4%9F%E7%94%B5%E6%BA%90%E7%94%B5%E5%8E%8B%E5%92%8C%E9%A2%9D%E5%A4%96%E7%94%B5%E5%AE%B9%E5%99%A8%E7%9A%84%E9%9C%80%E8%A6%81%E5%AF%BC%E8%87%B4%E7%9A%84%E5%87%8F%E5%B0%91%E9%A9%B1%E5%8A%A8%E5%99%A8%E6%95%88%E7%8E%87%E3%80%82    

        
https://zhuanlan.zhihu.com/p/416919304  
    
https://blog.csdn.net/u013024788/article/details/128719287    


驱动板原理图如下：  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/00bdb554-f2dd-4980-95e8-d3afc70c58d6)    
驱动板中，也有对输入电源进行滤波和储能并联的一排电容，同时该电容的封装均选取较大的，如1206封装。  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/049725fb-bd1f-47b3-8122-99bd12c3be48)  

方案三采用的MCU为STM32F405RGT6,届时可根据情况进行替换。  




###  总结  
目前暂定参考以上三种方案，如有新方案可以继续添加  

#  2023-12-29  
##  方案解析与理解：  
方案一：  
链接：  https://oshwhub.com/Knight_Sin/abcd   
作者在项目中有说到该项目是有以学习为目的，硬件为了契合ART-PI，ART-PI是RT-THREAD团队设计的一款具有扩展功能的DIY开源硬件。  致力打造一个开源的软硬件平台。  
github 仓库地址 ： https://github.com/RT-Thread-Studio/sdk-bsp-stm32h750-realthread-artpi    

gitee 仓库地址 ： https://gitee.com/mirrors/ART-Pi   
方案一中有CAN接口，TYPEC通信接口，SPI FLASH电路以及一个IPS屏幕，是一个功能比较完整的板子。  

关于电流采样，有以下关于FOC三种电流采样资料可供参考：  
https://zhuanlan.zhihu.com/p/357517561?utm_id=0  
  
###  1.1 总体思路  
MCU通过PWM输出控制电机驱动芯片对电机进行驱动  
在该工程中，PWM输出分为高边和低边。  



#  2024-01-02  
##  今天是2024年的第一个工作日，祝大家新年快乐，新的一年，更上一层楼。     



#  2024-01-03   
##  今日任务  
###  今日对键盘防护箱屏幕驱动板做Layout工作。  


#  2024-01-04  
##  今日任务  
###  继续Layout  腰更痛了今天  


#  2024-01-05  
##  今日任务  
###  继续Layout，在关于DC-DC电源Layout当中，有些许注意事项，比如输入滤波和输出滤波回路要短，要注意线路过长造成的杂散电感，在输出部分当中，往往有一个输出电感可以滤除回路产生的杂散电感，所以更需要注意输入端的电流回路以及其产生的杂散电感，如下：  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/3a7eb642-9261-4b5c-b3d5-3f81404a5fe0)  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/46ba06e1-7673-456c-af06-e9b6b527e463)  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/af6f9a4f-1e65-4ee1-98e7-ee4de08d1b6a)  

相关链接：  

https://zhuanlan.zhihu.com/p/161680198    



#  2024-01-08  
##  今日任务  
###  继续键盘防护箱屏幕驱动板设计修改及其Layout  


#  2024-01-09  
##  今日  
###  Layout已完成，但是在原理图上仍然有很多细节没有注意到，希望尽快投入FOC的项目中去。  

#  2024-01-10  
##  今日  
###  已发送做板，今日整理BOM表顺顺便FOC，腰好痛  



#  2024-01-11  
##  今日任务  
###  1、FOC;2、腰痛恢复了一点点，但大体上还是比较痛；  

##  注意事项  
在关于PWM驱动输出的时候，一定考虑PWM死区的问题，对于同一臂的两个MOS管切记不可同时导通，在PWM输出时候要设置相应的延时，参考链接如下：  
https://zhuanlan.zhihu.com/p/476600889  

关于MOSFET和IGBT的对比，以下是相关链接：  
https://zhuanlan.zhihu.com/p/668408627#:~:text=%E4%B8%80%E8%88%AC%E6%83%85%E5%86%B5%E4%B8%8B%EF%BC%8CMOSFET%E7%9A%84%E6%B8%A9%E5%BA%A6%E7%B3%BB%E6%95%B0%E6%AF%94IGBT%E4%BD%8E%EF%BC%8C%E5%9C%A8%E9%AB%98%E6%B8%A9%E7%8E%AF%E5%A2%83%E4%B8%8BMOSFET%E7%9A%84%E6%80%A7%E8%83%BD%E6%9B%B4%E5%8A%A0%E7%A8%B3%E5%AE%9A%E3%80%82%20%E7%94%B1%E4%BA%8E%E4%B8%8D%E5%90%8C%E7%9A%84%E8%AE%BE%E8%AE%A1%E5%8E%9F%E7%90%86%EF%BC%8CIGBT%E7%9A%84%E6%AD%A3%E5%90%91%E7%94%B5%E5%8E%8B%E9%99%8D%E8%BE%83MOSFET%E5%A4%A7%E3%80%82%20%E4%B8%8D%E8%BF%87%E5%9C%A8%E8%AE%B8%E5%A4%9A%E5%BA%94%E7%94%A8%E4%B8%AD%EF%BC%8CIGBT%E7%9A%84%E9%AB%98%E5%8E%8B%E9%99%8D%E4%BC%9A%E8%A2%AB%E5%85%B6%E4%BB%96%E4%BC%98%E7%82%B9%EF%BC%8C%E5%A6%82%E9%AB%98%E7%9E%AC%E6%80%81%E7%94%B5%E5%8E%8B%E6%89%BF%E5%8F%97%E8%83%BD%E5%8A%9B%E5%92%8C%E8%BE%83%E4%BD%8E%E7%9A%84%E5%AF%BC%E9%80%9A%E6%8D%9F%E8%80%97%E6%89%80%E6%8A%B5%E6%B6%88%E6%8E%89%E3%80%82,%E5%9B%A0%E6%AD%A4%E5%9C%A8%E9%AB%98%E7%94%B5%E5%8E%8B%E5%BA%94%E7%94%A8%E4%B8%AD%EF%BC%8CIGBT%E6%9B%B4%E5%8A%A0%E9%80%82%E7%94%A8%E3%80%82%20%E6%88%90%E6%9C%AC%E6%96%B9%E9%9D%A2%20IGBT%E5%9B%A0%E4%B8%BA%E5%88%B6%E9%80%A0%E8%BF%87%E7%A8%8B%E5%A4%8D%E6%9D%82%E4%B8%94%E5%8A%9F%E7%8E%87%E5%A4%84%E7%90%86%E8%83%BD%E5%8A%9B%E9%AB%98%EF%BC%8C%E6%88%90%E6%9C%AC%E9%AB%98%E4%BA%8EMOSFET%E3%80%82%20%E5%9B%A0%E6%AD%A4%E5%9C%A8%E4%B8%80%E4%BA%9B%E6%88%90%E6%9C%AC%E6%95%8F%E6%84%9F%E7%9A%84%E5%BA%94%E7%94%A8%EF%BC%8C%E4%BD%8E%E5%8A%9F%E7%8E%87%E5%92%8C%E9%AB%98%E5%BC%80%E5%85%B3%E9%A2%91%E7%8E%87%E7%9A%84%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF%EF%BC%8CMOSFET%E4%BB%A5%E5%85%B6%E8%BE%83%E4%BD%8E%E7%9A%84%E6%88%90%E6%9C%AC%E5%92%8C%E7%81%B5%E6%B4%BB%E6%80%A7%E6%88%90%E4%B8%BA%E5%B7%A5%E7%A8%8B%E5%B8%88%E7%9A%84%E9%A6%96%E9%80%89%E3%80%82  





#  2024-01-12  
##  今日，提交离职申请，准备离职。  

以下是一篇关于cmake的文章，做个结尾，链接如下：  
https://zhuanlan.zhihu.com/p/371257515  







