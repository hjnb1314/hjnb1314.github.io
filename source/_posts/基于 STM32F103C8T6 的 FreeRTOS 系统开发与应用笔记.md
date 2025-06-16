---
title: 基于 STM32F103C8T6 的 FreeRTOS 系统开发与应用笔记
categories:
  - 嵌入式学习笔记
top_img: ../img/index.jpg
---

# FreeRTOS 在 STM32F103C8T6 上的移植与应用笔记



## 第一章：FreeRTOS 移植到 STM32

FreeRTOS 是一个小巧、可裁剪、开源的实时操作系统，广泛应用于嵌入式系统中。STM32F103C8T6 是一款基于 ARM Cortex-M3 内核的微控制器，具备较强的处理能力和丰富的外设资源，适合移植 FreeRTOS 进行多任务调度控制。

### 1.1 准备工作

移植 FreeRTOS 之前需要完成以下准备工作：
- 安装 STM32CubeMX 和 Keil MDK 开发环境。
- 下载 FreeRTOS 源码包或通过 CubeMX 自动配置。
- 确保已有基本裸机程序运行（例如串口输出正常）。

### 1.2 使用 STM32CubeMX 创建工程

1. 打开 STM32CubeMX，选择 STM32F103C8Tx 芯片。
2. 配置时钟系统，确保主频设置为 72MHz。
3. 启用 USART（用于调试输出）和 SysTick（系统节拍来源）。
4. 在 Middleware 中启用 FreeRTOS，选择 CMSIS_V1 接口。
5. 配置任务参数，例如任务名、栈大小、优先级。
6. 生成工程并在 Keil 中打开。

### 1.3 工程结构

CubeMX 会生成一个包含 FreeRTOS 的完整项目结构，主要目录包括：
- `Core/Src`：用户源文件，如 main.c
- `Middlewares/Third_Party/FreeRTOS`：FreeRTOS 源码
- `freertos.c / freertos.h`：任务创建和初始化函数

### 1.4 配置文件 FreeRTOSConfig.h

需要重点关注以下宏定义：

```c
#define configUSE_PREEMPTION                    1  // 使用抢占式调度
#define configCPU_CLOCK_HZ                      (72000000)
#define configTICK_RATE_HZ                      ((TickType_t)1000) // 1ms 节拍
#define configMAX_PRIORITIES                    5  // 任务优先级等级
#define configMINIMAL_STACK_SIZE                ((uint16_t)128)
#define configTOTAL_HEAP_SIZE                   ((size_t)10240)
#define configUSE_IDLE_HOOK                     0
#define configUSE_TICK_HOOK                     0
#define configUSE_MALLOC_FAILED_HOOK            1
#define configCHECK_FOR_STACK_OVERFLOW          1
```

### 1.5 启动调度器

在 main 函数中，通过创建任务并调用 `osKernelStart()`（或 `vTaskStartScheduler()`）启动调度器。

```c
int main(void)
{
    HAL_Init();
    SystemClock_Config();
    MX_USART1_UART_Init();
    MX_FREERTOS_Init();
    osKernelStart();
    while (1);
}
```

---

## 第二章：FreeRTOS 创建任务

FreeRTOS 任务（Task）是程序执行的基本单元。每个任务都有自己的栈空间、任务函数和运行状态，FreeRTOS 会根据优先级和就绪状态进行调度。

### 2.1 任务函数格式

```c
void StartTask(void *argument)
{
    for(;;)
    {
        // 任务循环内容
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
        vTaskDelay(500); // 延时500ms
    }
}
```

### 2.2 创建任务

使用 API `xTaskCreate()` 创建任务：

```c
xTaskCreate(StartTask,     // 任务函数
            "LEDTask",     // 名称
            128,           // 栈大小（word）
            NULL,          // 传入参数
            2,             // 优先级
            NULL);         // 任务句柄
```

或者在 CubeMX 生成的 `freertos.c` 文件中：

```c
osThreadId_t ledTaskHandle;
const osThreadAttr_t ledTask_attributes = {
  .name = "ledTask",
  .stack_size = 128 * 4,
  .priority = (osPriority_t) osPriorityNormal,
};
ledTaskHandle = osThreadNew(StartTask, NULL, &ledTask_attributes);
```

### 2.3 栈大小与任务优先级

栈大小单位为字（word），通常设置为 128 或更高，避免栈溢出。优先级从 0（最低）开始，数值越大优先级越高。

### 2.4 启动调度器

在所有任务创建完成后，必须调用调度器：

```c
vTaskStartScheduler();
```

若不调用此函数，任务不会被执行。

---

## 第三章：FreeRTOS 任务管理

