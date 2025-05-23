# 汇编语言 x86 练习题解

计科2302胡德东

## Irvine32注意事项


一份 **Irvine32.inc 常用输入/输出函数与寄存器的对应总结**，再配上一些小例子😊

------

### 🧠 一览表：Irvine32 常用函数与寄存器

重点记住**有==输入寄存器==**的即可

ReadInt 输出到 EAX ， ReadChar 输出到 AL

| 函数名        | 用途                              | ==输入寄存器==                       | 输出寄存器     | 说明                         |
| ------------- | --------------------------------- | ------------------------------------ | -------------- | ---------------------------- |
| `WriteInt`    | 输出整数                          | `EAX`                                | 无             | 显示 `EAX` 中的整数          |
| `WriteString` | 输出字符串                        | `EDX`（字符串地址）                  | 无             | 显示 `EDX` 指向的字符串      |
| `WriteChar`   | 输出单个字符                      | `AL`                                 | 无             | 显示一个字符                 |
| `ReadInt`     | 读取整数                          | 无                                   | `EAX`          | 用户输入 -> 存到 `EAX`       |
| `ReadChar`    | 读取字符                          | 无                                   | `AL`           | 用户输入字符 -> 存到 `AL`    |
| `ReadString`  | 读取字符串                        | `EDX`（缓冲区地址）`ECX`（最大长度） | 无             | 把用户输入的字符串存进缓冲区 |
| `Gotoxy`      | 设置光标位置                      | `DH`（行）`DL`（列）                 | 无             | 控制输出的显示位置           |
| `Crlf`        | 换行                              | 无                                   | 无             | 显示回车换行                 |
| `Clrscr`      | 清屏                              | 无                                   | 无             | 清空控制台                   |
| `RandomRange` | 生成 0…(range−1) 之间的伪随机整数 | `EAX` = range 上限                   | `EAX` = 随机值 | 生成随机数                   |

------

✨ 例子1：输出字符串和整数

```asm
.data
msg BYTE "Your number is: ", 0

.code
main PROC
    ; 读取用户输入的整数
    call ReadInt         ; 输入结果存入 EAX

    ; 显示提示字符串
    mov edx, OFFSET msg
    call WriteString     ; 显示 "Your number is: "

    ; 显示整数
    call WriteInt        ; 输出 EAX 中的值

    call Crlf
    exit
main ENDP
END main
```

------

✨ 例子2：读取字符并再次显示

```asm
.data
prompt BYTE "Enter a letter: ", 0

.code
main PROC
    mov edx, OFFSET prompt
    call WriteString     ; 显示提示

    call ReadChar        ; 读取用户输入，保存在 AL

    call Crlf
    call WriteChar       ; 输出 AL 中的字符

    call Crlf
    exit
main ENDP
END main
```

------

✨ 例子3：读取字符串并输出（ReadString）

```asm
.data
buffer BYTE 20 DUP(0)     ; 字符串缓冲区（最多 19 字符 + 结束符）
prompt BYTE "Enter your name: ", 0

.code
main PROC
    mov edx, OFFSET prompt
    call WriteString

    mov edx, OFFSET buffer  ; 缓冲区地址放入 EDX
    mov ecx, SIZEOF buffer  ; 最大长度放入 ECX
    call ReadString         ; 从键盘读取字符串

    call Crlf
    call WriteString        ; 输出用户输入的字符串（EDX 已经是 buffer）

    call Crlf
    exit
main ENDP
END main
```

------

📌 提醒小技巧

- `ReadInt` → 返回的整数在 `EAX`
- `ReadChar` → 返回的字符在 `AL`
- `ReadString` → 用户输入写入你指定的内存区（通常是 buffer）
- `WriteString` → EDX 要指向字符串地址，不能直接给文字

------



## Practice 1

Write assembly program as following template

The template is as follows
```assembly
.386
.model flat,stdcall
.stack 4096 ; 给程序分配 4KB 的堆栈，保证所有过程调用和库函数都有足够的空间
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
L1: ; 标签的主流写法就是左对齐贴死
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

1. Draw Text Colors ==不会考==
    Write a program that displays the same string in four different colors, using a loop. Call the Set-TextColor procedure from the book’s link library. Any colors may be chosen, but you may find it easiest to change the foreground color.


3. Simple Addition (1)  ==不会考==
    Write a program that clears the screen, locates the cursor near the middle of the screen, prompts the user for two integers, adds the integers, and displays their sum.  


5. BetterRandomRange Procedure  
    The RandomRange procedure from the Irvine32 library generates a pseudorandom 伪随机  integer between 0 and N – 1. Your task is to create an improved version that generates an integer between M and N – 1. Let the caller pass M in EBX and N in EAX. If we call the procedure BetterRandomRange, the following code is a sample test:  

```assembly
mov ebx,-300      ; lower bound
mov eax,100       ; upper bound
call BetterRandomRange
```
Write a short test program that calls BetterRandomRange from a loop that repeats 50 times. Display each randomly generated value.  

6. Random Strings  <font style=background:#00C49F>真是个好题目！看完解你就知道了！</font>
Write a program that generates and displays 20 random strings, each consisting of 10 capital letters {A..Z}. (A VideoNote for this exercise is posted on the Web site.)  

7. Random Screen Locations  ==很好玩，但是期末手写肯定不考，所以不用复习这题==
Write a program that displays a single character at 100 random screen locations, using a timing delay of 100 milliseconds. Hint: Use the GetMaxXY procedure to determine the current size of the console window.

---

### Solution(s):

Sol 1:

红绿蓝黄四种，不贴图片了

```assembly
INCLUDE Irvine32.inc

