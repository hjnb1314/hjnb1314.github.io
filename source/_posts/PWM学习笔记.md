---
title: PWM学习笔记
categories:
  - 嵌入式学习笔记
top_img: ./img/index.jpg
---

# 深入理解PWM（脉宽调制）

## 1. 什么是PWM？

PWM，即脉宽调制（Pulse Width Modulation），是一种电子技术，通过调节电信号的高电平持续时间和低电平持续时间来控制输出信号的强度或表现形式。它是一种周期性的波形，周期内包含了高电平和低电平两种状态。

## 2. PWM的重要参数是什么？

在PWM中，有两个非常重要的参数：
- 周期（T）：表示PWM波形一个完整周期所持续的时间长度。
- 占空比（Duty Cycle）：指高电平在一个周期内所占的比例，通常用百分比表示。

## 3. PWM的应用场景有哪些？

PWM技术被广泛应用于各种领域，包括：
- 通信领域的脉宽调制技术。
- LED照明领域中，可以通过PWM波形来调节LED灯的亮度，实现调光效果。
- 控制蜂鸣器、电机等设备，控制其工作状态和频率。

## 4. PWM波形是如何产生的？

### 4.1 基本原理

PWM波形的生成基于定时器和GPIO引脚。通过配置定时器产生高低电平信号，并通过GPIO引脚输出，从而形成PWM波形。

### 4.2 早期实现方式

在早期的简单单片机中，由于没有专用的PWM定时器，需要手动结合GPIO和定时器模块来生成PWM波形。

### 4.3 现代实现方式

现代单片机通常内置了PWM模块，将定时器和GPIO引脚绑定，通过寄存器配置即可产生PWM波形，操作更加简便。

## 5. 生成PWM波形的步骤是怎样的？

1. 设置PWM的周期和占空比。
2. 系统时钟通过二级分频后与周期相乘，得到PWM波形的实际周期。
3. 根据设定的占空比，确定PWM波形的高电平持续时间和低电平持续时间。

## 6. 注意事项

- 在配置PWM时，需要根据实际需求设置合适的周期和占空比，以达到所需的控制效果。
- 系统时钟的设置和分频情况也需要考虑，以确保PWM波形的稳定和准确输出。

通过了解PWM的原理和应用，我们可以更好地利用这项技术来实现各种电子设备的控制和调节。

## 7.stm32关于pwm
![stm32关于pwm](https://s2.loli.net/2024/03/12/HUxKWMLRZapGY41.jpg)

## 8.代码步骤
1.开启时钟源
```/*开启时钟*/
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);			//开启TIM2的时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);			//开启GPIOA的时钟
```
2.GPIO初始化配置
```
/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);		
```

3.配置时钟源 TIM
```
TIM_InternalClockConfig(TIM2);		//选择TIM2为内部时钟，若不调用此函数，TIM默认也为内部时钟
	
	/*时基单元初始化*/
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;				//定义结构体变量
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;     //时钟分频，选择不分频，此参数用于配置滤波器时钟，不影响时基单元功能
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式，选择向上计数
	TIM_TimeBaseInitStructure.TIM_Period = 100 - 1;                 //计数周期，即ARR的值
	TIM_TimeBaseInitStructure.TIM_Prescaler = 36 - 1;               //预分频器，即PSC的值
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;            //重复计数器，高级定时器才会用到
	TIM_TimeBaseInit(TIM2, &TIM_TimeBaseInitStructure);             //将结构体变量交给TIM_TimeBaseInit，配置TIM2的时基单元
```

4.输出比较初始化
```
TIM_OCInitTypeDef TIM_OCInitStructure;							//定义结构体变量
	TIM_OCStructInit(&TIM_OCInitStructure);                         //结构体初始化，若结构体没有完整赋值
	                                                                //则最好执行此函数，给结构体所有成员都赋一个默认值
	                                                                //避免结构体初值不确定的问题
	TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;               //输出比较模式，选择PWM模式1
	TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;       //输出极性，选择为高，若选择极性为低，则输出高低电平取反
	TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;   //输出使能
	TIM_OCInitStructure.TIM_Pulse = 0;								//初始的CCR值
	TIM_OC3Init(TIM2, &TIM_OCInitStructure);                        //将结构体变量交给TIM_OC3Init，配置TIM2的输出比较通道3
```

5.TIM使能
/TIM使能/

TIM_Cmd(TIM2, ENABLE); //使能TIM2，定时器开始运行

6.强制修改CCR（影响占空比）的值
```
TIM_SetCompare3(TIM2, Compare);		//设置CCR3的值
```