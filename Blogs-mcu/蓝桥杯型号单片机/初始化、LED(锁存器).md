## 原理
![img](https://img2023.cnblogs.com/blog/3583913/202502/3583913-20250213155709150-1818692442.png)
其中M74HC573M1R为锁存器芯片，0表示LED导通。
#### 锁存器基础知识
[CSDN关于SR锁存器的讲解](https://blog.csdn.net/qian_yz/article/details/119358006)

[知乎关于D型锁存器的讲解](https://zhuanlan.zhihu.com/p/602904924) 注：透明状态（E=1）： 当控制信号（E）为1时，锁存器处于透明状态，意味着输入端D的值直接影响输出端Q。这个状态相当于锁存器的“门”是打开的，输入信号可以“自由通过”，输出会实时跟随输入变化。这是因为在这种状态下，锁存器没有对输入信号进行任何“存储”或“保持”，它只是一个简单的传输元件。

**M74HC573M1R为锁存器芯片** 
![img](https://img2023.cnblogs.com/blog/3583913/202502/3583913-20250213170256603-2081870984.png)

**74HC138译码器**
![img](https://img2023.cnblogs.com/blog/3583913/202502/3583913-20250213171246122-1541143652.png)

![img](https://img2023.cnblogs.com/blog/3583913/202502/3583913-20250214085658481-574556534.png)

**ULN2003**
![img](https://img2023.cnblogs.com/blog/3583913/202502/3583913-20250214090715010-521612343.png)




## LED代码
```cpp
//Init.h
#ifndef __INIT_H__
#define __INIT_H
void System_Init();
#endif
//Init.c
#include <STC15F2K60S2.H>
void System_Init(){
    //关闭LED
    P0 = 0xff;
    P2 = P2 & 0x1f | 0x80;
    P2 = P2 & 0x1f;
    //关闭蜂鸣器、继电器等
    P0 = 0x00;
    P2 = P2 & 0x1f | 0xA0;
    P2 = P2 & 0x1f;
}
//Led.h
#ifndef __LED_H__
#define __LED_H__
void Led_Disp(unsigned char addr, enable);
#endif
//Led.c
#include <STC15F2K60S2.H>
void Led_Disp(unsigned char addr, enable){
    //开关（enable = 0/1）某个Led的tmp值
    static unsigned char tmp = 0x00;
    static unsigned char tmp_old = 0xff;
    if(enable)
        tmp |= 0x01 << addr; 
    else
        tmp &= ~(0x01 << addr);
   //将tmp值赋给P0端口（条件tmp != tmp_old）
   if(tmp != tmp_old){
        //注意注意注意
        P0 = ~tmp;   //注意注意注意
        P2 = P2 & 0x1f | 0x80;
        P2 = P2 & 0x1f;
        tmp_old = tmp;
    }
}
```