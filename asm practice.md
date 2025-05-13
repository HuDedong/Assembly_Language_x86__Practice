## Practice 1

Write assembly program as following template

The template is as follows
```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode: DWORD

.data
    ; declare variables here

.code
main PROC
    ; write your code here
    INVOKE ExitProcess,0
main ENDP

END main
```

1. **Write asm program to adds two 32-bit integers and observe the result register and memory in debug mode.**

2) **Define 3 hexadecimal variables and initialized, Get the sum of first two variables and subtract 3rd variable from sum.**
Click debug, select the window, select the register and memory
Select memory, input & sum to view the result

3. **Calculate (2^29 + 40) observe the result**
4. **From the textbook(P92)**
    1. Data Definitions**
        Write a program that contains a definition of each data type listed in Table 3-2 in Section 3.4. Initialize each variable to a value that is consistent with its data type.
    2. **Symbolic Integer Constants**
          Write a program that defines symbolic constants for all of the days of the week. Create an array variable that uses the symbols as initializers.
	3. **Symbolic Text Constants**
        Write a program that defines symbolic names for several string literals (characters between quotes). Use each symbolic name in a variable definition.

---

### Solution(s):

Sol 1:

```assembly
;AddTwo.asm -add two 32 bit integers
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode:DWORD

.data
var1 DWORD 1
var2 DWORD 2
sum DWORD ?
.code
main PROC
	mov eax,val1
	add eax,val2
	mov sum,eax
	INVOKE ExitProcess,0
main ENDP
END main
```

Sol 2:

```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode:DWORD

.data
val1 DWORD 1h
val2 DWORD 2h
val3 DWORD 3h
sum DWORD ?
.code
main PROC
	mov eax,val1
	add eax,val2
	sub eax,val3
	mov sum,eax
	INVOKE ExitProcess,0
main ENDP
END main
```

Sol 3:

```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode:DWORD

.data
result DWORD ?
.code
main PROC
	mov eax,1
	shl eax,29
	add eax,40
	mov result,eax
	INVOKE ExitProcess,0
main ENDP
END main
```

Sol 4.1:

你需要根据这个表格来在程序中分别定义每一种数据类型并初始化一个合理的值。

Table 3-2 包含的数据类型与释义

| 名称   | 宽度   | 用法/含义与备注                              |
| ------ | ------ | -------------------------------------------- |
| BYTE   | 8-bit  | 无符号字节 (B=byte)                          |
| SBYTE  | 8-bit  | 有符号字节 (S=signed)                        |
| WORD   | 16-bit | 无符号字、可作实模式“近”指针                 |
| SWORD  | 16-bit | 有符号字                                     |
| DWORD  | 32-bit | 无符号双字，可为保护模式“近”指针             |
| SDWORD | 32-bit | 有符号双字                                   |
| FWORD  | 48-bit | 仅整数（实际上常用于“远”指针）               |
| QWORD  | 64-bit | 四字（整数）                                 |
| TBYTE  | 80-bit | 十字节整数 (T=Ten-byte)，有时用于保存FPU数据 |
| REAL4  | 32-bit | 短实数（4字节IEEE浮点）                      |
| REAL8  | 64-bit | 长实数（8字节IEEE浮点）                      |
| REAL10 | 80-bit | 扩展实数（10字节IEEE浮点）                   |

```assembly
.data
    ; 8位无符号整数
    var_byte    DB      128         ; (0 ~ 255)

    ; 8位有符号整数
    var_sbyte   DB      -64         ; (-128 ~ 127)

    ; 16位无符号整数
    var_word    DW      30000       ; (0 ~ 65535)

    ; 16位有符号整数
    var_sword   DW      -12345      ; (-32768 ~ 32767)

    ; 32位无符号整数
    var_dword   DD      4000000000  ; (0 ~ 4294967295)

    ; 32位有符号整数
    var_sdword  DD      -2000000000 ; (-2147483648 ~ 2147483647)

    ; 48位“远”指针/整数
    var_fword   DF      123456789ABC ; 48位可记为6字节，写法如：DF 12 34 56 78 9A BC

    ; 64位整数
    var_qword   DQ      1234567890123456789

    ; 80位整数/10字节（通常用于FPU数据）
    var_tbyte   DT      1000000000000000000 ; 一般用来持有大整数或FPU堆栈数据

    ; 32位IEEE短实数 (float)
    var_real4   DD      3.14   ; 单精度

    ; 64位IEEE长实数 (double)
    var_real8   DQ      3.14159265358979   ; 双精度

    ; 80位IEEE扩展实数
    var_real10  DT      2.718281828459045235 ; 扩展精度
.code
end
```

