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
    **Data Definitions**
    Write a program that contains a definition of each data type listed in Table 3-2 in Section 3.4.
    Initialize each variable to a value that is consistent with its data type.

  **Symbolic Integer Constants**
  Write a program that defines symbolic constants for all of the days of the week. Create an array variable that uses the symbols as initializers.

  **Symbolic Text Constants**
  Write a program that defines symbolic names for several string literals (characters between quotes). Use each symbolic name in a variable definition.

---

### Solution(s):



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

5. Reverse an Array  
Use a loop with indirect or indexed addressing to reverse the elements of an integer array in place. Do not copy the elements to any other array. Use the SIZEOF, TYPE, and LENGTHOF operators to make the program as flexible as possible if the array size and type should be changed in the future. Optionally, you may display the modified array by calling the DumpMem method from the Irvine32 library. See Chapter 5 for details. (A VideoNote for this exercise is posted on the Web site.)

6. Fibonacci Numbers  
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

---

### Solution(s):



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
