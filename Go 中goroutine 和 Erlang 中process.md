# Go 中goroutine 和 Erlang 中process

### Erlang 的Actor模型

Actor模型的Actor对应Erlang的进程，进程之间互相隔离，互不影响，每个进程有自己的邮箱，通过信息传递方式进行直接异步通信

### Go的CSP模型

goroutine之间互不联系，通过一个或多个channel发布信息或者监听

消息发布者和接收者通过channel松耦合，彼此不可见

在CSP中，消息交换是同步的，也就是任务需要立即执行，而Go中增加一个缓存，允许任务暂停，待可执行时逐个按序执行

### Erlang 内存管理

实现方式：process-centric，进程间通信需要复制信息，在进程终止后，其分配的内存区域可以在不间断的时间内被回收

Erlang进程包含：

进程控制块(PCB)：进程相关信息，pid、status、register name...

栈(Heap)：本地变量、返回地址...

堆(Stack)：进程邮箱的物理信息，但是超过64位的Binary不会存储到进程私有堆，而是存在Shared Area for Binaries，进程对维护的是一个列表，该列表存储进程指向该区域的所有指针

### Go GMP关系

[![yb6WRI.md.png](https://s3.ax1x.com/2021/02/23/yb6WRI.md.png)](https://imgchr.com/i/yb6WRI)