Sol 4.2:

```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode:DWORD

SUNDAY     = 0 ; SUNDAY EQU 0 是更常见的写法
MONDAY     = 1
TUESDAY    = 2
WEDNESDAY  = 3
THURSDAY   = 4
FRIDAY     = 5
SATURDAY   = 6
; 常量定义在 .data 之前
.data	
	daysOfWeek BYTE SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
.code
main PROC
	; 如果你还没学 Loop，可以先不做下面遍历
	mov ecx,7
	mov esi,OFFSET daysOfWeek
	L1:
		mov al,esi
		; add al,'0' 把al内数字转成ASCII字符，再INVOKE stdOut打印，繁琐不展示了
		add,esi TYPE daysOfWeek
		loop L1 ; 自动做ecx -= 1, 当ecx == 0时自动退出

	INVOKE ExitProcess,0
main ENDP
END main
```

daysOfWeek 的注意事项：

1. 用 BYTE 不用 DWORD，因为 BYTE 才是常量，DWORD 定义的是能被别人修改的（本质还是变量）

2. “数组”末尾要不要加 `,0` 的问题**一句话总结：用LOOP等循环次数已知，不必加 ,0；用0结尾表示内容终止时，才需要 ,0** ，注意两种情况都要用 Loop 搭配 index of source： `esi, OFFSET myArray` 来先指向数组开头，然后移动

    1. 知道循环次数时 `LOOP` 不需要 `,0`
    - `LOOP` 指令用的是 ECX（或 CX）寄存器计数：你只要**事先知道要循环几次**（比如7次），
    - ECX 设为7，`LOOP` 每次自动减1，到0跳出，
    - **数据数组不需要额外加结尾0来判断是否结束**。
    
    2. 用`ecx`配合 LOOP “查找0结尾”数组，才需要 `,0`
    
        有些情况下，你想**遍历一个变长数组**，以 0 为“终止符”，
    
        - 就像 C 字符串用 `NULL` 做结尾一样，
        - 这时，需要数组最后加一个 0，
        - 有时`ecx`只是临时变量，不同作用——不是固定循环计数，而是配合查找结尾。

Sol 4.3:

```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode:DWORD

; 定义符号名，对应字符串字面量
HELLO_TEXT  EQU <"Hello",0> ; 用 < > 避免读取错误，EQU 比 = 更正式
WORLD_TEXT  EQU <"World!",0>
AUTHOR_TEXT EQU <"YourName",0>
; 常量定义在 .data 之前
.data	
	str1 BYTE HELLO_TEXT
	str2 BYTE WORLD_TEXT
	str3 BYTE AUTHOR_TEXT
	
; 后面省略了
```



---

## Practice 2

Programming Exercises P129

1, 2, 3, 4, 5, 6, 7

---

1. Carry Flag  
Write a program that uses addition and subtraction to set and clear the Carry flag. After each instruction, insert the `call DumpRegs` statement to display the registers and flags. Using comments, explain how (and why) the Carry flag was affected by each instruction.

2. Zero and Sign Flags  
Write a program that uses addition and subtraction to set and clear the Zero and Sign flags. After each addition or subtraction instruction, insert the `call DumpRegs` statement (see Section 3.2) to display the registers and flags. Using comments, explain how (and why) the Zero and Sign flags were affected by each instruction.

3. Overflow Flag  
Write a program that uses addition and subtraction to set and clear the Overflow flag. After each addition or subtraction instruction, insert the `call DumpRegs` statement (see Section 3.2) to display the registers and flags. Using comments, explain how (and why) the Overflow flag was affected by each instruction. Include an ADD instruction that sets both the Carry and Overflow flags.