.data
message BYTE "Hello, colorful world!",0
colorsArr DWORD lightRed, lightGreen, lightBlue, yellow  ; Array of color attributes

.code
main PROC
    mov ecx, LENGTHOF colorsArr  ; Loop counter = number of colorsArr
    mov esi, OFFSET colorsArr    ; Pointer to colorsArr array

displayLoop:
    ; Set text color
    mov eax, [esi]            ; Get current color
    call SetTextColor ; 设置颜色的指令

    ; Display the string
    mov edx, OFFSET message
    call WriteString
    call Crlf                 ; New line

    add esi, TYPE colorsArr      ; Move to next color in array
    loop displayLoop

    ; Reset to default color (white on black)
    mov eax, white + (black SHL 4)
    call SetTextColor

    exit
main ENDP
END main
```



Sol 3:

```assembly
;.386
;.model flat,stdcall
;.stack 4096
; 下面的库已经涵盖上面内容
INCLUDE Irvine32.inc

.data
	prompt1 BYTE "Enter the first integer: ",0  ; 请一定要用BYTE，最适合存储字符串或字符数组，符合本质
	prompt2 BYTE "Enter the second integer: ",0
	sumMsg BYTE "The sum is: ",0
.code
main PROC
	call Clrscr
	; 第一次提示：行 12 列 40
    mov dh, 12         ; hang行号（0开始，12 是第13行）
    mov dl, 40         ; lie列号（0开始，40 是第41列）
    call Gotoxy		   ; 移动光标
	; index of destination 是“指针”！
	mov edx,OFFSET prompt1
	call WriteString
	call ReadInt	   ; eax 自动读取
	mov ebx,eax		   ; 第 1 个整数暂存在 EBX
    call Crlf		   ; 换行

	; 第二次提示：行 13 列 40
    mov dh,13
    mov dl,40
    call Gotoxy
	mov edx,OFFSET prompt2
	call WriteString
	call ReadInt	   ; eax 自动读取
	call Crlf
	
	add eax,ebx		   ; eax += ebx

	; 显示结果：行 14 列 40
    mov dh,14
    mov dl,40
    call Gotoxy
	mov edx,OFFSET sumMsg
    call WriteString
    call WriteInt	   ; 显示结果（EAX 中的值）
    call Crlf
	exit
main ENDP
END main
; 每次列不变，行 += 1
```

输出在我电脑上看其实是居中的（且左对齐）

```
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Enter the first integer: 11
                                        Enter the second integer: 22
                                        The sum is: +33

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 25128)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . . 
```

🌟 背景知识：`Gotoxy` 是什么？

在 `Irvine32.inc` 库中，`Gotoxy` 是一个用于**设置光标位置**的函数。这个函数可以让你把文字输出的位置定位在屏幕上的任意位置。

它的参数通过 **`DH`（行）** 和 **`DL`（列）** 来传递。

------

🖥️ 屏幕坐标怎么理解？

- **屏幕上的位置**是用“行”和“列”来表示的，就像坐标轴一样。
- 屏幕的左上角是 **(0,0)**。
- `DH` 表示第几行（从上往下）。
- `DL` 表示第几列（从左往右）。



Sol 5:

原 RandomRange :

```assembly
INCLUDE Irvine32.inc

.data
	
.code
main PROC
	mov ebx,-300      ; lower bound
	mov eax,100       ; upper bound
	call RandomRange  ; [0,100)
	call WriteInt
	call Crlf
	exit
main ENDP
END main

```

现 BetterRandomRange :

```assembly
INCLUDE Irvine32.inc

.data
	
.code
; 自定义"函数"写在 .code 后
BetterRandomRange PROC
	; Input: EBX = M, EAX = N
    ; Output: EAX = Random number between M and N-1
	push edx				; 保护 edx

	; 简单记：b 先 -a，random后 再 +a
	sub eax, ebx			; EAX = N - M，如果没这一步，范围错为：[-300,-200)
	call RandomRange		; EAX = [0, N-M) 随机 num
	add eax,ebx				; EAX = EAX + M → [M, N)

	pop edx
	ret						; return 千万不能忘
BetterRandomRange ENDP

main PROC
	call Randomize         ; 初始化随机种子，不然结果一定是 -206 （我的电脑）
	; Irvine32 的 RandomRange 是伪随机数生成器。它内部使用一个确定性算法，如果你不设置种子，它就每次生成一样的结果。
	mov ebx,-300		   ; M lower bound
	mov eax,100			   ; N upper bound
	
	; call RandomRange     ; [0,100)
	call BetterRandomRange ; [-300,100)
	call WriteInt
	call Crlf
	exit
main ENDP
END main

```

输出：

```
+38

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 31276)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



小 tips :

RandomRange` 内部可能修改了 `EDX，所以要保护

```asm
push edx  ; 保存 EDX
;
pop edx
```

这一步的目的是：**保护 EDX 寄存器的值，避免在调用 `RandomRange` 或执行其他操作时破坏原来的数据**。

------

📌 背景知识：哪些寄存器需要保护？

在使用过程调用（`PROC`）时，有一个约定俗成的规则，叫做 **调用约定（Calling Convention）**，Irvine32 遵循的是类似于 Windows 的标准调用约定：

A C D 一般不用保护

| 寄存器 | 用法             | 调用者是否需要保护？ |
| ------ | ---------------- | -------------------- |
| `EAX`  | 函数返回值       | ❌ 不需要（会被覆盖） |
| `ECX`  | 一般用途         | ❌ 不需要             |
| `EDX`  | 一般用途         | ❌ 不需要             |
| `EBX`  | 需要保护的寄存器 | ✅ 需要               |
| `ESI`  | 需要保护的寄存器 | ✅ 需要               |
| `EDI`  | 需要保护的寄存器 | ✅ 需要               |
| `EBP`  | 基址指针         | ✅ 需要               |
| `ESP`  | 栈指针           | ✅ 不可随便修改       |

