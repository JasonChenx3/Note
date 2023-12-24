### 题目要求

![题目要求图](https://s2.loli.net/2023/10/19/vW9jpsC53UHmGqr.png)



### 分析

#### PWM相关知识

PWM全称 Pulse Width Modulation：脉冲宽度调制（简称脉宽调制，通俗的讲就是调节脉冲的宽度），是电子电力应用中非常重要的一种控制技术。

脉冲波的基本信息：

![脉冲波基本信息](https://pic4.zhimg.com/80/v2-5c632e4c1e72934d6ba508b93b15ae6b_720w.webp)

脉冲周期（T）

- 单位是时间，比如纳秒ns，微妙us，毫秒ms。

脉冲频率（f）

- 单位是赫兹（HZ），千赫兹（KHZ），兆赫兹（MHZ），f = 1 / T。

脉冲宽度（W）：

- 简称 “脉宽” ，是脉冲高电平的持续时间。
- 单位是时间，比如纳秒ns，微妙us，毫秒ms。

占空比（D）：

- 脉宽除以脉冲周期的值。
- 百分数表示，比如50%。

以上关系如下图所示：

![关系图](https://pic1.zhimg.com/80/v2-0a0e9115348c91af098b1dbd23a46854_720w.webp)

#### 本题分析

PWM脉宽频率是100HZ，那么周期就是10ms。

利用定时器0，每一次中断是1us。

设置计数初值是100，即计数100次产生一次中断，也就是100us = 0.1ms产生一次中断。

将PWM的频率分成了100份。

设置占空比，单独用一个变量来设置。

### 代码

```c
#include <STC15F2K60S2.H>
#define uchar unsigned char
#define uint unsigned int

sbit L1 = P0 ^ 0;
sbit S7 = P3 ^ 0;

void HC138(uchar channel)
{
	switch(channel)
	{
		case 4:
			P2 = (P2 & 0x1f) | 0x80;
		break;
		case 5:
			P2 = (P2 & 0x1f) | 0xa0;
		break;
		case 6:
			P2 = (P2 & 0x1f) | 0xc0;
		break;
		case 7:
			P2 = (P2 & 0x1f) | 0xe0;
		break;
	}
}

// ===============定时器相关函数===============
uchar count = 0;
uchar pwm_duty = 0;  // 占空比
void InitTimer0()
{
	// 设置定时器工作模式，注意只能字节赋值
	TMOD = 0x01;
	// 设置计数初值
	// 每1us产生一次中断，一共产生一百次
	TH0 = (65535 - 100) / 256;
	TL0 = (65535 - 100) % 256;
	// 打开中断允许
	ET0 = 1;
	EA = 1;
}

void ServiceTimer0() interrupt 1
{
	// 16位不会自动重装，需要手动重装
	TH0 = (65535 - 100) / 256;
	TL0 = (65535 - 100) % 256;
	
	count ++ ;
	
	if (count == pwm_duty)
	{
		L1 = 1;
	}
	else if (count == 100)
	{
		L1 = 0;
		count = 0;
	}
}
// ============================================

// ===============按键相关函数===============
void DelayK(uint t)  // 软件去抖动延时函数
{
	while (t -- ) ;
}

uchar stat = 0;
void ScanKeys()  // 按键扫描函数
{
	if (S7 == 0)
	{
		DelayK(500);
		if (S7 == 0)
		{
			while (S7 == 0) ;
			switch(stat)
			{
				case 0:
					L1 = 0;
					TR0 = 1;
					pwm_duty = 10;
					stat = 1;
				break;
				
				case 1:
					pwm_duty = 50;
					stat = 2;
				break;
				
				case 2:
					pwm_duty = 90;
					stat = 3;
				break;
				
				case 3:
					L1 = 1;
					TR0 = 0;
					stat = 0;
				break;
			}
		}
	}
}
// ==========================================

void main()
{
	HC138(4);
	L1 = 1;
	InitTimer0();
	while(1)
	{
		ScanKeys();
	}
}
```