4. Direct-Offset Addressing  
Insert the following variables in your program:
```
.data
Uarray WORD 1000h,2000h,3000h,4000h
Sarray SWORD -1,-2,-3,-4
```
Write instructions that use direct-offset addressing to move the four values in Uarray to the EAX, EBX, ECX, and EDX registers. When you follow this with a `call DumpRegs` statement (see Section 3.2), the following register values should display:
```
EAX=00001000 EBX=00002000 ECX=00003000 EDX=00004000
```
Next, write instructions that use direct-offset addressing to move the four values in Sarray to the EAX, EBX, ECX, and EDX registers. When you follow this with a `call DumpRegs` statement, the following register values should display:
```
EAX=FFFFFFFF EBX=FFFFFFFE ECX=FFFFFFFD EDX=FFFFFFFC
```

5. Reverse an Array  ==经典好题，二分优化 反转数组==
Use a loop with indirect or indexed addressing to reverse the elements of an integer array in place. Do not copy the elements to any other array. Use the SIZEOF, TYPE, and LENGTHOF operators to make the program as flexible as possible if the array size and type should be changed in the future. Optionally, you may display the modified array by calling the DumpMem method from the Irvine32 library. See Chapter 5 for details. (A VideoNote for this exercise is posted on the Web site.)
6. Fibonacci Numbers  ==经典斐波那契==，此题开始我就正式地用 INCLUDE Irvine32.inc 了哈
Write a program that uses a loop to calculate the first seven values of the Fibonacci number sequence, described by the following formula: Fib(1) = 1, Fib(2) = 1, Fib(n) = Fib(n − 1) + Fib(n − 2). Place each value in the EAX register and display it with a `call DumpRegs` statement (see Section 3.2) inside the loop.

7. Arithmetic Expression  
Write a program that implements the following arithmetic expression:

```
EAX = -val2 + 7 - val3 + val1
```
Use the following data definitions:
```
val1 SDWORD 8
val2 SDWORD -15
val3 SDWORD 20
```
In comments next to each instruction, write the hexadecimal value of EAX. Insert a `call DumpRegs` statement at the end of the program.

==The new template is as follows==

```assembly
INCLUDE Irvine32.inc
	; Constant
.data
    ; declare variables here

.code
main PROC
    ; write your code here
    exit ; 不用INVOKE ExitProcess,0
main ENDP

END main
```

---

### Solution(s):

Sol 1:

```assembly
;.386
;.model flat,stdcall
;.stack 4096
;ExitProcess PROTO, dwExitCode:DWORD
INCLUDE Irvine32.inc
.data
	;var1 DWORD 0FFFFh ; 虽然这个例子没用，但是不加前置0也会报错！
	var1 DWORD 0FFFFFFFFh ; 哪怕有8个F了，还是要前置 0 后置 h
.code
main PROC
	mov eax,var1 ; 不能用 ax，会报错，因为 16-bit 和 DWORD(32-bit) 不匹配
	call DumpRegs

	add eax,1h
	call DumpRegs

	sub eax,2h
	call DumpRegs

	exit
main ENDP
END main
```
输出如下：可见 CF = 0 和 CF = 1 的变化

```
  EAX=FFFFFFFF  EBX=008B7000  ECX=000510AA  EDX=000510AA
  ESI=000510AA  EDI=000510AA  EBP=00AFFC64  ESP=00AFFC58
  EIP=0005366A  EFL=00000246  CF=0  SF=0  ZF=1  OF=0  AF=0  PF=1


  EAX=00000000  EBX=008B7000  ECX=000510AA  EDX=000510AA
  ESI=000510AA  EDI=000510AA  EBP=00AFFC64  ESP=00AFFC58
  EIP=00053672  EFL=00000257  CF=1  SF=0  ZF=1  OF=0  AF=1  PF=1


  EAX=FFFFFFFE  EBX=008B7000  ECX=000510AA  EDX=000510AA
  ESI=000510AA  EDI=000510AA  EBP=00AFFC64  ESP=00AFFC58
  EIP=0005367A  EFL=00000293  CF=1  SF=1  ZF=0  OF=0  AF=1  PF=0


C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 5580)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```

