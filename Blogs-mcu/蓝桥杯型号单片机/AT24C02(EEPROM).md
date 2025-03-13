## AT24C02模块
### 底层原理
![img](https://img2023.cnblogs.com/blog/3583913/202503/3583913-20250307161323794-596300804.png)
![img](https://img2023.cnblogs.com/blog/3583913/202503/3583913-20250307161340969-1638112690.png)
原理图
![img](https://img2023.cnblogs.com/blog/3583913/202503/3583913-20250307161448843-2096772438.png)
存储结构
![img](https://img2023.cnblogs.com/blog/3583913/202503/3583913-20250307161917652-2136966755.png)
### 代码编写
![img](https://img2023.cnblogs.com/blog/3583913/202503/3583913-20250307165146958-1616690513.png)
```cpp
//页写入
void EEPROM_Write(unsigned char* EERPROM_String, unsigned char addr, num){  //addr必须是8的倍数，num最大为8
    IIC_Start(); //启动信号
   
    IIC_SendByte(0xA0);  //选择EEPROM，确定 写 的模式
    IIC_WaitAck();

    IIC_SentByte(addr); //写入要存储的地址
    IIC_WaitACk();

    while(num --){
        IIC_SendByte(*EEPROM_String ++);
        IIC_WaitAck();
        IIC_Delay(200);
    }
    IIC_Stop();
}
//页读出
void EEPROM_Read(unsigned char *EEPROM_Sring, unsigned char addr, unsigned char num){
    IIC_Start();
    IIC_SendByte(0x40); //确定 写 模式
    IIC_WAitAck();

    IIC_SendByte(addr); //写入要读数据的地址
    IIC_WaitAck();
    
    IIC_Start();  
    IIC_SendByte(0x41);  //确定 读 模式
    IIC_WaitAck();

    while(num --){
        *EEPROM_Sting ++ = IIC_Recbyte();
        if(num) IIC_SendAck(0); //发送应答，继续传送剩余数据
        else IIC_SendAck(1); //不发送应答，结束读出
    }
    IIC_Stop();
}


//单字节写入与读出

//================24C02单字节写入=================
void Write_24C02(unsigned char addr, unsigned char dat)
{
  IIC_Start();          //起始信号
  IIC_SendByte(0xa0);    //EEPROM的写设备地址
  IIC_WaitAck();        //等待从机应答
  IIC_SendByte(addr);    //内存单元地址
  IIC_WaitAck();        //等待从机应答
  IIC_SendByte(dat);    //内存写入数据
  IIC_WaitAck();        //等待从机应答
  IIC_Stop();            //停止信号
}
//================24C02单字节读取=================
unsigned char Read_24C02(unsigned char addr)
{
  unsigned char tmp;
  //首先，进行一个伪写操作，主要为送出读的地址
  IIC_Start();          //起始信号
  IIC_SendByte(0xa0);    //EEPROM的写设备地址
  IIC_WaitAck();        //等待从机应答
  IIC_SendByte(addr);    //送出要读的内存单元地址
  IIC_WaitAck();        //等待从机应答
  //然后，开始字节读操作
  IIC_Start();          //起始信号
  IIC_SendByte(0xa1);    //EEPROM的读设备地址
  IIC_WaitAck();        //等待从机应答
  tmp = IIC_RecByte();  //读取内存中的数据
  IIC_SendAck(1);        //产生非应答信号
  IIC_Stop();            //停止信号
  return tmp;
}

```