所以，**`EDX` 是“易被破坏的寄存器”，调用者要自行保护它的值，如果后面还要用它的话。**

------

📍具体到这段代码：

```asm
BetterRandomRange PROC
    push edx            ; 保存 EDX，因为 RandomRange 可能会修改它
    call RandomRange    ; 使用 EAX 作为上限，返回值也在 EAX
    add eax, ebx        ; EAX = 0..(range-1) + lower bound => [M, N-1]
    pop edx             ; 恢复原来的 EDX 内容，避免影响主程序
    ret
BetterRandomRange ENDP
```

- 你在调用 `RandomRange` 时，它**内部可能使用并破坏 `EDX`**（比如输出调试信息或计算），
- 如果你在调用前后还打算用 `EDX`（比如主程序里可能在调用前设置了 `EDX = OFFSET prompt`），
- 那么必须在过程内部 **保存现场（`push edx`）**，用完再 **恢复现场（`pop edx`）**。

------

✅ 小结

- `push edx` 是为了**保存调用前的寄存器状态**，避免副作用。
- 如果你知道 `EDX` 在这段代码里不会被破坏，也不打算再用，可以不写。
- 但作为良好习惯，**写上 `push/pop` 是安全的选择**，尤其当你不知道底层库函数会不会用 `EDX` 的时候。

------

如你继续深入学习汇编，这就是“过程调用”的核心原则之一：**用完，负责还原**。这种技巧在写大型汇编程序中非常重要。



Sol 6:

```assembly
INCLUDE Irvine32.inc

.data

.code
main PROC
	call Randomize	; 初始化随机数种子

	mov ecx,20		; 外层循环计数器(自动减)：生成 20 行
outer_loop:
	mov esi,10		; 内层循环计数器(要手动减，没机制没办法)：每行生成 10 个字符	不一定要用 ESI！你完全可以用其他寄存器，使用 ESI 只是个人习惯
inner_loop:
	mov eax,26		; 千万千万千万别忘了初始化 eax 做随机
	call RandomRange; [0,26) 0 ~ 25
	; 妙手 ↓
	add al,'A'		; 'A' + 数字 转换成随机的大写字母（A = 65也可以直接加65）
	call WriteChar
	dec esi			; 手动减 1
	jnz inner_loop	; jump if not zero 这句话不能少，要不然下行执行不了
	call Crlf
	loop outer_loop
	;jnz outer_loop 这样写是错的！会无限循环
	exit
main ENDP
END main

```

输出（20 行，每行 10 个大写字母）：

```
YASVBAFJES
MKJKLRTBMQ
VWPVLBYZBG
EZWQJPDWWY
MQBGCQKHBV
FRLAYCTNJI
EARKOHCQBB
PUMHVBACEE
VSOXXFZXRG
GSECSCMNZO
NPKTGDCWEJ
VFFQLYHTND
TMBHITJZKT
JNHFLESHNO
CFWNFIAWFA
YUBXQAMXKG
SICSWOSIUC
ANGLJCJNZR
QYNJBPKUWI
OIWQHMEQTR

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 13104)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



Sol 7:

```assembly
; RandomScreenFixed.asm
INCLUDE Irvine32.inc      ; 自带 .386/.model/.stack

.data
    maxCol  DWORD ?       ; 改成 DWORD，让 mov eax,maxCol 合法
    maxRow  DWORD ?
    theChar BYTE '*'      ; 要显示的字符

.code
main PROC
    call Clrscr
    call Randomize
    call GetMaxXY         ; CX=列数，DX=行数
    movzx eax, cx         ; CX→EAX，零扩展
    mov maxCol, eax
    movzx eax, dx         ; DX→EAX
    mov maxRow, eax

    mov ecx, 100
display_loop:
    ; 随机列
    mov eax, maxCol
    call RandomRange      ; EAX←0..maxCol-1
    mov dl, al            ; DL = 列

    ; 随机行
    mov eax, maxRow
    call RandomRange
    mov dh, al            ; DH = 行

    call Gotoxy
    mov al, theChar
    call WriteChar

    mov eax, 100
    call Delay

    loop display_loop

    exit
main ENDP
END main