Sol 2:


```assembly
;.386
;.model flat,stdcall
;.stack 4096
;ExitProcess PROTO, dwExitCode:DWORD
INCLUDE Irvine32.inc

.data
    var1 DWORD 01h

.code
main PROC
    mov eax, var1
    call DumpRegs

    sub eax, 01h
    call DumpRegs

    sub eax, 01h
    call DumpRegs

    add eax, 03h
    call DumpRegs

    exit
main ENDP
END main
```

输出如下：可见 ZF 和 SF 的变化

```
  EAX=00000001  EBX=00537000  ECX=003D10AA  EDX=003D10AA
  ESI=003D10AA  EDI=003D10AA  EBP=0076F990  ESP=0076F984
  EIP=003D366A  EFL=00000246  CF=0  SF=0  ZF=1  OF=0  AF=0  PF=1


  EAX=00000000  EBX=00537000  ECX=003D10AA  EDX=003D10AA
  ESI=003D10AA  EDI=003D10AA  EBP=0076F990  ESP=0076F984
  EIP=003D3672  EFL=00000246  CF=0  SF=0  ZF=1  OF=0  AF=0  PF=1


  EAX=FFFFFFFF  EBX=00537000  ECX=003D10AA  EDX=003D10AA
  ESI=003D10AA  EDI=003D10AA  EBP=0076F990  ESP=0076F984
  EIP=003D367A  EFL=00000297  CF=1  SF=1  ZF=0  OF=0  AF=1  PF=1


  EAX=00000002  EBX=00537000  ECX=003D10AA  EDX=003D10AA
  ESI=003D10AA  EDI=003D10AA  EBP=0076F990  ESP=0076F984
  EIP=003D3682  EFL=00000213  CF=1  SF=0  ZF=0  OF=0  AF=1  PF=0


C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 12948)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```

Sol 3:

```assembly
;.386
;.model flat,stdcall
;.stack 4096
;ExitProcess PROTO, dwExitCode:DWORD
INCLUDE Irvine32.inc

.data
    var1 DWORD 7FFFFFFFh ; 最大的 32 位有符号正整数

.code
main PROC
    mov eax, var1       ; 载入最大正整数到 EAX
    add eax, 1          ; 这将导致结果超出最大正整数，设置溢出标志 OF
    call DumpRegs       ; 显示寄存器状态，观测 OF 是否被设置

    exit
main ENDP
END main
```
输出如下：可见 OF = 1

```

  EAX=80000000  EBX=00746000  ECX=002310AA  EDX=002310AA
  ESI=002310AA  EDI=002310AA  EBP=008FFC60  ESP=008FFC54
  EIP=0023366D  EFL=00000A96  CF=0  SF=1  ZF=0  OF=1  AF=1  PF=1


C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 7832)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```

Sol 4:

```assembly
;.386
;.model flat,stdcall
;.stack 4096
;ExitProcess PROTO, dwExitCode:DWORD
INCLUDE Irvine32.inc

.data
Uarray WORD 1000h,2000h,3000h,4000h
Sarray SWORD -1,-2,-3,-4

.code
main PROC
    ; mov esi,OFFSET Uarray 先让指针指向 Uarray 的起始位置
    ; movzx eax, [esi]        ; EAX = 00001000
    ; movzx ebx, [esi+2]      ; EBX = 00002000
    ; movzx ecx, [esi+4]      ; ECX = 00003000
    ; movzx edx, [esi+6]      ; EDX = 00004000

    movzx eax,Uarray[0] ; 如果用 mov 则不能用eax！！英文左右 size 必须一样！但是题目想让我们高位补 0， 那我就用movzx了
    movzx ebx,Uarray[2]
    movzx ecx,Uarray[4]
    movzx edx,Uarray[6]
    call DumpRegs

    movsx eax,Sarray[0]
    movsx ebx,Sarray[2]
    movsx ecx,Sarray[4]
    movsx edx,Sarray[6]
    call DumpRegs

    exit
main ENDP
END main
```

输出与题目要求一致：

