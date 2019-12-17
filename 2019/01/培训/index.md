# ASC上课笔记

# 19/1/14

CPU（Central Processing Unit）架构：  **冯诺依曼**计算机结构
<!-- more -->
现代CPU 



a = b + c  编译器：优化  指令时间

### 什么是CPU

完成基本的逻辑指令          基本的操作单元  

MAC 乘加指令   a = b*c+d  一个指令周期执行一次乘累加



ALU(算数逻辑单元)   L1(一级缓存)     L2    L3



![s](https://i.loli.net/2019/01/14/5c3beedc4f991.png)

### 桌面程序

真正用于数值运算的指令很少



### CPU架构

![x](https://i.loli.net/2019/01/14/5c3bf11987047.jpg)



###　简单的CPU

![](https://i.loli.net/2019/01/14/5c3bf1c1452f9.png)

​                                                          取指 → 译码 → 执行 → 访存 → 写回

* **流水线　Pipeline**

  内存是很慢的 400周期

* **分支预测 Branch Prediction**

* **分支断定 Predication**

  用条件语句替换分支

* **提升 IPC(instructions/cycle)**

  Add r0,r1 -> r2

  Add_v (r0,r1...r8) -> r 

  超标量（增加流水线宽度）

  N路超标量

  问题：N倍资源、旁路网络N^2^

  

   Sandy Bridge 超标量

**指令调度**
xor 异或
xor r1,r2 -> r3
add r3,r4 -> r4
sub r5,r2 -> r3
addi r3,1 -> r1

**寄存器重命名**  替换寄存器
xor p1,p2 -> p6
add p6,p4 -> p7
sub p5,p2 -> p8
addi p8,1 -> p9
xor and sub 可以并行执行



**乱序执行 Out-of-Order(OoO) Execution**

重排指令，获得最大的吞吐率

**OoO Sandy Bridge**

μop 微指令

**乱序执行**

+IPC 接近理想状态
– 面积增加
– 功耗增加

**存储器架构/层次Memory Hierarchy**

存储器越大越慢

SRAM、DRAM(断电消失)、Flash、HDD

**缓存Caching**

缓存层次Cache Hierarchy

硬件管理：L1、L2、L3

软件管理：Main memory、Disk

缓存一致性

**存储器的另外设计考虑**

* 分区Banking

* 一致性Coherency

* 控制器Memory Controller

**CPU内部的并行性**



**线程级并行Thread-Level Parallelism**

栈 先进先出

队列 

**锁、一致性和同一性**

问题: 多线程读写同一块数据
解决方法: 加锁



**结论**

CPU 为串行程序优化

缓慢的内存带宽（存储器带宽）将会是大问题

并行处理是方向

