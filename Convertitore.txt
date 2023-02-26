; multi-segment executable file template.

data segment
    ; add your data here!
    pkey db "Inserisci il numero:  $"
    pkey2 db 10,13, "Ecco il numero convertito:  $"
    num dw ?
ends

stack segment
    dw   128  dup(0)
ends

code segment
start:

    mov ax, data
    mov ds, ax
    mov es, ax

   
            
    lea dx, pkey
    mov ah, 9
    int 21h          
    
    
    
    
input:   ;ciclo per memorizzare un numero in una variabile (max 65535) in una variabile
      
    mov ah, 1
    int 21h
    cmp al, 13 
    je salva 
    mov ah, 0
    sub al, '0'
    mov cx, ax
    
cicloinput:
       
    mov ah, 1
    int 21h
    cmp al, 13 
    je salva 
    mov ah, 0
    sub al, '0'
    mov bx, 10
    xchg cx, ax
    mul bx
    add cx, ax
    jmp cicloinput
    
    
salva:
    mov num, cx ;sava il numero nella variabile
            
         
preconvertitore:           
    mov bx, 32  ;inizia la memorizzazione dalla posizione 32
    mov ax, num  
    
    
convertitore: ;salva in memoria il numero binario
    mov cx, 2 
    xor dx, dx
    div cx 
    add dx, '0'
    mov [bx], dx
    inc bx   
    cmp ax, 0
    ja convertitore
    jmp prestampa
    
    
prestampa:
    lea dx, pkey2
    mov ah, 9
    int 21h    
    
stampa: ;stampa il numero binario
    mov dl, [bx]
    mov ah, 2
    int 21h
    dec bx
    cmp bx, 32
    jae stampa
    
    
       
         
    
    mov ax, 4c00h ; exit to operating system.
    int 21h    
ends

end start ; set entry point and stop the assembler.
