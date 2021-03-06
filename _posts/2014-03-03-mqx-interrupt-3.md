---
layout: post
title: "MQX机制分析——中断机制(三)"
description: ""
categories: 
- C
tags: [MQX,操作系统,飞思卡尔]
---

　　这一节聊一聊宏观上中断发生时所做的一些操作流程，如寄存器的入栈出栈情况，堆栈的切换以及PC是如何改变的等等。首先看下任务被中断打断的情况下是如何响应的：
  
　　（1）有中断请求时，中断控制器NVIC获取中断源硬件设备的中断向量号，并通过识别的中断向量号将对应硬件中断源模块的中断状态寄存器中的“中断活动位”置位。  
　　（2）响应中断的时候，首先要做的就是保存中断现场：依次将PC、xPSR、R0~R3、R12、LR这个8个寄存器压入任务堆栈（任务栈的栈顶指针使用PSP），这一步是由硬件压入的，注意这里的顺序。  
　　（3）堆栈指针切换为使用MSP进行堆栈操作，系统计算EXC\_RETURN值①（EXC\_RETURN值只是用于区分返回后换入Handler模式还是线程模式②以及栈操作使用MSP还是PSP，与具体任务无关）并将它赋值给LR，此时的LR值已经被修改为EXC\_RETURN供异常在返回时判断使用，更新中断号程序状态寄存器IPSR中的低8位为新的中断号，更换程序计数器PC值为中断服务例程的入口地址。  
　　（4）执行中断服务例程，当执行PUSH {LR}语句时将EXC\_RETURN值压入堆栈，中断服务例程结束时，更新NVIC寄存器，将中断状态寄存器中对应的“中断活动位”清除，接着执行POP {PC}就是将EXC\_RETURN弹出到PC，此时会将MSP切换为PSP，CM4中使用把EXC\_RETURN写入PC来触发中断返回动作序列，中断返回动作序列就是将之前压入的8个寄存器弹出。把EXC\_RETURN写入PC有还有两种形式，一种是当LR寄存器中存放了EXC\_RETURN时，使用BX LR即可返回，另外一种就是将PC作为目的寄存器，使用LDR或者LDM指令也可以启动中断返回序列。此时会将之前压入堆栈的PC、xPSR、R0~R3、R12、LR这8个寄存器已经全部恢复。  

　　当在退出中断服务例程的时候有另外一个中断被挂起时（存在中断嵌套的情况下），更新完更新NVIC寄存器，将中断状态寄存器中对应的“中断请求位”清除之后，处理器不会从堆栈中恢复所有已保存的寄存器，相反，处理器将放弃堆栈弹出操作，而是根据挂起中断的中断号取出中断向量，将挂起中断的“中断活动位”置位，更新中断号程序状态寄存器IPSR中的低8位为新的中断号，更换程序计数器PC值为挂起中断的中断服务例程的入口地址，然后转而执行这个挂起的中断服务例程，并且LR寄存器会保存之前的EXC\_RETURN值，从而减少从一个异常处理程序切换到另一个异常处理程序时出栈再入栈的延迟，这也就是Cortex-M 处理器所用的一种名为咬尾中断（也叫尾链）的优化技术，使得使得中断处理更高效，系统响应速度更快。


----------
  
　　①在进入中断服务例程后，将自动更新lr的值为EXC\_RETURN，这是一个高28位全为1，只有[3:0]位有特殊含义的值。其具体含义参见下表。该值由内核自动设置，所以一般不需要改动。  
![1](http://github-blog.qiniudn.com/2014-02-28-mqx-interrupt-3-1.png-BlogPic)
　　合法的EXC\_RETURN值共3个，EXC\_RETURN=0xFFFF\_FFF1，返回Handler模式；EXC\_RETURN=0xFFFF\_FFF9，返回线程模式，并使用主堆栈（SP=MSP）；EXC\_RETURN=0xFFFF\_FFFD，返回线程模式，并使用进程堆栈（SP=PSP）。由EXC\_RETURN的格式可以看出， 0xFFFF\_FFF0~0xFFFF\_FFFF中的地址作为任何返回地址，所以CM4把这个地址范围设置为不可取指区。  
　　②Cortex-M4处理器支持Handle模式和线程模式这两种操作模式以及特权级和用户级这两级特权等级，用户级有时也被称为“非特权级”。如下表所示。
![2](http://github-blog.qiniudn.com/2014-02-28-mqx-interrupt-3-2.png-BlogPic)
　　**Handle模式和线程模式的区别是Handle模式使用的MSP而线程模式下使用PSP**，这样做的目的是为了避免系统堆栈因为应用程序错误而毁坏。**特权级和用户级的区别是特权级下程序可以访问所有范围的寄存器而在用户模式下与系统控制相关的寄存器时被禁止访问的**，如程序状态寄存器（xPSR）、中断使能/除能寄存器（PRIMASK等）和控制寄存器（CONTROL）等等，合理的操作模式转换图如下：  
![3](http://github-blog.qiniudn.com/2014-02-28-mqx-interrupt-3-3.png-BlogPic)  

----------

　　在MQX中，其实只用到了特权级线程模式和特权级handle模式，程序并没有切换到用户级线程模式下。在调试过程中我们可以看到，在MQX启动过程的\_\_boot函数中，CONTROL寄存器的低两位都为0，意味着程序运行在特权级Handle模式，并且使用MSP（复位后缺省值），接着在\_\_boot函数中将CONTROL寄存器的第1位写了1，就将MSP切换为PSP了，之后程序便一直运行在了特权级线程模式下，只有中断发生时才会进入到特权级handle模式。  
　　CONTROL[1]位只有在特权级的线程模式下才可以改写（如启动过程中），CONTROL[0]位只有在特权级下才可以被改写，一旦进入了用户级，那么如果想要再返回特权级，只能先触发一个软中断（如SVC指令触发系统调用），再由中断服务例程修改该位。