```

非常好玩哈哈哈！！

```
***********                                                                                                                                                                                                                                                    *                                                                                                                                                                                                                                                                                                                                                                                                                                                                  *******                                                                                                       *                                                                                                                                                                                                                                                                                                                                 ***************                                                                                                                                                                                                                                                                                                                                                                                                ***********************              **********
C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 3520)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .                 **         ********                                                                                                                                                                                                                  ***************                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               *                                                                                                                                                                                                                                       ****** 
```

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



8. Boolean Calculator (2) ==不会考==

Continue the solution program from the preceding exercise by implementing the following procedures:

 • AND_op: Prompt the user for two hexadecimal integers. AND them together and display the result in hexadecimal.

 • OR_op: Prompt the user for two hexadecimal integers. OR them together and display the result in hexadecimal.

 • NOT_op: Prompt the user for a hexadecimal integer. NOT the integer and display the result in hexadecimal.

 • XOR_op: Prompt the user for two hexadecimal integers. Exclusive-OR them together and display the result in hexadecimal.



11. Message Encryption ==不会考==

Revise the encryption program in Section 6.3.4 in the following manner: Let the user enter an encryption key consisting of multiple characters. Use this key to encrypt and decrypt the plain-text by XORing each character of the key against a corresponding byte in the message. Repeat the key as many times as necessary until all plain-text bytes are translated. Suppose, for example, the key equals "ABXmv#7". This is how the key would align with the plain-text bytes:

| Plain text | T    | h    | i    | s    |      | i    | s    |      | a    |      | P    | l    | a    | i    | n    |      | t    | e    | x    | t    |      | m    | e    | s    | s    | a    | g    | e    | (etc.) |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------ |
| Key        | A    | B    | X    | m    | v    | #    | 7    | A    | B    | X    | m    | v    | #    | 7    | A    | B    | X    | m    | v    | #    | 7    | A    | 8    | X    | m    | v    | #    | 7    |        |

(The key repeats until it equals the length of the plain text...)

(A VideoNote for this exercise is posted on the Web site.)

---

### Solution(s):

Sol 1:

1. 数组值计数

编写一个应用程序，执行以下操作： 

(1) 用 50 个随机整数填充一个数组； 

(2) 循环数组，显示每个值，并计算负值的个数； 

(3) 循环结束后，显示计数结果。

注：Irvine32 库中的 Random32 存储过程可生成随机整数。

其实压根不用数组存，1、2、3步骤用一个循环就能全搞定：显示random值、统计负值的个数了

```assembly
INCLUDE Irvine32.inc

.data
    arr DWORD 50 DUP(?)     ; 存放 50 个随机整数
    negCount DWORD 0
    negMsg BYTE "Negative numbers count: ", 0 ; 别忘了补0 
.code
main PROC
    call Randomize
    mov ecx, 50
    mov esi, OFFSET arr
Fill:
    ; mov eax, Random32 这是错误写法，我脑抽了，会自动填入eax的
    call Random32           ; 用 Random32 产生一个 32位 随机数
    mov [esi], eax
    add esi, TYPE arr
    loop Fill

    mov ecx, 50             ; ecx 计数器重置成 50
    mov esi, OFFSET arr     ; esi 重新设置成 array 起点

ShowAndCount:
    mov eax, [esi]
    call WriteInt           ; 无论如何都要 Show
    call Crlf
    cmp eax, 0              ; 和 0 比大小
    jge Skip                ; 如果是正数，Skip而不Count
    inc negCount            ; negCount ++
Skip:
    add esi, TYPE arr       ; 无论如何都会"流"入Skip
    loop ShowAndCount

    mov edx, OFFSET negMsg  ; edx 专门用来打印字符串和字符数组 打印 prompt，一定要加 OFFSET 因为是"指针"
    call WriteString
    mov eax, negCount       
    call WriteDec           ; 打印负数数量，显示无符号十进制计数，不用WriteInt(有+ -)
    call Crlf
    exit
main ENDP
END main

```

输出很成功：

```
-1106571600
-1982322940
+685932789
+1266235999
-499931028
+493404000
+1824245554
+2135783863
+1184035892
-238944615
-265756204
-218046241
-363275894
+90127910
-1261005696
-1485785876
-830285516
+1506282424
-1192360423
-1692266191
-222301899
-311132860
-55065405
-815211710
+1456193746
-1051998977
-2140141085
+1140520692
-91951987
-705624740
+1152400924
-138167719
-566050260
+693265680
+1209122130
-351559225
+36576435
-1121033459
-1930569047
+524829650
+1721862244
-1853658487
+274670726
-1826060980
+181708485
-1524783369
-722999157
+1202785611
-296299880
-105441269
Negative numbers count: 31

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 7632)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



Sol 8:

记住让 eax 在左边就好了，太 low 了，不可能考

```assembly
INCLUDE Irvine32.inc

.data
    prompt1   BYTE "Enter first hex integer: ",0
    prompt2   BYTE "Enter second hex integer: ",0
    resultMsg BYTE "Result = 0x",0

.code

;------------------------------------------------------------------------------ 
; ReadTwoHex: 提示并读取两个 16 进制数，结果保存在 EBX(第1个), EAX(第2个)
;------------------------------------------------------------------------------
ReadTwoHex PROC
    mov edx, OFFSET prompt1
    call WriteString
    call ReadHex        ; EAX ← 第一个 hex
    mov ebx, eax

    mov edx, OFFSET prompt2
    call WriteString
    call ReadHex        ; EAX ← 第二个 hex
    ret
ReadTwoHex ENDP

;------------------------------------------------------------------------------ 
; PrintHex: 输出 EAX 中的值为十六进制，然后换行
;------------------------------------------------------------------------------
PrintHex PROC
    mov edx, OFFSET resultMsg
    call WriteString
    call WriteHex       ; 输出 EAX 为 hex
    call Crlf
    ret
PrintHex ENDP

;------------------------------------------------------------------------------ 
; AND_op: 读取两个 hex → EAX AND EBX → 打印
;------------------------------------------------------------------------------
AND_op PROC
    call ReadTwoHex

    and eax, ebx
    call PrintHex
    ret
AND_op ENDP

;------------------------------------------------------------------------------ 
; OR_op: 读取两个 hex → EAX OR EBX → 打印
;------------------------------------------------------------------------------
OR_op PROC
    call ReadTwoHex
    or  eax, ebx
    call PrintHex
    ret
OR_op ENDP

;------------------------------------------------------------------------------ 
; XOR_op: 读取两个 hex → EAX XOR EBX → 打印
;------------------------------------------------------------------------------
XOR_op PROC
    call ReadTwoHex
    xor eax, ebx
    call PrintHex
    ret
XOR_op ENDP

;------------------------------------------------------------------------------ 
; NOT_op: 读取一个 hex → NOT EAX → 打印
;------------------------------------------------------------------------------
NOT_op PROC
    mov edx, OFFSET prompt1
    call WriteString
    call ReadHex        ; EAX ← hex
    not eax             ; 按位取反
    call PrintHex
    ret
NOT_op ENDP

;------------------------------------------------------------------------------ 
; main: 演示调用
;------------------------------------------------------------------------------
main PROC
    call Clrscr
    call AND_op
    call OR_op
    call XOR_op
    call NOT_op
    exit
main ENDP

END main

```

