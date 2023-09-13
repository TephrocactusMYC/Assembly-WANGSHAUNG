# 磁盘读写
```
assume cs:code
code segment
start:
        mov ax,0b800h
        mov es,ax
        mov bx,0

        mov al,0
        mov ch,0
        mov cl,1
        mov dl,0
        mov dh,0
        mov ah,3
        int 13h

        mov ax,4c00h
        int 21h
code ends
end start
```
抄了个网友的，没啥心思写了。
```
assume cs:code

code segment
start:
	mov ax,cs
	mov ds,ax
	mov si,offset int7ch
	mov ax,0
	mov es,ax
	mov di,200h
	mov cx,offset int7ch_end - offset int7ch
	cld
	rep movsb

	cli
	mov word ptr es:[7ch*4],200h
	mov word ptr es:[7ch*4+2],0
	sti

	mov ah,0
	mov bx,1
	int 7ch

	mov ax,4c00h
	int 21h


	;bx为入口参数，16位除法，dx高位，ax低位，ax商，dx余数
int7ch:
	cmp ah,1
	ja over

	push cx
	push dx
	push bx
	push ax

	mov ax,bx
	mov dx,0
	mov bx,1440
	div bx
	mov cl,al		;盘面号，先存在cl中

	mov ax,dx
	mov dh,cl		;将暂存在cl中的盘面号存在dh中
	mov dl,0		;驱动器号，软驱 0

	mov bl,18
	div bl
	mov ch,al		;磁道号

	inc ah
	mov cl,ah		;扇区号

	mov ax,0b800h
	mov es,ax
	mov bx,160*5	;es:bx  指向读写区

	pop ax
	push ax
	mov al,8		;读写扇区数
	cmp ah,0
	je read
	cmp ah,1
	je write

read:
	mov ah,2
	jmp short ok
write:
	mov ah,3
	jmp short ok
ok:
	int 13h
	pop ax
	pop bx
	pop dx
	pop cx
over:
	iret

int7ch_end:
	nop

code ends
end start

```