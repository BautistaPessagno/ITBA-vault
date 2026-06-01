---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---
# System Calls

[Linux Syscall Reference](https://syscalls.gael.in/)

> [!note]+ # Compilado y Linkeditacion
> ```shell
>  nasm -f elf32 <archivo.asm> -o <archivo.o>
>  ld -m elf_i386 <archivo.o> -o <archivo>
> 
> ```
> 
> Compilar con C
> 
> ```bash
> nasm -f elf32 main.asm
> gcc -c -m32 hello.c
> gcc -m32 main.o hello.o -o hello
> ./hello
> ```
> 
> > [!note]+ ## Compilar en 64 bits
> > ```bash
> > nasm -f elf64 main.asm
> > gcc -c -m64 hello.c
> > 
> > ld -m elf_i386 <archivo.o> -o <archivo>
> > #eso o...
> > gcc -m64 main.o hello.o -o hello
> > ./hello
> > ```

> [!note]+ # Print
> ```assembly
> section .text
> 
> GLOBAL _start
> 
> 
> _start:
>         ;Codigo previo ...
>         
>         mov ecx, cadena         ;puntero a la cadena
>         mov edx, length         ;longitud cadena
>         
>         call print
>         
>         ;mas codigo ...
> 
> print:
>         mov eax, 4              ; FileDescriptor (STDOUT)
>         mov ebx, 1              ; ID del Syscall WRITE
> 			  int 80h                 ;Ejecucion de la llamada
>         ret
> 
> end:
>         mov eax, 0              ; ID del Syscall EXIT
>         mov ebx, 0              ; Valor de Retorno
>         int 80h                 ; Ejecucion de la llamada
> 
> 
> 
> section .data
> 				;poner tu cadena de texto y el 10 hace de \n 
>         cadena db "<tu cadena de texto>",10 
>         length equ $-cadena ; largo de la cadena
> ```

> [!note]+ # toString
> ```assembly
> section .text
> ;===============================================================================
> ; toString - convierte un numero en String.
> ;===============================================================================
> ; Argumentos:
> ;	ebx: 
> ; Retorno:
> ;	eax: 
> ;===============================================================================	
> 
> toString:
>     mov esi, ebx
> .stringLoop:
>     mov ebx, 10
>     xor edx, edx
>     div ebx
>     add edx, "0"
>     push edx
>     inc ecx
>     cmp eax, 0
>     jne .stringLoop
> 
>     mov eax, ecx
>     mov ecx, 0 
> .popNumbers:
>     pop edx
>     mov [esi + ecx], edx
>     dec eax
>     inc ecx
>     cmp eax, 0
>     jne .popNumbers
>     
>     ret
> ```

> [!note]+ # Exit
> ```assembly
> 
> ;===============================================================================
> ; exit - termina el programa
> ;===============================================================================
> ; Argumentos:
> ;	ebx: valor de retorno al sistema operativo
> ;===============================================================================
> exit:
> 	mov eax, 1		; ID del Syscall EXIT
> 	int 80h		; Ejecucion de la llamada
> ```

> [!note]+ # strlen
> ```assembly
> ;===============================================================================
> ; strlen - calcula la longitud de una cadena terminada con 0
> ;===============================================================================
> ; Argumentos:
> ;	ebx: puntero a la cadena
> ; Retorno:
> ;	eax: largo de la cadena
> ;===============================================================================
> strlen:
> 	push ecx	; preservo ecx	
> 	push ebx	; preservo ebx
> 	pushf		; preservo los flags
> 
> 	mov ecx, 0	; inicializo el contador en 0
> .loop:			; etiqueta local a strlen
> 	mov al, [ebx] 	; traigo al registo AL el valor apuntado por ebx
> 	cmp al, 0	; lo comparo con 0 o NULL
> 	jz .fin 	; Si es cero, termino.
> 	inc ecx	; Incremento el contador
> 	inc ebx
> 	jmp .loop
> .fin:			; etiqueta local a strlen
> 	mov eax, ecx	
> 	
> 	popf
> 	pop ebx	; restauro ebx	
> 	pop ecx	; restauro ecx
> 	ret
> ```

> [!note]+ # strcmp
> ```assembly
> ;===============================================================================
> ; strcmp - compara dos strings para ver si son iguales
> ;===============================================================================
> ; Argumentos:
> ;	eax: direccion de la cadena 1.
> ;	ebx: direccion de la cadena 2.
> ;	ecx: largo de la cadena 2. --> se podria mejorar para que no se necesite esto
> ; Retorno:
> ;	eax: 0 si son distintos, 1 si son iguales
> ;===============================================================================	
> strcmp:                 
>     push ebp            ;Armado de StackFrame
>     mov ebp, esp
> 
>     mov esi, eax        ;Puntero a la primera cadena en esi
>     mov edi, ebx        ;Puntero a la segunda cadena en edi
> 
> .loop:
>     mov al, [esi]       ;Guardo el caracter actual de la primer cadena
>     mov bl, [edi]       ;Guardo el caracter actual de la segunda cadena
>     cmp al, bl          ;Comparo ambos caracteres
>     jne .notEqual       ;Si no son iguales, directamente voy a terminar el programa y devolver 0 en eax
>     inc esi             ;Si son iguales, avanzo en esi
>     inc edi             ;Avanzo tambien en edi
>     dec ecx             ;Decremento la cantidad de caracteres por comparar
>     cmp ecx, 0          ;Si no me quedan caracteres por comparar, es porque es igual
>     jne .loop           ;Si me quedan, repito el ciclo
>     mov eax, 1          ;Si ya no quedan, es porque es igual. Asigno 1 a eax y retorno
>     jmp .endFunc
> 
> .notEqual:
>     mov eax, 0
> .endFunc:
>     pop ebp             ;Desarmado de StackFrame
>     ret
> ```

> [!note]+ # toUpper
> ```assembly
> ;===============================================================================
> ; toUpper - convierte caracteres en minuscula a mayuscula
> ;===============================================================================
> ; Argumentos:
> ;	ebx: direccion de la cadena terminada en cero
> ; Retorno:
> ;
> ;===============================================================================
> toUpper:
>     mov ecx, 0
> .upLoop:
>     mov AL, [ebx + ecx]
>     cmp AL, 10
>     jne .checkMin
>     ret
> .checkMin:
>     cmp AL, "a"
>     jae .checkMax
>     inc ecx
>     jmp .upLoop
> .checkMax:
>     cmp AL, "z"
>     jbe .modify
>     inc ecx
>     jmp .upLoop
> .modify:
>     sub byte[ebx + ecx], 32
>     inc ecx
>     jmp .upLoop
> ```

> [!note]+ # Armado Pila
> ```assembly
> 	;armado stack
> 	push ebp
> 	mov ebp, esp
> 	
> 	;desarmado stack
> 	mov esp, ebp
> 	pop ebp
> ```

> [!note]+ # EJ
> ```assembly
> section .text
> 
> GLOBAL _start
> extern funcion ;aqui asi se importan funciones de afuera
> ;despues en al linkeditar hay que inlucir el archivo.o del programa
> 
> _start:
> 	mov ecx, cadena 	; Puntero a la cadena
> 	mov edx, longitud	; Largo de la cadena 
> 	call print
> 
> 	mov ecx, cadena_2
> 	mov edx, cadena_2_len
> 	call print
> 
> 	mov eax, 1		; ID del Syscall EXIT
> 	mov ebx, 0		; Valor de Retorno
> 	int 80h		; Ejecucion de la llamada
> 
> 
> ;===============================================================================
> ;Funcion print
> ;Recibe en ECX, la cadena a imprimir y en EDX el largo de la misma.
> ;===============================================================================
> 
> print:
> 	mov ebx, 1		; FileDescriptor (STDOUT)
> 	mov eax, 4		; ID del Syscall WRITE
> 	int 80h		; Ejecucion de la llamada
> 	ret			; retorno de la funcion
> 
> end:
> 	; Finaliza el programa correctamente
> 	mov eax, 1         ; syscall number (sys_exit)
> 	xor ebx, ebx       ; código de salida 0
> 	int 0x80
> 
> 
> section .data
> 
> cadena db "Hola Mundo!!", 10	; "Hola Mundo!!\n"
> longitud equ $-cadena
> cadena_2 db "Arquitectura de Computadoras", 10
> cadena_2_len equ $-cadena_2
> 
> 
> section .bss
> 
> placeholder resb 128
> 
> ```