输出:

```
Enter first hex integer: 1
Enter second hex integer: 1
Result = 0x00000001
Enter first hex integer: 0
Enter second hex integer: 0
Result = 0x00000000
Enter first hex integer: 0
Enter second hex integer: 0
Result = 0x00000000
Enter first hex integer: 1
Result = 0xFFFFFFFE

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 18056)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .                                                                                                                          
```



Sol 11:

- 用户输入一段明文；
- 用户输入密钥（任意多个字符）；
- 加密时用密钥字符对明文每个字符进行 `XOR`；
- 再次 `XOR` 即可解密；
- 支持长度判断，避免除零异常。

太难了，不可能考，做不出来

```assembly

```



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



---

### Solution(s):

Sol 1:

我断点调试过了，没问题

```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess proto, dwExitCode:dword

.data
    source WORD 1,2,3,4,5,6,7,8,9,10    ; 原始WORD数组
    target DWORD 10 DUP(?)               ; 目标DWORD数组

.code
main PROC
    mov esi, OFFSET source   ; ESI指向源数组
    mov edi, OFFSET target   ; EDI指向目标数组
    mov ecx, LENGTHOF source ; 循环计数器（10次）

copy_loop:
    movsx eax, WORD PTR [esi] ; 1. 符号扩展读取WORD到EAX
    mov [edi], eax            ; 2. 存储到DWORD数组
    add esi, 2               ; 3. 源指针+2字节（WORD大小）
    add edi, 4               ; 4. 目标指针+4字节（DWORD大小）
    loop copy_loop           ; 5. 循环直到处理完所有元素

    invoke ExitProcess, 0
main ENDP
END main
```

简洁版：

```assembly
.386
.model flat,stdcall
.stack 4096
ExitProcess proto, dwExitCode:dword

.data
    source WORD 1,2,3,4,5,6,7,8,9,10
    target DWORD 10 DUP(?)

.code
main PROC
    mov esi, OFFSET source    
    mov edi, OFFSET target
    mov ecx, LENGTHOF source    ; 今天一直在写汇编，有点头晕了，这里容易忘
copy_loop:    
    movsx eax, WORD PTR [esi]   ; 老实说，头昏昏又忘了写 movsx 和 WORD PTR，不行，睡了，用movsx而不用movzx是因为可能有负数，sx更规范
    add [edi], eax

    add esi, 2
    add edi, 4
    loop copy_loop

    invoke ExitProcess, 0
main ENDP
END main
```



Sol 3:

将 `array` 中：

- `1,2` → `2,1`
- `3,4` → `4,3`
- `5,6` → `6,5`
- ...

然后打印交换后的数组。

```assembly
.386 
.model flat,stdcall 
.stack 4096 
ExitProcess proto,dwExitCode:dword 
INCLUDE Irvine32.inc
.data 
	array DWORD 1,2,3,4,5,6,7,8,9,10,11,12
.code
main PROC
	mov esi, OFFSET array
	mov ecx, LENGTHOF array / 2
Swap_loop:	
	; 不能直接交换：xchg [esi], [esi + 4]
	mov eax, [esi]
	mov ebx, [esi + 4]
	mov [esi], ebx
	mov [esi + 4], eax

	add esi, 8
	loop Swap_loop

	INVOKE ExitProcess, 0
main ENDP
END main
```

Irvine打印

```assembly
INCLUDE Irvine32.inc
.data 
	array DWORD 1,2,3,4,5,6,7,8,9,10,11,12
.code
main PROC
	mov esi, OFFSET array
	mov ecx, LENGTHOF array / 2
Swap_loop:	
	; 不能直接交换：xchg [esi], [esi + 4]
	mov eax, [esi]
	mov ebx, [esi + 4]
	mov [esi], ebx
	mov [esi + 4], eax

	add esi, 8
	loop Swap_loop

	; === 打印交换后的数组 必须用 Irvine32.inc ===
    mov esi, OFFSET array
	mov ecx, LENGTHOF array
print_loop:
    mov eax, [esi]
    call WriteInt
    call Crlf
    add esi, 4
    loop print_loop
	INVOKE ExitProcess, 0
main ENDP
END main
```

输出：

```
+2
+1
+4
+3
+6
+5
+8
+7
+10
+9
+12
+11

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 2888)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



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

9. Probabilities and Colors ==不会考==
Write a program that randomly chooses among three different colors for displaying text on the screen. Use a loop to display 20 lines of text, each with a randomly chosen color. The probabilities for each color are to be as follows: white = 30%, blue = 10%, green = 60%. Hint: Generate a random integer between 0 and 9. If the resulting integer is in the range 0 to 2, choose white. If the integer equals 3, choose blue. If the integer is in the range 4 to 9, choose green. (A VideoNote for this exercise is posted on the Web site.)

10. Print Fibonacci until Overflow
Write a program that calculates and displays the Fibonacci number sequence {1, 1, 2, 3, 5, 8, 13, ...}, stopping only when the Carry flag is set. Display each unsigned decimal integer value on a separate line.

---

### Solution(s):

Sol 2:

```assembly
.386
.model flat,stdcall
.stack 4096
include Irvine32.inc

