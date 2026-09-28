---
temas: []
Cuatri: 1ro-2025
Materia: "[[ arqui.base |Aqrui]]"
categories:
  - "[[ITBA.base|ITBA]]"
---

# Sintaxis

existen varias sintaxis para ASM, pero las mas conocidas son:

- Intel
- AT & T

```assembly
mov EAX, 1 #(Sintaxis intel)

movl $1, %eax #(sintaxis AT&T)
```

obs: Tener en cuenta que el gcc por default genera salidas en sintaxis AT&T

![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_10.11.19.png)

## Instrucciones

[Set de instrucciones | VonSim](https://vonsim.github.io/es/computer/instructions/)

Flujo de bytes que interpretados por el procesador que realizan una acción

ej:Instrucción: **add eax, 0x1**

![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_10.13.06.png)

> [!note]+ ### Lectura de memoria
> ![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_10.15.30.png)

> [!note]+ ### Simulador VonSim
> [VonSim — A 8088-like Assembly Simulator](https://vonsim.github.io/)
> 
> ![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_10.16.24.png)

> [!note]+ ### Segmentación de memoria en 80386
> ![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_10.25.47.png)

> [!note]+ ### Modos de direccionamiento
> 
> Como en otros procesadores tendremos la sintaxis general
> será:
> 
> ```assembly
> Instrucción destino , fuente
> 
> ```
> 
> <u>**Direccionamiento inmediato:**</u>
> 
> ```assembly
> mov ax, 0ffffh
> ```
> 
> <u>**Direccionamiento de registro:**</u>
> 
> ```assembly
> mov edx,eax
> mov ah,al
> ```
> 
> <u>**Direccionamiento directo o absoluto:**</u>
> 
> ```assembly
> mov ax, [57D1h]
> mov ebx, es:[42c9h]
> ```
> 
> <u>**Direccionamiento indirecto:**</u>
> 
> ```assembly
> mov cx, [bp]
> mov es:[di],ax
> ```
> 
> <u>**Direccionamiento con índice o indexado:**</u>
> 
> ```assembly
> mov cx, [bp+4]
> mov es:[di+8],ax
> ```
> 
> <u>**Recordatorio:**</u>
> <u>NO</u> existe el movimiento de datos de memoria a memoria en
> una sola instrucción:
> 
> ![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_10.26.51.png)
> 
> > [!note]+ ### Explicación según Gemma 3
> > Okay, let's break down addressing modes in assembly language.  The images you provided show examples for x86 architecture (likely 16-bit or early 32-bit). I'll explain each mode with explanations and analogies to make it easier to understand.
> > 
> > **What are Addressing Modes?**
> > 
> > Addressing modes tell the processor *how* to find the data it needs to perform an instruction.  Think of it like this: you want a book from your library. The addressing mode is how you tell the librarian (the CPU) where to find that book. Is it by title, author, call number, or something else?
> > 
> > **1. Immediate Addressing (Direccionamiento Inmediato)**
> > 
> > - **What it is:** The data itself is included directly in the instruction.
> > - **Example:** `mov ax, 0fffh`
> >     - This means: "Move the *value* 0fffh into register AX."
> > - **Analogy:** You tell the librarian: "Give me the book that's *exactly* 'The Cat in the Hat'." You're giving them the complete information.
> > - **Characteristics:** Simple, fast (no memory access needed), but limited to constant values.
> > 
> > **2. Register Addressing (Direccionamiento de Registro)**
> > 
> > - **What it is:** The data resides in a CPU register.
> > - **Example:** `mov edx, eax`
> >     - This means: "Move the *contents* of register EAX into register EDX."
> > - **Analogy:** You tell the librarian: "Give me the book that's on shelf 'A'." (Shelf A represents a register). You don't specify the title, just where to look.
> > - **Characteristics:** Very fast because registers are directly accessible by the CPU.
> > 
> > **3. Direct Addressing (Direccionamiento Directo o Absoluto)**
> > 
> > - **What it is:** The instruction contains the *absolute memory address* of the data.
> > - **Example:** `mov ax, [57D1h]`
> >     - This means: "Move the data located at memory address 57D1h into register AX." (The brackets `[]` are important; they indicate a memory location.)
> > - **Analogy:** You tell the librarian: "Give me the book that's at shelf number 1234." (1234 is the absolute memory address).
> > - **Characteristics:** Relatively simple, but less flexible. The address must be known in advance and can’t change during program execution.
> > 
> > **4. Indirect Addressing (Direccionamiento Indirecto)**
> > 
> > - **What it is:** The instruction contains a register that *holds* the memory address of the data.
> > - **Example:** `mov cx, [bp]`
> >     - This means: "Move the data located at the memory address contained in register BP into register CX." BP (Base Pointer) holds an address; we're using that address to find the actual data.
> > - **Analogy:** You tell the librarian: "Give me the book whose location is written on a card in shelf 'B'." Shelf B represents the register BP, and it contains the address of the book you want.
> > - **Characteristics:** More flexible than direct addressing because the address can be changed by modifying the register's value.
> > 
> > **5. Indexed Addressing (Direccionamiento con Índice o Indexado)**
> > 
> > - **What it is:** This combines a base register with an index to calculate the memory address. The index is often used to access elements in arrays or data structures.
> > - **Example:** `mov cx, [bp+4]`
> >     - This means: "Move the data located at the memory address calculated by adding 4 to the value in register BP into register CX." BP holds a base address, and we're offsetting it by 4 bytes.
> > - **Analogy:** You tell the librarian: "Give me the book that is four spaces after the book on shelf 'B'." Shelf B represents the base pointer (BP), and you are adding an offset of 4 to find the desired book.
> > - **Characteristics:** Very flexible, commonly used for array access. The index can be a constant or another register.
> > 
> > **Key Points & Considerations**
> > 
> > - **Brackets **`**[]**`**:** These are crucial in assembly language. They indicate that you're referring to the *contents of memory* at a particular address, not the address itself.
> > - **Segment Registers (e.g., ES):** In segmented architectures like x86, addresses are often divided into segments and offsets. The segment register (like `ES`) specifies which segment the data is in. The offset is the distance from the beginning of that segment to the actual data location.
> > - **Data Size:** Addressing modes work with different data sizes (bytes, words, doublewords). The size affects how many bytes are accessed at a time.
> > 
> > **Why is this important for you as a software engineer student?**
> > 
> > - **Understanding Low-Level Code:** Even if you primarily write high-level code (like Python or Java), understanding assembly language helps you grasp what's happening "under the hood."
> > - **Performance Optimization:** Sometimes, knowing how to use addressing modes effectively can lead to performance improvements in critical sections of your code.
> > - **Debugging and Reverse Engineering:** When debugging low-level issues or analyzing existing binary code (reverse engineering), assembly language knowledge is essential.
> > - **Compiler Design:** If you ever want to delve into compiler design, understanding how high-level constructs are translated into assembly instructions is vital.
> > 
> > Let me know if you'd like more details on any of these addressing modes, or if you have other questions!

## Registro de Flags

![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_12.18.13.png)

> [!note]+ ### Ejs
> ![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_12.18.31.png)
> 
> - en section .text se indica donde esta el codigo
> - en `parametros db 11h,12h,13h` lo que muestra es que db va a ser un vector y se van a guardar uno al lado del otro
> - en Ciclo se marca una etiqueta indicando que hay un ciclo
> ![](Attachments/Captura_de_pantalla_2025-03-18_a_la%28s%29_12.45.19.png)

# Escribir código assembler

comilar y linkeditar

```bash
 nasm -f elf32 <archivo.asm> -o <archivo.o>
 ld -m elf_i386 <archivo.o> -o <archivo>
```

ej: hello world

```assembly
section .text

GLOBAL _start
extern funcion ;aqui asi se importan funciones de afuera
;despues en al linkeditar hay que inlucir el archivo.o del programa

_start:
	mov ecx, cadena 	; Puntero a la cadena
	mov edx, longitud	; Largo de la cadena 
	call print

	mov ecx, cadena_2
	mov edx, cadena_2_len
	call print

	mov eax, 1		; ID del Syscall EXIT
	mov ebx, 0		; Valor de Retorno
	int 80h		; Ejecucion de la llamada


;===============================================================================
;Funcion print
;Recibe en ECX, la cadena a imprimir y en EDX el largo de la misma.
;===============================================================================

print:
	mov ebx, 1		; FileDescriptor (STDOUT)
	mov eax, 4		; ID del Syscall WRITE
	int 80h		; Ejecucion de la llamada
	ret			; retorno de la funcion

end:
	; Finaliza el programa correctamente
	mov eax, 1         ; syscall number (sys_exit)
	xor ebx, ebx       ; código de salida 0
	int 0x80


section .data

cadena db "Hola Mundo!!", 10	; "Hola Mundo!!\n"
longitud equ $-cadena
cadena_2 db "Arquitectura de Computadoras", 10
cadena_2_len equ $-cadena_2


section .bss

placeholder resb 128

```

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_10.15.10.png)

![](Attachments/Captura_de_pantalla_2025-03-25_a_la%28s%29_10.15.50.png)

```bash
#compilar
nasm -f elf64 archivo.asm -o archivo.o

#linkeditar con gcc
gcc archivo.o -o arhcivo
```

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Arqui)**

- [Codigos Assembler](Codigos%20Assembler.md) — ejemplos de código
- [Clase 2 ASM intel](Clase%202%20ASM%20intel.md) — clase de ASM Intel
- [ASM y C](ASM%20y%20C.md) — interoperación ASM/C
- [Modo protegido](Modo%20protegido.md) — modos de ejecución del procesador
- [Seguimiento de Pila en C](Seguimiento%20de%20Pila%20en%20C.md) — stack frames en la práctica

**Otras materias**

- **EDA**  [EDA - Stack](EDA%20-%20Stack.md) — push/pop: la pila a nivel máquina

<!-- notas-relacionadas:fin -->
