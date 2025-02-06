## 实现单片机按键控制点灯 
##### 2025/1/28
>单片机第二课，能通过判断i/o接口的值（由原理图可知，按键按下i/o接口接受值为0）判断是哪个按键被按下。
![img](https://img2023.cnblogs.com/blog/3583913/202501/3583913-20250128090434835-858079307.png)
![img](https://img2023.cnblogs.com/blog/3583913/202501/3583913-20250128090612981-930264987.png)

>按键的操作分为按键下降沿，上升沿和是否一直按下三个状态。代码实现如下：
```cpp
//变量声明
unsigned char key_val,key_down,key_up,key_old;//全局变量
//读按键函数
unsigned char key_read(){
	unsigned char temp=0;
	if(P3_4==0) temp=1;
	if(P3_4==0) temp=2;
	if(P3_4==0) temp=3;
	if(P3_4==0) temp=4;
	return temp;
}
//按键的操作的计算
key_val=key_read();
key_down=key_val&(key_val^key_old);
key_up=~key_val&(key_val^key_old);
key_old=key_val;
```
