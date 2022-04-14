---
title: 运行时栈与函数
authors:
    - CH3CHOHCH3
date: 2022-04-11 19:00:00
---
# 运行时栈与函数

!!! info ""
    这是“程序的机器级表示”系列的第三篇文章。本篇介绍运行时栈在函数调用中如何发挥作用。

编程者一定听说过所谓运行时栈、栈内存、栈溢出等概念，下面我们尝试从汇编层面理解运行时栈对程序的运行意味着什么。如果能真正明白计算机仅仅能够在寄存器和内存之间搬运数据并计算数据这件事，我们就会发现运行时栈这一设计是如此的水到渠成。

# 函数

编程中，某些功能会被反复使用，将其提炼为代码块，并根据输入的参数返回相应的结果，即 **函数** 。在 C 语言中，所有工作都是在函数中完成的，我们在函数 P 中调用函数 Q，提供一些参数，得到其返回值，又继续执行 P 的代码。在汇编层面，调用一个函数需要考虑这些要素：

1. 从 P 调用 Q 时，需要将 PC 的值设为 Q 的代码起始地址；从 Q 返回 P 时，又需要将 PC 改为 P 调用 Q 后面的那条指令地址；
2. P 可能需要向 Q 传递参数，可以在跳转至 Q 前预先修改某些寄存器或内存的值，跳转后供 Q 直接使用；Q 可能向 P 返回结果，也可以通过返回前修改寄存器或内存的值实现。
3. Q 可能要为某些局部变量分配一些内存，而在 Q 返回后，这些内存不再被使用，所以需要在返回前释放。

如果直接实现这些机制，是否可行呢？对于 1，是容易实现的，只需使用上篇讲到的`jal`和`jalr`指令即可。

来看 2，调用时传递一个变量可以存在寄存器中，传递一个数组或结构体可以把内存首地址存在寄存器里，返回也可以如法炮制。但有几个问题：

+ 如果 Q 需要的参数很多，寄存器就会不够使用，我们 **需要在内存中有一块空间** 来存放参数，并把这块空间的地址放在某个寄存器中以供访问。
+ Q 的执行过程中，极有可能会修改其他寄存器，而这些寄存器中可能存储着函数 P 需要的数据，返回 P 后便会产生错误。限制寄存器使其不被修改是不可行的，这会导致函数递归调用时可用的寄存器越来越少，因此 **需要内存中有一块空间** 存放 Q 修改了哪些寄存器，并在 Q 返回前将它们修改回来。

再来看 3，很多时候我们只需要寄存器就能存储局部变量，但有时候也不得不利用内存：

+ 寄存器不足以存放所有局部变量。
+ 对局部变量使用了 `&` 运算，所以必须为它产生一个地址。
+ 变量是数组或结构体，需要通过其引用被访问到。

因此，我们确实 **需要一块内存空间** 存储这些变量，并在结束调用时将其释放。

综上，为了实现函数调用，我们不得不在内存中安排一块空间存储必要的信息。

# 运行时栈

上面提到的这块空间就是 **运行时栈** 。采用栈的设计，是因为函数调用的过程和栈很相似：在层层调用的函数中，每个函数的数据称为一个栈帧，后调用函数的栈帧放在栈顶，调用结束时弹出，此时栈顶的栈帧恰好属于上一层函数。我们用一个专用的寄存器存放栈顶在内存中的位置（RISC-V 中是 x2，x86 中有 %rsp），就可以实现对栈内存的访问了。

!!! info ""
    一个函数的栈帧大小由函数需要的参数、局部变量等的空间决定，这些因素在编译时，就会被编译器计算好。

如果把栈想象成一个井，那么 RISC-V 的栈是倒置的，即“井口”朝下

<div align="center">
<img src="https://user-images.githubusercontent.com/72899584/163212084-c79c8cce-0b31-4a6d-aff2-655972ddf3eb.png" width=50%>
<p>进程在内存中的模型</p>
</div>

这张图十分经典，在以后的学习中我们还会不断地分析它。

# 举例

没有例子的阐释是抽象的，下面我们举一个调用函数的具体例子：

```C
long long int fact(long long int n){
    if(n < 1) return 1;
    else return n*fact(n-1);
}
```

其 RISC-V 汇编的分析如下图：

<div align="center">
<img src="https://user-images.githubusercontent.com/72899584/163212137-5313aebe-c893-4940-b605-98e53782f8e0.png" width=50%>
<p> RISC-V 函数调用</p>
</div>

图片来自本校老师的 ppt。

# 后记——关于 C

本系列在讲述汇编的函数调用和内存分配（强调了 C 中的数组，结构体）时采用的均为 C 语言，大多数其他汇编教材也都如此。那么，C 语言为何如此重要，使得大部分汇编都以此为模型呢？

事实上，诸多常见的操作系统内核都是基于 C 语言的，C 语言本身就是为了编写 UNIX 操作系统而诞生。丹尼斯·里奇与肯·汤普逊最初希望以一种简洁的方式来代替汇编语言，编写控制计算机硬件的操作系统。因此，C 本身的思维也十分贴近汇编语言，二者有一种密不可分的联系。