org 0x7C00
jmp start
;Настройка сегмента стека
start:
    cli                 
    mov ax, 0           
    mov ss, ax
    mov sp, 0xFFFF      
    sti                 
    mov ax, 0x13        
    int 0x10
    mov ax, 0x0010
    int 0x10
    ;Рисование треугольника
    mov cx, 290
    mov dx, 10
    mov bx, 95
    call draw_triangle
    ;Установка цвета фона
    mov ah, 0x0B
    mov bl, 0x05
    int 0x10
    ;Вывод тектса
    mov ah, 0x13
    mov al, 0
    mov bh, 0
    mov bl, 3
    mov dl, 34
    mov dh, 21
    push cs
    pop es
    mov bp, msg
    mov cx, 10
    int 0x10
    mov ah, 0x13        
    mov al, 0
    mov bh, 0
    mov bl, 3
    mov dl, 34          
    mov dh, 22         
    push cs
    pop es
    mov bp, msg1
    mov cx, 10
    int 0x10
    mov ah, 0x13       
    mov al, 0
    mov bh, 0
    mov bl, 3
    mov dl, 34        
    mov dh, 23         
    push cs
    pop es
    mov bp, msg2
    mov cx, 10
    int 0x10
    mov ah, 0x13        
    mov al, 0
    mov bh, 0
    mov bl, 3
    mov dl, 34         
    mov dh, 24        
    push cs
    pop es
    mov bp, msg3
    mov cx, 10
    int 0x10
hang:
    jmp hang
;Отрисовка треугольника
draw_triangle:
    pusha
    mov bp, cx
triangle_loop:
    push cx
    mov ax, bx
    shr ax, 1
    sub cx, ax
    add ax, bp
;Отрисовка линии
draw_line:
    cmp cx, ax            
    jae line_done         
    push ax
    push bx
    push dx
    mov ah, 0x0C
    mov al, 0x0A
    int 0x10
    pop dx
    pop bx
    pop ax
    inc cx
    jmp draw_line

line_done:
    pop cx
    add dx, 1            
    add cx, 2
    dec bx                
    jnz triangle_loop   

    popa                   
    ret
;Заполнение текста
msg db "Anisimova", 0
msg1 db "Anastasia", 0
msg2 db "Anatolevna", 0
msg3 db "NMT-313901", 0
;Заполнение оставшейся части
times 510-($-$$) db 0
dw 0xAA55