```

  EAX=00001000  EBX=00002000  ECX=00003000  EDX=00004000
  ESI=00F810AA  EDI=00F810AA  EBP=0053FB54  ESP=0053FB48
  EIP=00F83681  EFL=00000246  CF=0  SF=0  ZF=1  OF=0  AF=0  PF=1


  EAX=FFFFFFFF  EBX=FFFFFFFE  ECX=FFFFFFFD  EDX=FFFFFFFC
  ESI=00F810AA  EDI=00F810AA  EBP=0053FB54  ESP=0053FB48
  EIP=00F836A2  EFL=00000246  CF=0  SF=0  ZF=1  OF=0  AF=0  PF=1


C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 25060)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```

**注意！！**

用esi更适合配合循环读取很大的或数量不确定的数组，用esi而不是直接索引读数组时：

==用movzx或movsx	而不是mov！！==

- `movzx eax, word ptr [esi]`
    只取2字节（16位），**高位补0**，适合无符号数。
- `movsx eax, word ptr [esi]`
    只取2字节（16位），**高位补符号位**，适合有符号数。

- **如果你用 `mov eax, [esi]`，会多读2字节，结果不对。**
- **如果你用 `mov ax, [esi]`，只会影响16位，EAX高16位保持原值，也不是题目想要的效果。**
- **用 `movzx` 或 `movsx`，能保证EAX等寄存器的值和题目要求完全一致。**



Sol 5:

```assembly
;.386
;.model flat,stdcall
;.stack 4096
;ExitProcess PROTO, dwExitCode:DWORD
INCLUDE Irvine32.inc

.data
array DWORD 1, 2, 3, 4, 5

.code
main PROC
    mov esi,OFFSET array ; index of source points to array[first]
    mov edi,OFFSET array + SIZEOF array - TYPE array ; index of destination points to array[last]
    ; SIZEOF array - TYPE array means  
    ; DWORD array: |1 2 3 4|5 7 6 8|9 10 11 12| we can get the "9th" from the memory
    mov ecx, LENGTHOF array / 2 ; only need arrLen / 2 times loop
    L1:
        ; Swap left & right, remember add [] for esi & edi !!
        mov eax,[esi] ; mov eax, array[esi]，something to notice
        mov ebx,[edi] 
        mov [esi],ebx
        mov [edi],eax
        ; [esi] is not esi, [edi] is not edi!

        ; Move pointers
        add esi, TYPE array ; add
        sub edi, TYPE array ; sub
        loop L1
    ; DumpMem must clarify the parameters
    ; call DumpMem Error!!! because did not clarify parameters
    ; 我用中文注释了，无论用不用含ebx的交换方法，都要明确下面3个参数
    mov esi, OFFSET array   ; 1. 数组起始地址
    mov ecx, LENGTHOF array ; 2. 元素数量
    mov ebx, TYPE array     ; 3. 每个元素大小（DWORD=4）
    call DumpMem            ; 调用 DumpMem 显示内存

    exit
main ENDP
END main
; Swap left & right 这种更好理解，不用 ebx 了
; mov eax, [esi]
; xchg eax, [edi]  ; 交换 eax 和 [edi], xchg 要求必须 >= 一方是寄存器
; mov [esi], eax
```
- **`mov eax, [esi]`**：适用于 `esi` 是绝对地址（指针）。
- **`mov eax, array[esi]`**：适用于 `esi` 是相对于数组首地址的字节偏移量（索引）。

输出如下（reversed）：

```

Dump of offset 00A76000
-------------------------------
00000005  00000004  00000003  00000002  00000001

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 23660)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



Sol 6:

核心：

```assembly
; 假设 L1 代表 第一轮循环
; 初始	     交换↓ L1     交换↓ L2
; eax = 1    1+1=2 ->1    1+2=3 ->2
; ebx = 1        1 ->2        2 ->3

; n 从 3 涨到 7
add eax, ebx ; eax先加ebx到第三个数: 2
call WriteInt ; 打印
call Crlf ; 换行
xchg ebx, eax ; ebx变成2，eax回退到ebx的值（不是位置！）
; 上面理解不了的话看下面
; 1 1 2 3
; a b
;   a b 第一轮做完后
```

简洁版：

```assembly
INCLUDE Irvine32.inc