.data
array DWORD 10, 60, 20, 33, 72, 89, 45, 65, 72, 18
sample DWORD 50
ArraySize DWORD LENGTHOF array
index DWORD 0
sum DWORD 0

.code
main PROC
    mov edi, 0              ; edi = index
    mov sum, 0              ; sum = 0

    .WHILE edi < ArraySize  ; 没办法，必须至少有一方是 Reg
        mov eax, array[edi * TYPE array] ; array[index * 4]
        .IF eax < sample    ; 没办法，必须至少有一方是 Reg
            add sum, eax
        .ENDIF
        inc edi             ; edi++ 就是 index++
    .ENDW

    mov eax, sum
    call WriteInt           ; 输出 sum
    call Crlf
    exit
main ENDP
END main
```

输出:

```
+126

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 18640)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```

真正考试能用的写法（输出+126一模一样）：

```assembly
.386
.model flat,stdcall
.stack 4096
include Irvine32.inc

.data
    array DWORD 10, 60, 20, 33, 72, 89, 45, 65, 72, 18
    sample DWORD 50
    ArraySize DWORD LENGTHOF array
    index DWORD 0
    sum DWORD 0

.code
main PROC
    mov edi, 0              ; edi = index
    mov sum, 0              ; sum = 0
L1:
    cmp edi, ArraySize
    jge quit
    mov eax, array[edi * TYPE array]
    cmp eax, sample
    jge SkipAddition
    add sum, eax
SkipAddition:
    inc edi
    jmp L1
quit:
    mov eax, sum
    call WriteInt           ; 输出 sum
    call Crlf
    exit
main ENDP
END main
```



Sol 3:

```assembly
.386
.model flat,stdcall
.stack 4096
INCLUDE Irvine32.inc

.data
score DWORD ?

.code
main PROC
    call Clrscr
    ; 提示输入
    mov edx, OFFSET prompt
    call WriteString
    call ReadInt           ; 用户输入的整数 -> EAX
    mov score, eax         ; 保存到 score

    ; 判定成绩
    mov eax, score
    .IF eax >= 90 && eax <= 100
        mov edx, OFFSET gradeA
    .ELSEIF eax >= 80
        mov edx, OFFSET gradeB
    .ELSEIF eax >= 70
        mov edx, OFFSET gradeC
    .ELSEIF eax >= 60
        mov edx, OFFSET gradeD
    .ELSE
        mov edx, OFFSET gradeF
    .ENDIF

    call WriteString
    call Crlf

    exit
main ENDP

.data
prompt  BYTE "Enter your test score (0~100): ", 0
gradeA  BYTE "Your grade: A", 0
gradeB  BYTE "Your grade: B", 0
gradeC  BYTE "Your grade: C", 0
gradeD  BYTE "Your grade: D", 0
gradeF  BYTE "Your grade: F", 0

END main

```

输出:

```
Enter your test score (0~100): 75
Your grade: C
```

不用伪指令，只用 **纯汇编的条件跳转指令**

==挺有意思的，和 switch case 很像，我不是很熟悉==

用法 EDX：字符串、字符数组；EDI：数组

==用 **WriteString** 一定是强绑定 **EDX** 的，换成 EDI 就是无法正常运行==

```assembly
INCLUDE Irvine32.inc

.data
score DWORD ?
gradeA BYTE "Your grade: A", 0
gradeB BYTE "Your grade: B", 0
gradeC BYTE "Your grade: C", 0
gradeD BYTE "Your grade: D", 0
gradeF BYTE "Your grade: F", 0
prompt BYTE "Enter your test score (0~100): ", 0

.code
main PROC
    ; 提示并读取成绩
    mov edx, OFFSET prompt ; EDX：字符串、字符数组；EDI：数组
    call WriteString
    call ReadInt
    mov score, eax     ; 保存输入分数

    ; 分数判断
    mov eax, score
    cmp eax, 90
    jge grade_A
    cmp eax, 80
    jge grade_B
    cmp eax, 70
    jge grade_C
    cmp eax, 60
    jge grade_D

    ; 小于 60 -> F
    mov edx, OFFSET gradeF
    jmp show

grade_A:
    mov edx, OFFSET gradeA
    jmp show
grade_B:
    mov edx, OFFSET gradeB
    jmp show
grade_C:
    mov edx, OFFSET gradeC
    jmp show
grade_D:
    mov edx, OFFSET gradeD

show:
    call WriteString
    call Crlf
    exit
main ENDP
END main
```



Sol 4:

```assembly
INCLUDE Irvine32.inc

.data
    score       DWORD ?
    count       DWORD 0

    prompt      BYTE "Enter your test score (0~100) or -1 to quit: ", 0
    errRange    BYTE "  Error: score must be 0~100 (or -1 to quit)", 0

    gradeA      BYTE "  Your grade: A", 0
    gradeB      BYTE "  Your grade: B", 0
    gradeC      BYTE "  Your grade: C", 0
    gradeD      BYTE "  Your grade: D", 0
    gradeF      BYTE "  Your grade: F", 0

    totalMsg    BYTE "Number of valid scores entered: ", 0

.code
main PROC
    call Clrscr

input_loop:
    mov edx, OFFSET prompt
    call WriteString
    call ReadInt
    mov score, eax

    cmp score, -1
    je show_total

    cmp score, 0
    jl out_of_range
    cmp score, 100
    jg out_of_range

    inc count

    mov eax, score
    cmp eax, 90
    jge grade_A
    cmp eax, 80
    jge grade_B
    cmp eax, 70
    jge grade_C
    cmp eax, 60
    jge grade_D

    mov edx, OFFSET gradeF
    jmp show

