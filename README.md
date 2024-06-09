# traffic_lights
demo
#start=Traffic_Lights.exe#

name "traffic"

data segment
    mes db 13, 10, 'Press 0 to turn off all lights, 1 for all red, 2 or 3 for maunal mode, 4 for auto mode, 5 to stop the program $'
    mes1 db 13, 10, 'When in auto mode, press E to exit the auto mode $'
         
;                FEDC_BA98_7654_3210
situation dw 0000_0011_0000_1100b
s1        dw 0000_0010_1000_1010b
s2        dw 0000_1000_0110_0001b
s3        dw 0000_1000_0110_0001b
s4        dw 0000_0100_0101_0001b    

sit_end = $
;            FEDC_BA98_7654_3210
all_red  equ 0000_0010_0100_1001b
all_off  equ 0000_0000_0000_0000b
s5        dw 0000_1000_0110_0001b
s6        dw 0000_0011_0000_1100b
s7        dw 0000_0100_1001_0010b
data ends

code segment
    assume cs:code, ds:data

start:
    mov ax, @data
    mov ds, ax
    
    lea dx, mes
    mov ah, 9
    int 21h       
    
    lea dx, mes1
    mov ah, 9
    int 21h 

    mov ax, all_red
    out 4, ax

input:
    mov ah, 01h    ; Read character from keyboard
    int 21h        ; AL contains the ASCII code of the pressed key
    ;Manual
    ;turn off all light
    cmp al, '0'
    je turn_off_lights
    
    ;turn on all red light
    cmp al, '1'
    je turn_on_red_lights 
    
 
    cmp al, '2'
    je turn_on_x_axis_lights

    cmp al, '3'
    je turn_on_y_axis_lights

    cmp al,'4'
    je automatic_mode
    
    cmp al, '5'
    je endd