.data

.code
main PROC
    mov eax,1
    mov ebx,1
    mov ecx,5 ; 计数器勿忘
    call WriteInt ; +1 Fib(1)
    call Crlf
    mov eax,ebx
    call WriteInt ; +1 Fib(2)
    call Crlf
    L1:
        add eax,ebx
        call WriteInt ; Fib(3-7)
        call Crlf
        xchg eax,ebx
        loop L1
    exit
main ENDP
END main
```

详解版：

```assembly
INCLUDE Irvine32.inc

.data

.code
main PROC
    ; 初始化前两个斐波那契数
    mov eax, 1      ; Fib(1) = 1
    mov ebx, 1      ; Fib(2) = 1

    call WriteInt   ; 显示第一个数
    call Crlf

    mov eax, ebx    ; 把 ebx mov给 eax 才能显示第二个数
    call WriteInt   
    call Crlf
    
    mov ecx, 5      ; count 剩余需要计算的数量（Fib3-Fib7）

L1:
    ; 计算新值
    add eax, ebx    ; eax = Fib(n)
    call WriteInt   ; 显示当前 eax 内的数
    call Crlf
    
    ; 更新寄存器状态
    ; xchg 换的是值而不是地址！！
    xchg eax, ebx   ; ebx = Fib(n), eax = Fib(n-1)
    loop L1
    
    exit
main ENDP
END main
```
输出：

```
+1
+1
+2
+3
+5
+8
+13

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 31824)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



Sol 7:

在汇编语言中，**SDWORD (Signed Double Word)** 表示 **32位有符号整数**，因此所有数值都必须用32位二进制表示。

==求 -15 的十六进制表示==

**步骤1：取15的二进制**

```
0000 0000 0000 0000 0000 0000 0000 1111 (二进制32位) 0x0000000F (十六进制)
```

**步骤2：按位取反（NOT运算）**

```
1111 1111 1111 1111 1111 1111 1111 0000  (0xFFFFFFF0)
```

**步骤3：加1（Two's Complement规则）**

```
1111 1111 1111 1111 1111 1111 1111 0000  
+                                     1
---------------------------------------
1111 1111 1111 1111 1111 1111 1111 0001  (0xFFFFFFF1)
```

32位分成8份，每份对应一个十六进制数 ↑ 

==先填**低**位再填**高**位，如 **20decimal 是 00000014h** 而不是 000000F5h==

**最终结果**

- **-15的二进制**：`1111 1111 1111 1111 1111 1111 1111 0001`（32位）
- **-15的十六进制**：`0xFFFFFFF1`

```assembly
INCLUDE Irvine32.inc
.data
	val1 SDWORD 8 ; 00000008h
	val2 SDWORD -15 ; FFFFFFF1h
	val3 SDWORD 20 ; 00000014h
.code
main PROC
	; 求 EAX = -val2 + 7 - val3 + val1 都是些十进制的值
	; 额外要求：在每条指令旁的注释中，写出 EAX 的十六进制值。在程序末尾插入 “调用 DumpRegs ”语句。
	mov eax,val2  ; EAX = val2 = FFFFFFF1h (-15)
	neg eax		  ; EAX = -EAX = 0000000Fh (15)
	add eax,7	  ; EAX = 0x0000000F + 7 = 00000016h (22) 不是17h哦！ 
	sub eax,val3  ; EAX = 0x00000016 - 0x00000014 = 0x00000002 (2)
	add eax,val1  ; EAX = 0x00000002 + 0x00000008 = 0x0000000A (10)
	call DumpRegs ; 验证我算对没
	
	exit
main ENDP
END main

```

算对了：

```

  EAX=0000000A  EBX=007EE000  ECX=005210AA  EDX=005210AA
  ESI=005210AA  EDI=005210AA  EBP=0040FF68  ESP=0040FF5C
  EIP=0052367B  EFL=00000206  CF=0  SF=0  ZF=0  OF=0  AF=0  PF=1


C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 30140)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .

```



---

## Practice 3

Programming Exercises P178

1, 3, 5, 6, 7

---

