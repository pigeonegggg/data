# *灯*

头文件：<regx52.h>

## 亮灯

<img src="C:\Users\5700h\Pictures\Saved Pictures\a.png" style="zoom:50%;" />



<img src="C:\Users\5700h\AppData\Roaming\Typora\typora-user-images\image-20211107224210819.png" alt="image-20211107224210819" style="zoom: 50%;" />

<!--P2.0与D1连，控制P2.0端低电平，led灯前后有电势差，亮-->

- 代码：

  1.



~~~c
#include<regx52.h>

void main()

{

P2=0x7f;//0111 1111

}
~~~

<img src="C:\Users\5700h\Pictures\1.png" alt="1" style="zoom:50%;" />

8位，0111 1111

<!--P2可位寻址，用P2^0……表示该位的高低电平，也可用P2=0xXX表示8位的高低电平-->

 

2.



~~~c
#include<regx52.h>

sbit led1=P2^0;

void main()

{

	led1=0;

}


~~~

<img src="C:\Users\5700h\Pictures\2.png" alt="2" style="zoom:50%;" />

一位，0或1……

## 闪烁

在死循环中控制灯先亮再暗再亮，因交替速度太快，看起来像持续亮

解决：交替中间加延迟（延迟就是给单片机循环空语句）

延迟：

可以由stc生成一个延迟1ms的代码，再封装成一个函数，参数是x毫秒；

也可直接从网上找对应波特率的延迟函数

delay（xms）

- 代码：

 

```c
void Delay(unsigned int xms)       //@12.000MHz 

{

	unsigned char i, j;
	while(--xms) //循环一次是1ms，循环xms次就是x毫秒，--xms为0，就退出循环

	{i = 12;

	j = 169;

	do

	{

	while (--j);

	} while (--i);

	}

}
```

## 流水灯

1. 使用_crol_或_cror_函数(到达最后一位时不会移出去)

2. 使用数组（存储一段时间的亮灯的状态）

3. 使用左/右移

```c
void main()
{
    P2=0x7f;
    Delay(50);
    while(1)
    {
        P2=_crol_(P2,1);//数据左移1位，灯右一位亮
        Delay(50);
    }
}
```



## 按键控制灯亮

注意：按下机械按键会有抖动，可通过延时来消除抖动

实际是等待一段时间，什么都不干，直到抖动过去

<img src="C:\Users\5700h\Pictures\3.png" alt="3" style="zoom: 33%;" />

 要使用机械按键，首先要消除按键抖动，否则在抖动期间按键对应端口的电平忽高忽低，被判断为快速按按键。

```c
void main()

{

    int m=0;

    P2=0xff;

    while(1)

    {

       if(K1==0)   //按下按键

       {

           Delay(20);// 消除抖动

           while(K1==0);//保持按下状态，空循环，不操作

           Delay(20);//松手后，从空循环跳出，等待抖动结束

           P2<<=1;//执行左移操作

       }   

    }
```

步骤：

1.配置好独立按键，给两个独立按键两个返回值，比如：按下按键1就返回1，按下按键2就返回2

2.在主函数中给对应的返回值执行对应的操作，比如：返回值为1就让第一盏灯亮或灭

## 判断模块作用时输出电平高/低

<img src="C:\Users\5700h\Pictures\4.png" alt="4" style="zoom: 67%;" />

<img src="C:\Users\5700h\Pictures\5.png" alt="5" style="zoom: 67%;" />

上拉电阻位置：电源vcc到引脚

作用：使得平时（不作用时）引脚位置为高电平

下拉电阻位置：地到引脚

作用：使得平时（不作用时）引脚位置为低电平

<img src="C:\Users\5700h\Pictures\6.png" alt="6" style="zoom: 67%;" />

<img src="C:\Users\5700h\Pictures\7.png" alt="7" style="zoom:50%;" />

灯：

<img src="C:\Users\5700h\Pictures\8.png" alt="8" style="zoom: 33%;" />

P1^0=0时，灯亮

按键：

<img src="C:\Users\5700h\Pictures\9.png" alt="9" style="zoom: 33%;" />

按下按键时，P31直接接地，引脚为低电平。

所以：P3^1=0时，按键被按下

 
