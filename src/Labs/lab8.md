# 分析
```
assume cs:codesg
codesg segment
            mov ax,4c00h
            int 21h

    start:  mov ax,0
        s:  nop
            nop

            mov di,offset s
            mov si,offset s2
            mov ax,cs:[si]
            mov cs:[di],ax

        s0: jmp short s

        s1: mov ax,0
            int 21h
            mov ax,0
        s2: jmp short s1
            nop

codesg ends
end start
```

这个程序从start开始

先执行s

di之中是s的地址
si之中的s2的地址

cs:[di]之中存cs:[si]，也就是cs:s2的内容。

程序现在变成了这样。
```
assume cs:codesg
codesg segment
            mov ax,4c00h
            int 21h

    start:  mov ax,0
        s:  jmp short s1

            mov di,offset s
            mov si,offset s2
            mov ax,cs:[si]
            mov cs:[di],ax

        s0: jmp short s

        s1: mov ax,0
            int 21h
            mov ax,0
        s2: jmp short s1
            nop

codesg ends
end start
```
然后执行s0，跳到s。这里需要注意，jmp short是通过计算IP来的，也就是说是把IP往下减8个单位。因此，当运行到S的jmp short s1时，实际上是把IP减少8个byte，这时正好跳回一开始的mov ax,4c00h，然后执行。因此可以顺利退出。