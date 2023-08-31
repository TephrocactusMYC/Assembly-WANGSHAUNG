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