1. Draw Text Colors
    Write a program that displays the same string in four different colors, using a loop. Call the Set-TextColor procedure from the book’s link library. Any colors may be chosen, but you may find it easiest to change the foreground color.


3. Simple Addition (1)  
    Write a program that clears the screen, locates the cursor near the middle of the screen, prompts the user for two integers, adds the integers, and displays their sum.  


5. BetterRandomRange Procedure  
    The RandomRange procedure from the Irvine32 library generates a pseudorandom integer between 0 and N – 1. Your task is to create an improved version that generates an integer between M and N – 1. Let the caller pass M in EBX and N in EAX. If we call the procedure BetterRandomRange, the following code is a sample test:  

```assembly
mov ebx,-300      ; lower bound
mov eax,100       ; upper bound
call BetterRandomRange
```
Write a short test program that calls BetterRandomRange from a loop that repeats 50 times. Display each randomly generated value.  

6. Random Strings  
Write a program that generates and displays 20 random strings, each consisting of 10 capital letters {A..Z}. (A VideoNote for this exercise is posted on the Web site.)  

7. Random Screen Locations  
Write a program that displays a single character at 100 random screen locations, using a timing delay of 100 milliseconds. Hint: Use the GetMaxXY procedure to determine the current size of the console window.

---

### Solution(s):



---

## Practice 4 真

Programming Exercises P225

1, 8, 11

---

1. Counting Array Values

Write an application that does the following: 

(1) fill an array with 50 random integers; 

(2) loop through the array, displaying each value, and count the number of negative values; 

(3) after the loop finishes, display the count. 

Note: The Random32 procedure from the Irvine32 library generates random integers.



8. Boolean Calculator (2)

Continue the solution program from the preceding exercise by implementing the following procedures:

 • AND_op: Prompt the user for two hexadecimal integers. AND them together and display the result in hexadecimal.

 • OR_op: Prompt the user for two hexadecimal integers. OR them together and display the result in hexadecimal.

 • NOT_op: Prompt the user for a hexadecimal integer. NOT the integer and display the result in hexadecimal.

 • XOR_op: Prompt the user for two hexadecimal integers. Exclusive-OR them together and display the result in hexadecimal.



11. Message Encryption 

Revise the encryption program in Section 6.3.4 in the following manner: Let the user enter an encryption key consisting of multiple characters. Use this key to encrypt and decrypt the plain-text by XORing each character of the key against a corresponding byte in the message. Repeat the key as many times as necessary until all plain-text bytes are translated. Suppose, for example, the key equals "ABXmv#7". This is how the key would align with the plain-text bytes:

| Plain text | T    | h    | i    | s    |      | i    | s    |      | a    |      | P    | l    | a    | i    | n    |      | t    | e    | x    | t    |      | m    | e    | s    | s    | a    | g    | e    | (etc.) |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------ |
| Key        | A    | B    | X    | m    | v    | #    | 7    | A    | B    | X    | m    | v    | #    | 7    | A    | B    | X    | m    | v    | #    | 7    | A    | 8    | X    | m    | v    | #    | 7    |        |

(The key repeats until it equals the length of the plain text...)

(A VideoNote for this exercise is posted on the Web site.)

---

### Solution(s):



---

## Practice 4 假

Exercise 1: Copying a Word Array to Dword array 

```assembly
.386 
.model flat,stdcall 
.stack 4096 
ExitProcess proto,dwExitCode:dword 
.data 
source word 1,2,3,4,5,6,7,8,9,10 
target dword 10 DUP(?) 
```

Exercise 2: Use a loop with indirect or indexed addressing to reverse the 
elements of an integer array in place. 

```assembly
.386 
.model flat,stdcall 
.stack 4096 
ExitProcess proto,dwExitCode:dword 
.data 
array dword 1,5,6,8,0Ah,1Bh,1Eh,22h,2Ah,32h 
```

Exercise 3: Exchanging adjacent pairs of array values, exchange between 
element i and element i+1, exchange between element i+2 and element 
i+3,so on.  

```assembly
.386 
.model flat,stdcall 
.stack 4096 
ExitProcess proto,dwExitCode:dword 
.data 
array DWORD 1,2,3,4,5,6,7,8,9,10,11,12  
```