grade_A:
    mov edx, OFFSET gradeA
    jmp show
grade_B:
    mov edx, OFFSET gradeB
    jmp show
grade_C:
    mov edx, OFFSET gradeC
    jmp show
grade_D:
    mov edx, OFFSET gradeD

show:
    call WriteString
    call Crlf
    jmp input_loop

out_of_range:
    mov edx, OFFSET errRange
    call WriteString
    call Crlf
    jmp input_loop

show_total:
    call Crlf
    mov edx, OFFSET totalMsg
    call WriteString
    mov eax, count
    call WriteInt
    call Crlf
    exit
main ENDP

END main

```

输出:

```
Enter your test score (0~100) or -1 to quit: 123
  Error: score must be 0~100 (or -1 to quit)
Enter your test score (0~100) or -1 to quit: 12
  Your grade: F
Enter your test score (0~100) or -1 to quit: 3
  Your grade: F
Enter your test score (0~100) or -1 to quit: 88
  Your grade: B
Enter your test score (0~100) or -1 to quit: 9
  Your grade: F
Enter your test score (0~100) or -1 to quit: 0
  Your grade: F
Enter your test score (0~100) or -1 to quit: 100
  Your grade: A
Enter your test score (0~100) or -1 to quit: 00
  Your grade: F
Enter your test score (0~100) or -1 to quit: 99
  Your grade: A
Enter your test score (0~100) or -1 to quit: 546
  Error: score must be 0~100 (or -1 to quit)
Enter your test score (0~100) or -1 to quit: 56
  Your grade: F
Enter your test score (0~100) or -1 to quit: 75
  Your grade: C
Enter your test score (0~100) or -1 to quit: 95
  Your grade: A
Enter your test score (0~100) or -1 to quit: 74
  Your grade: C
Enter your test score (0~100) or -1 to quit: 65
  Your grade: D
Enter your test score (0~100) or -1 to quit: ^A
 <invalid integer>
  Your grade: F
Enter your test score (0~100) or -1 to quit: -1

Number of valid scores entered: +14

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 17628)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



Sol 9:

概率与颜色
编写一个程序，在屏幕上显示文字时随机选择三种不同的颜色。使用循环显示 20 行文字，每行随机选择一种颜色。每种颜色的概率如下：白色 = 30%，蓝色 = 10%，绿色 = 60%。

==提示：随机生成一个 0 到 9 之间的整数。如果生成的整数在 0 到 2 之间，则选择白色。如果整数等于 3，则选择蓝色。如果整数在 4 到 9 之间，选择绿色。==



🎯 说明：

- 使用 `RandomRange` 限制随机数在 0~9。
- 判断：
    - 0~2：白色（30%）
    - 3：蓝色（10%）
    - 4~9：绿色（60%）
- 用 `SetTextColor` 设置输出颜色，前景为文字色，背景为浅灰。

```assembly
INCLUDE Irvine32.inc

.data
    whiteText   BYTE "This is WHITE text", 0
    blueText    BYTE "This is BLUE text", 0
    greenText   BYTE "This is GREEN text", 0

.code
main PROC
    call Randomize         ; 初始化随机数种子

    mov ecx, 20            ; 显示 20 行

displayLoop:
    ; 生成 0~9 随机数
    mov eax, 10
    call RandomRange       ; eax = 0 ~ 9

    cmp eax, 3
    je blue_color          ; eax = 3 → 蓝色 (10%)

    cmp eax, 3
    jl white_color         ; eax = 0~2 → 白色 (30%)

    ; 否则 → 绿色 (60%)
    jmp green_color

white_color:
    mov eax, white + (black * 16)     ; 前景白，背景黑
    call SetTextColor
    mov edx, OFFSET whiteText
    call WriteString
    call Crlf
    loop displayLoop
    jmp done

blue_color:
    mov eax, blue + (black * 16)      ; 前景蓝，背景黑
    call SetTextColor
    mov edx, OFFSET blueText
    call WriteString
    call Crlf
    loop displayLoop
    jmp done

green_color:
    mov eax, green + (black * 16)     ; 前景绿，背景黑
    call SetTextColor
    mov edx, OFFSET greenText
    call WriteString
    call Crlf
    loop displayLoop
    jmp done

done:
    mov eax, lightGray + (black * 16) ; 恢复默认颜色
    call SetTextColor
    exit
main ENDP

END main

```

输出:

不给你看颜色了，自己试试几次

```
This is WHITE text
This is GREEN text
This is GREEN text
This is GREEN text
This is GREEN text
This is WHITE text
This is BLUE text
This is GREEN text
This is BLUE text
This is GREEN text
This is GREEN text
This is WHITE text
This is GREEN text
This is WHITE text
This is WHITE text
This is GREEN text
This is GREEN text
This is GREEN text
This is GREEN text
This is GREEN text

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 17364)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



Sol 10:

==有必要会Fib()==

```assembly
INCLUDE Irvine32.inc

.data
	doneMsg BYTE "Overflow detected. Fibonacci sequence ended.", 0
.code
main PROC
	mov eax, 1         ; Fib(1)
    mov ebx, 1         ; Fib(2)
	call WriteInt
	call Crlf
	call WriteInt
	call Crlf

fib_loop:
	add eax, ebx       ; eax = eax + ebx (计算下一个数)
    jc overflow_exit   ; 如果溢出，跳出循环 jump if carry flag was set
	xchg eax, ebx
	call WriteInt
	call Crlf
	jmp fib_loop

