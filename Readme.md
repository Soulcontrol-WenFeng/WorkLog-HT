2023-07-04
4G板子的开发参考文件为杂项文件中2023-07-03中

2023-07-04记录
关于利用STM32CUBEIDE对stm32f030k6t6进行开发，由于原始的IDE界面是白色的太过刺眼，过下载一个DARK插件，但是下载到46%会出现错误，忙猜一手软件版本不够高导致的，先升级一手IDE，等等再进行下载。
开发问题提取1：关于stm32f030c6t6和EC800M-CN通信，利用AT指令，但是AT指令数量多的时候，在程序中进行进行判断识别的时候可能需要在switch中写许多case，或者if中写许多的if if else，会对代码的阅读和维护带来极大的不方便，这边有一种称之为消息地图的方法，巧用函数指针和结构体来对指令判断进行处理。



2023-07-05记录
关于昨日插件下载进度条一直停留在46%的问题已解决。通过升级STM32CUBEIDE再重新下载插件成功。今日任务将消息地图应用到自己的HT-IOT-LC1-C_V10工程代码中。
开发过程概念提取：
1、PPP连接：点对点协议连接





2023-07-06记录
傅哥发送的参考代码在今日的杂项文件中。
开发过程问题提取：
关于TCP连接过程中的三种方式：1、TCP客户端在缓存模式下工作；2、TCP客户端在透传模式下工作；3、TCP客户端在真土模式下工作；4、TCP服务器在缓存模式下工作；先写一份1、TCP客户端在缓存模式下工作。








2023-07-10
开发问题提取：
1、在LTE.c文件中的RegConfig函数添加到主函数中运行，报错region ‘RAM' overflowed by 104 bytes



2023-07-11
开发问题提取：
1、在测试LTE模组功能时，发现AT指令发送无响应。
问题策略：
1、优先考虑开机启动工作是否做好，发现代码中开机其中的2000MS延时不够，改为3000，模组启动成功，发送指令“ATI"有回应，重新烧入代码，问题再次出现.
问题确认：
1、测试发现串口初始化函数影响PC通过串口对LTE模组进行测试
开发问题提取：
2、发送”AT+CPIN?"显示ERROR,sim卡未插入
问题策略：
2、尝试重启
问题确认：
2、硬件电路出现问题，在原理图绘制时候将NANO Sim卡座的地连接到数据输出引脚，将数据输出引脚连接到地，已告知硬件工程师。通过断铜和飞线已经解决问题。


2023-07-13
开发问题提取：
1、测试EC800M-CN进行服务器连接并且进行注册，前期问题为设备发送的注册信息不符合要求，进行修改后，发现数据发送命令不执行，PC串口监视发送数据命令无回复，且一段时间后断开连接，实测发现串口发送AT指令后，并没有进入下一个接收处理函数。
问题确定：
1、经过检查代码发现，在MCU接收LTE模组串口发送回来的数据的时候，在判断连接是否成功的代码里面，会首先进入else的判断里面，而我在这里面做的是一个延时的空处理，当超过一定的累加时间的时候，就会进入CLOSE的处理，所以在上述所说的断开连接实际上是我的设备主动发送断开命令的。修改其中代码，将该段判断为为成功连接，重新进行连接动作。虽然现在可以成功注册服务器，但是其中较为疑问的一个点在第一次进入该else判断中的时候，PC串口是有收到LTE模组发送给我的连接成功信息。考虑可能是MCU串口接收处理有潜在的bug。


2023-07-17
开发问题提取：
1、在接收服务器下发数据的时候，进行判断会判断不到SEND OK回复，检查发现，当模组同样以十六进制字符串的形式发送回（这里我个人的理解更倾向于回显）MCU的时候数据量会超过我接收数组的定义256个，显然SEND OK在最后面并没有被拷贝到接收的buff中，导致在下一步的对AT_QISENDEX中对于是否发送成功的SEND OK 判断为错误。
问题确定：
如上，将数组个数改为512个，发送成功判断工作正常，同时要注意，这算是一个低级错误，以及在循环拷贝接收的数组时，将下标定义成十六位的变量，否则八位的变量最高只能拷贝256个，相当于重复犯了上面的低级错误。
注意事项：
1、由于将接收数组改大，执行帧中断的时间会响应变长，所以在每一次AT指令发送完成后可适当地延时一会，防止接收程序处理太快而数据没有拷贝完整。



