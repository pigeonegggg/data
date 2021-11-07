# *灯**

头文件：<regx52.h>

- ## l 亮灯：



![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

<img src="C:\Users\5700h\AppData\Roaming\Typora\typora-user-images\image-20211107224210819.png" alt="image-20211107224210819" style="zoom:50%;" />

l P2.0与D1连，控制P2.0端低电平，led灯前后有电势差，亮

l C51扩充数据类型--特殊功能寄存器：

sfr：声明一个8位的寄存器

sfr16：声明一个16位的寄存器

sfr(special funcTlon register)用于将一个单片机的特殊功能寄存器赋给一个变量，这样在后面的程序就可以通过这个变量指引该寄存器。

sbit：声明特殊功能位 一位

- 代码：

  1.

\

~~~c
```#include<regx52.h>

int main()

{

P2=0x7f;//0111 1111

return 0;

}
~~~

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

8位，0111 1111

 

2.

\

~~~c
```#include<regx52.h>

sbit led1=P2^0;

int main()

{

	led1=0;

	return 0;

}


~~~

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

一位，0或1……

## l 闪烁

再死循环中控制灯先亮再暗，因交替速度太快，看起来像不闪烁

解决：交替中间加延迟（延迟就是给单片机循环空语句）

延迟：可以由stc生成一个延迟1ms的代码，再封装成一个函数，参数是x毫秒

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

l 流水灯

\1.    使用_crol_或_cror_函数(到达最后一位时不会移出去)

\2.    使用数组（存储一段时间的亮灯的状态）

\3.    使用左/右移

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

## l 按键控制灯亮

注意：按下机械按键会有抖动，可通过延时来消除抖动

实际是等待一段时间，什么都不干，直到抖动过去

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

 

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

           Delay(20);//松手后，从空循环跳出，消除抖动

           P2<<=1;//执行左移操作

       }   

    }
```

## l 判断上拉/下拉电阻，模块作用时输出电平高/低

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image018.jpg)

上拉电阻位置：电源vcc到引脚

作用：使得平时（不作用时）引脚位置为高电平

下拉电阻位置：地到引脚

作用：使得平时（不作用时）引脚位置为低电平

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image020.jpg)

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image022.jpg)

 

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image024.jpg)

所以：P1^0=0时，灯亮

按键：

![img](file:///C:/Users/5700h/AppData/Local/Temp/msohtmlclip1/01/clip_image026.jpg)按下按键时，P31直接接地，引脚为低电平。

所以：P3^1=0时，按键被按下

 