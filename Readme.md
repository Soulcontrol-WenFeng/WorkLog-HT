

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

旁路（bypass）电容：是把输入信号中的高频成分当作滤除对象  
去耦（decoupling））电容：去耦电容是电路中装设在元件的电源端的电容，此电容可以提供较稳定的电源，同时也可以降低元件耦合到电源端的噪声，间接可以减少其他元件受此元件噪声的影响。  
![image](https://github.com/Soulcontrol-WenFeng/WorkLog-HT/assets/74033919/f76f103c-b5bd-45b9-ae76-83cd87c91dae)  



  