FreeRTOS 提供了丰富的任务管理函数，可以实现任务的挂起、恢复、删除等控制。合理使用任务管理函数可以提升系统运行效率并减少资源浪费。

### 3.1 任务删除

使用 `vTaskDelete()` 可以删除任务。参数为 NULL 表示删除当前任务：

```c
vTaskDelete(NULL); // 删除自己
```

也可以删除其他任务：

```c
vTaskDelete(taskHandle); // 删除指定任务
```

被删除的任务将从调度器中移除，资源不会自动释放，建议提前完成清理。

### 3.2 任务挂起与恢复

挂起任务：

```c
vTaskSuspend(taskHandle);
```

恢复任务：

```c
vTaskResume(taskHandle);
```

使用挂起与恢复机制，可以临时阻止某个任务的调度，常用于任务间的同步与控制。

### 3.3 获取任务状态

使用 `eTaskGetState()` 可获取任务当前状态：

```c
#include "task.h"
eTaskState state = eTaskGetState(taskHandle);
```

常见任务状态包括：
- `eRunning`：正在运行
- `eReady`：就绪状态
- `eBlocked`：等待事件（如延时）
- `eSuspended`：被挂起
- `eDeleted`：已删除

### 3.4 空闲任务与钩子函数

系统在无任务可运行时会运行默认的空闲任务。可启用空闲钩子函数（Idle Hook）用于执行后台清理等操作。

启用方式：

```c
#define configUSE_IDLE_HOOK 1
```

实现钩子函数：

```c
void vApplicationIdleHook(void)
{
    // 空闲任务运行时执行的函数
}
```

同理可启用系统节拍钩子（Tick Hook）用于定时操作。

### 3.5 延时与周期调度

`vTaskDelay()` 使任务延时指定时间，单位为系统节拍（tick）。

```c
vTaskDelay(pdMS_TO_TICKS(1000)); // 延时1000ms
```

也可使用 `vTaskDelayUntil()` 实现精确定时：

```c
TickType_t xLastWakeTime = xTaskGetTickCount();
for(;;)
{
    DoSomething();
    vTaskDelayUntil(&xLastWakeTime, pdMS_TO_TICKS(100));
}
```

---


# 第四章：FreeRTOS 消息队列

消息队列（Queue）是 FreeRTOS 中任务间通信的重要机制。它允许一个任务或中断服务程序将数据发送到队列中，另一个任务从队列中读取数据，从而实现任务之间的同步与信息传递。

## 4.1 创建队列

```c
QueueHandle_t xQueue;
xQueue = xQueueCreate(10, sizeof(uint32_t));
```

- 第一个参数表示队列长度（可存放元素数量）。
- 第二个参数为每个元素的字节大小。

## 4.2 发送数据到队列

```c
uint32_t data = 123;
xQueueSend(xQueue, &data, portMAX_DELAY);
```

也可使用非阻塞方式：

```c
xQueueSend(xQueue, &data, 0);
```

## 4.3 从队列接收数据

```c
uint32_t recv;
xQueueReceive(xQueue, &recv, portMAX_DELAY);
```

或非阻塞：

```c
if(xQueueReceive(xQueue, &recv, 0) == pdPASS)
{
    // 成功读取数据
}
```

## 4.4 队列使用建议

- 队列元素建议为结构体或基本类型。
- 建议在中断中使用 `xQueueSendFromISR()`。
- 队列可用于任务间状态传递、数据共享等。

---

# 第五章：FreeRTOS 信号量

信号量（Semaphore）用于任务同步或中断与任务之间的通信控制，是一种二值机制。

## 5.1 创建二值信号量

```c
SemaphoreHandle_t xSemaphore;
xSemaphore = xSemaphoreCreateBinary();
```

## 5.2 获取与释放信号量

```c
xSemaphoreTake(xSemaphore, portMAX_DELAY);  // 获取信号量
xSemaphoreGive(xSemaphore);                 // 释放信号量
```

## 5.3 中断中使用

```c
BaseType_t xHigherPriorityTaskWoken = pdFALSE;
xSemaphoreGiveFromISR(xSemaphore, &xHigherPriorityTaskWoken);
portYIELD_FROM_ISR(xHigherPriorityTaskWoken);
```

---

# 第六章：FreeRTOS 互斥量

互斥量（Mutex）是特殊的信号量，用于保护共享资源，防止资源竞争。

## 6.1 创建互斥量

```c
SemaphoreHandle_t xMutex;
xMutex = xSemaphoreCreateMutex();
```

## 6.2 使用互斥量

```c
xSemaphoreTake(xMutex, portMAX_DELAY);
// 临界区
xSemaphoreGive(xMutex);
```

## 6.3 与二值信号量区别

- 互斥量支持优先级继承（防止优先级反转）。
- 通常用于串口、SPI、LCD 等资源访问控制。

