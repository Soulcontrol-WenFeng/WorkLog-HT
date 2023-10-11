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
```
在电子电路中，去耦电容和旁路电容都是起到抗干扰的作用，电容所处的位置不同，称呼就不一样了。对于同一个电路来说，旁路（bypass）电容是把输入  
信号中的高频噪声作为滤除对象，把前级携带的高频杂波滤除，而去耦（decoupling）电容也称退耦电容，是把输出信号的干扰作为滤除对象。去耦电容用  
在放大电路中不需要交流的地方，用来消除自激，使放大器稳定工作  
```
`(1)旁路（bypass）电容：是把输入信号中的高频成分当作滤除对象`  
`(2)去耦（decoupling））电容：去耦电容是电路中装设在元件的电源端的电容，此电容可以提供较稳定的电源，同时也可以降低元件耦合到电源端的噪声，间接可以减少其他元件受此元件噪声的影响`    
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/f76f103c-b5bd-45b9-ae76-83cd87c91dae)  


### 二、功能  
#### 1、去耦电容作用  
`（1）去除高频信号干扰；`  
`（2）蓄能作用；`    
在电子电路中，去耦电容和旁路电容都是起到抗干扰的作用，电容所处的位置不同，称呼就不一样了。对于同一个电路来说，旁路（bypass）电容是把输入信号中的高频噪声作为滤除对象，把前级携带的高频杂波滤除，而去耦（decoupling）电容也称退耦电容，是把输出信号的干扰作为滤除对象。去耦电容用在放大电路中不需要交流的地方，用来消除自激，使放大器稳定工作。  

  