Exercise 4: Write a program that uses a loop to calculate the first seven values of the 
Fibonacci number sequence, described by the following formula: Fib(1) = 1, 
Fib(2) = 1, Fib(n) = Fib(n _1) _ Fib(n _ 2). 



---

## Practice 5

Programming Exercises P225

2, 3, 4, 9, 10

---

题目文字：
2. Selecting Array Elements
Implement the following C++ code in assembly language, using the block-structured .IF and .WHILE directives. Assume that all variables are 32-bit signed integers:

```cpp
int array[] = {10,60,20,33,72,89,45,65,72,18};
int sample = 50;
int ArraySize = sizeof array / sizeof sample;
int index = 0;
int sum = 0;
while( index < ArraySize )
{
    if( array[index] <= sample )
    {
        sum += array[index];
    }
    index++;
}
```

Optional: Draw a flowchart of your code.

3. Test Score Evaluation (1)
Using the following table as a guide, write a program that asks the user to enter an integer test score between 0 and 100. The program should display the appropriate letter grade:

| Score Range | Letter Grade |
| ----------- | ------------ |
| 90 to 100   | A            |
| 80 to 89    | B            |
| 70 to 79    | C            |
| 60 to 69    | D            |
| 0 to 59     | F            |

4. Test Score Evaluation (2)
Using the solution program from the preceding exercise as a starting point, add the following features:
- Run in a loop so that multiple test scores can be entered.
- Accumulate a counter of the number of test scores.
- Perform range checking on the user’s input: Display an error message if the test score is less than 0 or greater than 100. (A VideoNote for this exercise is posted on the Web site.)

9. Probabilities and Colors
Write a program that randomly chooses among three different colors for displaying text on the screen. Use a loop to display 20 lines of text, each with a randomly chosen color. The probabilities for each color are to be as follows: white = 30%, blue = 10%, green = 60%. Hint: Generate a random integer between 0 and 9. If the resulting integer is in the range 0 to 2, choose white. If the integer equals 3, choose blue. If the integer is in the range 4 to 9, choose green. (A VideoNote for this exercise is posted on the Web site.)

10. Print Fibonacci until Overflow
Write a program that calculates and displays the Fibonacci number sequence {1, 1, 2, 3, 5, 8, 13, ...}, stopping only when the Carry flag is set. Display each unsigned decimal integer value on a separate line.

---

### Solution(s):



---

## Practice 6

Programming Exercises P267

3, 4, 6

---

3. ShowFileTime  
The time stamp of a MS-DOS file directory entry uses bits 0 through 4 for the number of 2-second increments, bits 5 through 10 for the minutes, and bits 11 through 15 for the hours (24-hour clock). For example, the following binary value indicates a time of 02:16:14, in hh:mm:ss format:  

```
00010 010000 00111
```

Write a procedure named ShowFileTime that receives a binary file time value in the AX register and displays the time in hh:mm:ss format.

4. Encryption Using Rotate Operations  
Write a program that performs simple encryption by rotating each plaintext byte a varying number of positions in different directions. For example, in the following array that represents the encryption key, a negative value indicates a rotation to the left and a positive value indicates a rotation to the right. The integer in each position indicates the magnitude of the rotation:

```
key BYTE –2, 4, 1, 0, –3, 5, 2, –4, –4, 6
```

Your program should loop through a plaintext message and align the key to the first 10 bytes of the message. Rotate each plaintext byte by the amount indicated by its matching key array value. Then, align the key to the next 10 bytes of the message and repeat the process. (A VideoNote for this exercise is posted on the Web site.)


6. Greatest Common Divisor (GCD)  
    The greatest common divisor (GCD) of two integers is the largest integer that will evenly divide both integers. The GCD algorithm involves integer division in a loop, described by the following C++ code:

```cpp
int GCD(int x, int y)
{
    x = abs(x);         // absolute value
    y = abs(y);
    do {
        int n = x % y;
        x = y;
        y = n;
    } while (y > 0);
    return x;
}
```

Implement this function in assembly language and write a test program that calls the function using samples of different values. Display the results of each calculation.

---

### Solution(s):