---

# 第七章：FreeRTOS 事件组

事件组（Event Group）用于多任务之间的标志位同步，一个事件组包含多个 bit 位，每个 bit 表示一个事件。

## 7.1 创建事件组

```c
EventGroupHandle_t xEventGroup;
xEventGroup = xEventGroupCreate();
```

## 7.2 设置与清除事件位

```c
xEventGroupSetBits(xEventGroup, BIT_0);
xEventGroupClearBits(xEventGroup, BIT_0);
```

## 7.3 等待事件位

```c
EventBits_t bits;
bits = xEventGroupWaitBits(xEventGroup, BIT_0 | BIT_1, pdTRUE, pdFALSE, portMAX_DELAY);
```

- `pdTRUE` 表示等待成功后清除事件位。
- `pdFALSE` 表示“任一”事件满足就返回。

---

# 第八章：FreeRTOS 任务通知

任务通知是效率更高的任务通信机制，用于替代二值信号量或事件组。

## 8.1 等待通知

```c
ulTaskNotifyTake(pdTRUE, portMAX_DELAY);
```

- `pdTRUE` 表示自动清除通知。

## 8.2 发送通知

```c
xTaskNotifyGive(taskHandle);
```

## 8.3 中断中发送

```c
vTaskNotifyGiveFromISR(taskHandle, &xHigherPriorityTaskWoken);
portYIELD_FROM_ISR(xHigherPriorityTaskWoken);
```

## 8.4 使用建议

- 每个任务拥有一个通知值。
- 比信号量更节省资源，延迟更低。

---

# 第九章：FreeRTOS 软件定时器

软件定时器适用于不需精确实时性的周期性任务或延时执行任务。

## 9.1 创建定时器

```c
TimerHandle_t xTimer;
xTimer = xTimerCreate("T1", pdMS_TO_TICKS(1000), pdTRUE, 0, vTimerCallback);
```

- `pdTRUE` 表示周期定时器，`pdFALSE` 为一次性定时器。

## 9.2 启动定时器

```c
xTimerStart(xTimer, 0);
```

## 9.3 回调函数

```c
void vTimerCallback(TimerHandle_t xTimer)
{
    // 定时器触发时执行
}
```

---

# 第十章：FreeRTOS 内存管理

FreeRTOS 提供五种内存管理算法，分别由 `heap_1.c` 到 `heap_5.c` 实现。

## 10.1 选择内存模型

在 `FreeRTOSConfig.h` 中定义：

```c
#define configTOTAL_HEAP_SIZE ((size_t)10240)
```

## 10.2 常用模型说明

- `heap_1.c`：简单静态分配，不能释放。
- `heap_2.c`：允许释放，支持碎片整理。
- `heap_4.c`：最佳选择，支持合并、释放、碎片管理。

## 10.3 内存使用查询

```c
xPortGetFreeHeapSize();
xPortGetMinimumEverFreeHeapSize();
```

---


# 第十一章：FreeRTOS 中断管理

在嵌入式系统中，中断是重要的事件处理机制。FreeRTOS 支持与中断服务程序（ISR）的安全交互，允许中断控制任务的执行或发送数据。

## 11.1 ISR 中的限制

在 ISR 中不能使用可能引起阻塞的 API，例如：

- `vTaskDelay()`
- `xQueueReceive()`（需用 FromISR 版本）

必须使用 `_FromISR` 后缀的 API 来与任务交互。

## 11.2 队列相关的中断交互

中断中发送队列：

```c
BaseType_t xHigherPriorityTaskWoken = pdFALSE;
xQueueSendFromISR(xQueue, &data, &xHigherPriorityTaskWoken);
portYIELD_FROM_ISR(xHigherPriorityTaskWoken);
```

## 11.3 信号量与中断

中断触发信号量释放：

```c
xSemaphoreGiveFromISR(xSemaphore, &xHigherPriorityTaskWoken);
```

## 11.4 任务通知与中断

任务通知是中断唤醒任务的高效方式：

```c
vTaskNotifyGiveFromISR(taskHandle, &xHigherPriorityTaskWoken);
portYIELD_FROM_ISR(xHigherPriorityTaskWoken);
```

## 11.5 优先级切换机制

使用 `portYIELD_FROM_ISR()` 可在中断中显式请求上下文切换。

## 11.6 注意事项

- 中断优先级需配置在 FreeRTOS 可管理范围内。
- 在 STM32 中，需使用 `NVIC_SetPriority()` 设置合适优先级，小于等于 `configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY`。

示例：

```c
HAL_NVIC_SetPriority(EXTI0_IRQn, 5, 0);  // 优先级必须符合 FreeRTOS 要求
```

---
