# 转移指令
ret是用栈中的数据修改IP，从而实现近转移（弹出IP）

retf是用栈中的数据修改CS和IP，从而实现远转移（弹出IP，然后弹出CS）

# CALL
call将当前的IP或者CS、IP压入栈中

转移

CALL不能用于短转移

# CALL RET
组合起来就是高级语言的函数

# 监测点10.1
```
assume cs:code
stack segment
     db 16 dup (0)
stack ends

code segment

start:  mov ax,stack
        mov ss,ax
        mov sp,16
        mov ax, 1000h
        push ax
        mov ax,   0
        push ax
        retf
code ends

```
# 监测点10.2
ax=6
# 监测点10.3
CS=1000,IP=9

AX=1018
# 监测点10.4
下面的程序执行后，ax中的数值为多少？

内存地址   机器码        汇编指令       执行后情况

1000:0     b8 06 00      mov ax,6       ax=6,ip指向1000:3

1000:3     ff d0         call ax        pop ip,ip指向1000:6

1000:5     40            inc ax

1000:6     58            mov bp,sp      bp=sp=fffeh

                         add ax,[bp]    ax=[6+ds:(fffeh)]=6+5=0bh



用debug进行跟踪确认，“call ax(16位reg)”是先将该指令后的第一个字节偏移地址ip入栈，再转到偏移地址为ax(16位reg)处执行指令。
# 监测点10.5
1. ax=0
2. ax=1,bx=0（s-nop=1，CS-CS=0）

这个问题需要知道机器码长度，很无语

如果单步会由于中断导致栈段被覆盖。需要使用d指令