overflow_exit:
	call Crlf
	mov edx, OFFSET doneMsg
	call WriteString
	call Crlf
	exit
main ENDP
END main
```

输出:

```
+1
+1
+1
+2
+3
+5
+8
+13
+21
+34
+55
+89
+144
+233
+377
+610
+987
+1597
+2584
+4181
+6765
+10946
+17711
+28657
+46368
+75025
+121393
+196418
+317811
+514229
+832040
+1346269
+2178309
+3524578
+5702887
+9227465
+14930352
+24157817
+39088169
+63245986
+102334155
+165580141
+267914296
+433494437
+701408733
+1134903170
+1836311903

Overflow detected. Fibonacci sequence ended.

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 8236)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



---

## Practice 6

Programming Exercises P267

3, 4, 6

---

3. ShowFileTime  ==不会考==
The time stamp of a MS-DOS file directory entry uses bits 0 through 4 for the number of 2-second increments, bits 5 through 10 for the minutes, and bits 11 through 15 for the hours (24-hour clock). For example, the following binary value indicates a time of 02:16:14, in hh:mm:ss format:  

```
00010 010000 00111
```

Write a procedure named ShowFileTime that receives a binary file time value in the AX register and displays the time in hh:mm:ss format.

4. Encryption Using Rotate Operations  ==不会考==
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

Sol 3:

==会考我吃，看看下面的代码，真吓人==

```assembly
.386
.model flat,stdcall
.stack 4096
INCLUDE Irvine32.inc

.data
    ; 无需额外数据段内容

.code

; ----------------------------------------
; Print2DigitDec: 输出两位数字 (00–99)
; 输入：EAX = 数值 (0–99)
; ----------------------------------------
Print2DigitDec PROC
    push edx
    push ecx

    mov edx, 0
    mov ecx, 10
    div ecx             ; EAX / 10, EAX=十位, EDX=个位

    add al, '0'
    call WriteChar

    mov al, dl
    add al, '0'
    call WriteChar

    pop ecx
    pop edx
    ret
Print2DigitDec ENDP


; ----------------------------------------
; ShowFileTime: 解析 DOS 文件时间格式并输出为 hh:mm:ss
; 输入：AX = DOS 文件时间 (bits: 15-11 = hour, 10-5 = min, 4-0 = sec/2)
; ----------------------------------------
ShowFileTime PROC
    movzx ecx, ax
    shr   ecx, 11        ; hours

    movzx ebx, ax
    shr   ebx, 5
    and   ebx, 3Fh       ; minutes

    movzx edx, ax
    and   edx, 1Fh
    shl   edx, 1         ; seconds = low 5 bits * 2

    mov eax, ecx
    call Print2DigitDec
    mov al, ':'
    call WriteChar

    mov eax, ebx
    call Print2DigitDec
    mov al, ':'
    call WriteChar

    mov eax, edx
    call Print2DigitDec

    ret
ShowFileTime ENDP


; ----------------------------------------
; main 函数，调用测试
; ----------------------------------------
main PROC
    call Clrscr

    mov ax, 1207h   ; 02:16:14 正确的 DOS 时间编码，
    call ShowFileTime

    call Crlf
    exit
main ENDP

END main

```

输出:

```
02:16:14

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 11556)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . . 
```



Sol 4:

懒得写，不会考

Sol 6:

好题，但90行，不会期末考

```assembly
INCLUDE Irvine32.inc

.data
    msg1 BYTE "GCD(48, 18) = ", 0
    msg2 BYTE "GCD(100, 75) = ", 0
    msg3 BYTE "GCD(-42, 56) = ", 0

.code

; ----------------------------------------
; Abs: 获取 EAX 的绝对值
; 输出: EAX = 绝对值
; ----------------------------------------
Abs PROC
    test eax, eax      ; 判断是否为负
    jns notNegative    ; 如果是正数，跳过
    neg eax            ; 取相反数
notNegative:
    ret
Abs ENDP

; ----------------------------------------
; GCD PROC
; 输入: EAX = x, EBX = y
; 输出: EAX = GCD(x, y)
; ----------------------------------------
GCD PROC
    ; 取 x 的绝对值
    call Abs
    mov ecx, eax    ; ecx = abs(x)

    mov eax, ebx    ; eax = y
    call Abs
    mov ebx, eax    ; ebx = abs(y)

    mov eax, ecx    ; eax = abs(x)

L1:
    cmp ebx, 0
    je Done
    mov edx, 0
    div ebx         ; eax / ebx, 余数在 edx
    mov eax, ebx
    mov ebx, edx
    jmp L1

Done:
    ret
GCD ENDP

; ----------------------------------------
; 主程序 main
; ----------------------------------------
main PROC
    call Clrscr

    ; 示例 1: GCD(48, 18)
    mov edx, OFFSET msg1
    call WriteString

    mov eax, 48
    mov ebx, 18
    call GCD
    call WriteInt
    call Crlf

    ; 示例 2: GCD(100, 75)
    mov edx, OFFSET msg2
    call WriteString

    mov eax, 100
    mov ebx, 75
    call GCD
    call WriteInt
    call Crlf

    ; 示例 3: GCD(-42, 56)
    mov edx, OFFSET msg3
    call WriteString

    mov eax, -42
    mov ebx, 56
    call GCD
    call WriteInt
    call Crlf

    exit
main ENDP

END main

```

输出:

```
GCD(48, 18) = +6
GCD(100, 75) = +25
GCD(-42, 56) = +14

C:\Users\David\source\repos\ProjectTest\Debug\ProjectTest.exe (进程 5496)已退出，代码为 0 (0x0)。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