2023-07-18
开发问题提取：
1、在连接服务器注册后，程序如果没有动作的话在一定时间后会被服务器断开。在此要设置十秒钟的心跳上报。






2023-07-24
开发问题提取：
1、在给代码中增加十秒心跳以及服务器命令接收处理函数之后，发现客户端发送注册信息会卡在缓冲转十六进制这里。
问题猜想：
1、感觉可能是定时器中断产生了影响。
问题确定：
1、注释掉定时器初始化函数，程序仍然在发送注册信息的时候卡死。该问题与定时器初始化无关。
2、尝试启用回显或者关闭回显，均无法解决。
3、尝试切回上一版代码，可以进行注册登陆。目前尚不确定问题出现在哪里。



2023-07-25
开发问题提取：
1、在移植了郑哥的循环接收代码后，发现如下问题：1、初始化过后，程序发送命令不执行，猜想可能模组没有启动成功，但是串口却有收到RDY回复，说明应该是有启动成功的。2、我们可以假定说初始化是成功的，而移植完了的接收函数是有问题的，但是串口监视并没有接收到第一条ATE0发送的回复，极大可能说明程序在一开始就已经跑飞了。
问题排除：
1、注释掉串口初始化和发送函数，保留启动函数，直接PC和模组通信，是可以通信的，说明模组启动是成功的。



2023-07-26
开发问题提取：
1、在昨天移植完代码后，发现对其中的逻辑理解不是很透彻。换回自己原本的代码，希望可以找到突破口。目前的问题是：当我向服务器发送注册信息后，服务器会回复我，这段对于我注册的回复信息MCU是有收到且拷贝出来处理的。而随着服务器发送回复信息后又接着两条命令，在PC监视端口上可以看到模组以及收到这两条消息并且通过串口发送给MCU，但是MCU中的处理函数在这个时候就没有起作用，无法进入到进行相应回复的动作当中。
问题猜想：
1、我更多的是倾向于我自己代码处理逻辑的问题。在我处理服务器回复的注册信息时，可能随后的两条指令就发送过来了，而此时中断函数虽然接收了，然后将帧中断标志位置1，而如果这时候代码正好处理完成并且标志位被置0，这样处理函数退出去后便不再进来，相当于这段紧随而来的数据被丢失。
验证方法：
1、根据猜想1进行验证，我们将处理函数一进来的中断帧标志马上置false，这样即使这段还在处理的时候，外部帧中断置true后仍然可以进来处理。
问题定位：
1、通过以上验证方法的测试，而后又发现了发送函数后延时可能会太长，于是把发送命令后的延时注释掉，然后发现设备会重复注册，猜想可能是速度太快导致第一次接收没有判断到。



