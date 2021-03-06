---
layout: post
title: "异常(同步中断)和中断（异步中断）"
description: ""
categories: 
- 嵌入式
tags: []
---

　　中断和异常到底有什么区别？看到的一些书上经常将这两个词混着使用，最近在查阅《ARM Cortex-M3 权威指南》时看到一段小字给出了答案，果断摘录下来：*所有能打断正常执行流的事件都称为异常。如果非得分个丁一卯二，则中断与异常的区别在于，那240个中断对CM3核来说都是“意外突发事件”——也就是说，该请求信号来自CM3内核的外面，来自各种片上外设和外扩的外设，对CM3来说是“异步”的；而异常则是因CM3内核的活动产生的——在执行指令或访问存储器时产生，因此对CM3来说是“同步”的。*[1] 
![1](http://github-blog.qiniudn.com/2014-02-20-interrupt-1.png-BlogPic)  
　　以K60为例，如图所示，中断向量表中前16个（编号为0-15）是由于执行指令或者访问存储器产生的，比如说MCU/MPU想要访问非法位置时会产生MemManage fault或者取址发生失败时产生的总线fault，再举个例子：系统调用(SuperVisor Call)，在CM3/4中有个SVC指令（类似于ARM7中的SWI指令），当你在软件中写上SVC指令，那么在执行到这个语句的时候就会触发SVC异常，进入SVC的中断服务例程，这些中断就被称为**系统异常**。因为只有在一条指令执行完毕后 MCU才会发出中断，而不是发生在代码指令执行期间。也被称为**同步中断**[2]

　　而后面的那些（编号16以后）由片上外设产生的，像串口接收中断等等的这些被叫做**外部中断**，因为其能够在指令之间发生，也被称为**异步中断**。不过这也就是个说法的问题，就像大部分书里面都会强调不区分两种说法，所以了解就好。  

　　还有个问题就是软中断和硬中断的区别，从名称上理解的话，软中断就是由软件触发的中断，而硬中断是由硬件触发的中断（解释了等于没解释=。=！），在我看来软中断是和异常(同步中断)等价的，硬中断是和中断（异步中断）类似的，不知道这样理解有没有错。还有一个区别在于软中断无需中断控制器的参与，硬中断通常是由外设硬件发出的，需要有中断控制器参与。


　　参考文献：  
　　[1] [ARM Cortex-M3 权威指南](http://book.douban.com/subject/3883699/)  
　　[2] http://www.ibm.com/developerworks/cn/linux/l-cn-linuxkernelint/