2023-07-27
开发问题提取：
1、目前使用的仍然是串口帧中断的方式，问题仍然出现在服务器回复我注册信息的时候后面两条下发命令没用做处理并且回复导致服务器后面会断开我的设备。
问题确定：
1、通过测试发现，PC监视有看到模块通过串口发送的两条服务器下发指令，这个时候要么是速度太快，MCU串口没有及时拷贝，要么就是MCU串口有拷贝但是程序处理的时候由于逻辑没写好给忽略掉了，最后一种可能是有接收到也有判断到，但是发送失败导致服务器没有收到。从PC串口监视的情况来看，设备是没有发送回复的，当下的问题是确定以上几种情况的哪一种。
测试确认：
1、通过LED灯指示，可以确定注册回复信息是有处理到的；
2、三次LED灯指示测试，可以确定在注册回复信息后的服务器下发指令是有被收到且处理的（但是不能确定是两条都收到还是只收到一条），目前可以确定在处理普通下发指令后，发送动作没有执行成功或者没有执行，进行下一步测试。
3、通过LED灯指示发现，在上一个步骤完成时候，并没有按照程序处理跳到发送回复的代码段中。可能标志位被重写，或者处理发送信息的时间太长导致的程序跑丢？
4、多次测试，发现接收并且处理了注册信息回复后的指令（是一条还是两条目前不确定），没有再冲入发送指令函数的HEART状态中的发送判断处理。
5、再次测试发现，程序有回到指令发送函数中的HEART状态，但是没有进入判断是否回复其他指令的函数中。
6、继续测试发现，程序有进入指令发送函数中的HEART状态，并且落入非心跳中断当中，但是仍然没有落入判断回复命令的代码段中。
7、测试发现，程序进入指令发送函数中的HEART状态中的非心跳判断当中，但是落入了else判断中
8、测试中发现，在对于下发命令的拷贝中，如下代码所示：
	type = a2->u8Type;
          subtype = a2->u16SubType;
	reserved = a2->u32Reserved;
将LED灯指示放在该段代码前面可以点亮，放在该段代码后面不可以点亮，所以该段拷贝代码应该是有问题的。



2023-07-31
问题提取：
1、接上述问题，仍然是没有及时回复服务器下发的命令，将接收的变量类型替换成静态类型，发现 没有用，现在直接赋值进行测试；
测试确认：
还是无法解决以上问题


2023-08-01
策略更改：
同事建议我换成MQTT通信，正在构建代码中……
该模组版本不支持MQTT……………………重回TCP
问题提取：
1、重新测试发现，在模组尚未注册服务器的时候，定时器正常工作，但是注册完成后，定时器不再进入中断（通过亮灯现象）。考虑是否有程序跑飞或者是否该引入看门狗。如果引入看门狗，程序重启意味着模组又要重新注册。看门狗先不考虑。
问题确认：
1、跑飞原因是因为在定时器中断当中跑了sprintf()这个函数，导致程序跑飞进入Hardfault，将发送代码放在定时器中断外面并且多设一个标志位，心跳发送成功。




2023-08-02
问题确认：
1、经过debug调试发现，程序进入了一个叫Hardfault handler的中断函数，里面有个while(1),故认为程序进入此处导致定时器中断没用进去。
问题提取：
1、当前的问题为设备上线后，服务器下发开锁命令，然后程序进入Hardfault Handler。查询lr寄存器的值，发现是在调用主线程的时候出现问题。
问题考虑：
1、比较有可能的是定时器中断与串口中断产生冲突，但是将定时器中断优先级降低后仍然出现该现象，又将定时器中断周期由100ms改成1s后，现象仍然出现。将中断周期改成10s一次，仍然在下发命令的时候会死掉。
2、将定时器中断注释掉，发现仍会出现问题，说明该问题应该不是串口与定时器中断冲突，而是串口本身的问题。


2023-08-03
问题重现：
1、昨天在AT指令接收函数当中，在拷贝完串口传来的数据到buff后，将rx_buffer清零，设备有接收到服务器命令并且不会进入hardfault handler，开锁成功。但是由于发送回复后的代码没处理好导致被断，今早添加完该部分代码后，昨天的问题仍然重现。显然昨天的成功是偶发性的，继续排查
2、在调换串口线收发查看MCU是否发送数据的时候，发现有判断到下发开门命令，继电器开启，但是一直重复发送，然后重新上电后调换串口收发线程序又死掉，
问题确认：
1、经测试发现程序跑飞是在开门命令判断的时候产生的
if(a2->u8Type == 0x01 && a2->u16SubType == 0X4010)
 a2->u16SubType == 0X4010 将该条判断去掉可以正常判断，尚不清楚问题出在哪里




2023-08-09
代码更新：
目前版本HT-IOT-LC1-C_V117_1为最新可用版本，标记。




# 2023-10-10  
## 代码随笔  
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



