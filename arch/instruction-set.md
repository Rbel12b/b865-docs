# Instruction set

## Arguments

| Argument Type | Meaning          |
|---------------|------------------|
| R             | Register index [A, X, Y, SP, R0, R1, R2, R3] |
| A             | [Addressing mode](addressing-modes) (0-15)|
| flags         | Condition Flags  |

## Instruction List

| Mnemonic                                              | binary   |
|-------------------------------------------------------|----------|
| [RST](#rst-reset)                                     | 00000000 |
| [MOV R,R](#mov-rr-move-register-to-register)          | 00000001 |
| [MOV R,A](#mov-ra-move-register-to-address)           | 00000010 |
| [MOV A,A](#mov-aa-move-address-to-address)            | 00000011 |
| [MOV A,R](#mov-ar-move-address-to-register)           | 00000100 |
| [BRH flags, A](#brh-flags-a-branch-on-flags)          | 00000101 |
| [DEX](#dex-decrement-x)                               | 00000110 |
| illegal                                               | 00000111 |
| [ADD R, A](#add-r-a-add-register-and-address)         | 00001000 |
| [SUB R, A](#sub-r-a-subtract-address-from-register)   | 00001001 |
| [INX](#inx-increment-x)                               | 00001010 |
| [NOP](#nop-no-operation)                              | 00001011 |
| [RSH R](#rsh-r-right-shift-register)                  | 00001100 |
| [LSH R](#lsh-r-left-shift-register)                   | 00001101 |
| [ROR R](#ror-r-rotate-right-register)                 | 00001110 |
| [ROL R](#rol-r-rotate-left-register)                  | 00001111 |
| [ADD R, R](#add-r-r-add-register-to-register)         | 00010000 |
| [XNOR R, R](#xnor-r-r-xnor-registers)                 | 00010001 |
| [SUB R, R](#sub-r-r-subtract-register-from-register)  | 00010010 |
| [XOR R, R](#xor-r-r-xor-registers)                    | 00010011 |
| [OR R, R](#or-r-r-or-registers)                       | 00010100 |
| [NOR R, R](#nor-r-r-nor-registers)                    | 00010101 |
| [NAND R, R](#nand-r-r-nand-registers)                 | 00010110 |
| [AND R, R](#and-r-r-and-registers)                    | 00010111 |
| [ADC R, R](#adc-r-r-add-with-carry)                   | 00011000 |
| [PHR R](#phr-r-push-register)                         | 00011001 |
| [SBC R, R](#sbc-r-r-subtract-with-carry)              | 00011010 |
| [PLR R](#plr-r-pull-register)                         | 00011011 |
| [JSR](#jsr-jump-to-subroutine)                        | 00011100 |
| [CMP R,R](#cmp-rr-compare-registers)                  | 00011101 |
| [DEC R](#dec-r-decrement-register)                    | 00011110 |
| [INC R](#inc-r-increment-register)                    | 00011111 |
| [JMP A](#jmp-a-jump-to-address)                       | 00100000 |
| reserved                                              | 00100001 |
| reserved                                              | 00100010 |
| [CLC](#clc-clear-carry)                               | 00100011 |
| reserved                                              | 00100100 |
| reserved                                              | 00100101 |
| [STC](#stc-set-carry)                                 | 00100110 |
| illegal                                               | 00100111 |
| reserved                                              | 00101000 |
| reserved                                              | 00101001 |
| illegal                                               | 00101010 |
| [HLT](#hlt-halt)                                      | 00101011 |
| reserved                                              | 00101100 |
| reserved                                              | 00101101 |
| reserved                                              | 00101110 |
| reserved                                              | 00101111 |
| reserved                                              | 00110000 |
| reserved                                              | 00110001 |
| reserved                                              | 00110010 |
| reserved                                              | 00110011 |
| reserved                                              | 00110100 |
| reserved                                              | 00110101 |
| reserved                                              | 00110110 |
| reserved                                              | 00110111 |
| reserved                                              | 00111000 |
| reserved                                              | 00111001 |
| reserved                                              | 00111010 |
| reserved                                              | 00111011 |
| [RTS](#rts-return-from-subroutine)                    | 00111100 |
| reserved                                              | 00111101 |
| reserved                                              | 00111110 |
| reserved                                              | 00111111 |

## Instruction descriptions

Each instruction in the instruction set is documented with the following structure:

1. **Instruction Name and Description**: The name of the instruction is provided as a heading, followed by a brief description of its purpose and functionality.

2. **Arguments**: If the instruction takes arguments, they are listed and described. For example, the source and destination registers or memory addresses.

3. **C Representation**: A snippet of C code is provided to represent the behavior of the instruction. This helps in understanding how the instruction operates at a low level. `Memory` is a 64kB array (65536 bytes), represents the full memory region (0x0000-0xFFFF). The registers are represented with their name: `PC`, `ACC`, `X`, `Y`, `SP`, `R0`, `R1`, `R2`, `R3`. The `resolveAddress` function represents the addresssing mode logic, see [Addressing modes](addressing-modes)

4. **Instruction Encoding**: The encoding of the instruction in memory is detailed in a table. This includes the byte values and how arguments are encoded into the instruction. Each [addressing mode](addressing-modes) has a differt size in bytes after the instruction, so byte numbers in the instruction descriptions may not be correct.

### RST (Reset)

Resets the processor to its initial state. Before executing this instruction the X register must be set to zero.

Size: 1 byte

```c
PC = Memory[(X - 1) << 8 + (X - 2)] << 8 + Memory[(X - 1) << 8 + (X - 1)];
X -= 1;
```

So if X is 0 PC will be set to the reset vector at 0xFFFE - 0xFFFF

| Instruction | Byte 0 |
|-------------|--------|
| RST         | 0x00   |

### MOV R,R (Move Register to Register)

Transfers data from one register to another.

Size: 2 bytes

Arguments:

 1. `Arg0` = Destinarion register index
 2. `Arg1` = Source register index

```c
Regs[Arg0] = Regs[Arg1];
```

| Instruction | Byte 0                | Byte 1 |
|-------------|-----------------------|--------|
| MOV R, R    | `0x01 \| (Arg0 << 5)` | `Arg1` |

### MOV R,A (Move Register to Address)

Moves data from a register to a specified memory address.

Size: 2-4 bytes depending on the addressing mode

Arguments:

 1. `Arg0` = Destinarion register index
 2. `Arg1` = Addressing mode for source byte

```c
Regs[Arg0] = *resolveAddress(Arg1);
```

| Instruction | Byte 0                | Byte 1 | Byte 2... |
|-------------|-----------------------|--------|-----------|
| MOV R, A    | `0x02 \| (Arg0 << 5)` | `Arg1` | Arguments for addressing mode |

### MOV A,A (Move Address to Address)

Transfers data between two memory addresses.

Size: 2-6 bytes depending on the addressing modes

Arguments:

 1. `Arg0` = Addressing mode for destination
 2. `Arg1` = Addressing mode for source byte

```c
*resolveAddress(Arg0) = *resolveAddress(Arg1);
```

| Instruction | Byte 0 | Byte 1              | Byte 2... |
|-------------|--------|---------------------|-----------|
| MOV R, A    | `0x03` | `Arg1 \| Arg0 << 5` | Arguments for addressing modes (`Arg0` first) |

### MOV A,R (Move Address to Register)

Moves data from a memory address to a register.

Size: 2-4 bytes depending on the addressing mode

Arguments:

 1. `Arg0` = Addressing mode for destination
 2. `Arg1` = Source register index

```c
*resolveAddress(Arg0) = Regs[Arg1];
```

| Instruction | Byte 0                | Byte 1 | Byte 2... |
|-------------|-----------------------|--------|-----------|
| MOV R, A    | `0x04 \| (Arg1 << 5)` | `Arg0` | Arguments for addressing mode |

### BRH flags, A (Branch on Flags)

Branches to a specified address based on condition flags. Only 1 flag should be set, if more are set it work as an or operation (if eighter is true it will branch)

Size: 3 bytes

Arguments:

 1. `Arg0` = conditon flags, compared with the Status register = `Carry | Zero << 1 | Negative << 2`
 2. `Arg1` = An absolute address, where it will jump to if the conditions are true. (2 bytes)

```c
if (S & Arg0) {
    PC = Arg1;
}
```

| Instruction  | Byte 0                | Byte 1-2            |
|--------------|-----------------------|---------------------|
| BRH flags, A | `0x05 \| (Arg0 << 5)` | `Arg1` in LE format |

### DEX (Decrement X)

Decrements the X index register by one.

Size: 1 byte

```c
X += 1;
```

| Instruction  | Byte 0 |
|--------------|--------|
| DEX          | `0x06` |

### ADD R, A (Add Register and Address)

Adds the value at a memory address to a register.

Size: 2-4 bytes depending on the addressing mode

Arguments:

 1. `Arg0` = register index
 2. `Arg1` = Addressing mode

```c
Regs[Arg0] += *resolveAddress(Arg1);
```

| Instruction  | Byte 0                | Byte 1 | Byte 2...|
|--------------|-----------------------|--------|----------|
| Add R, A     | `0x08 \| (Arg0 << 5)` | `Arg1` | Arguments for addressing mode |

### SUB R, A (Subtract Address from Register)

Subtracts the value at a memory address from a register.

Size: 2-4 bytes depending on the addressing mode

Arguments:

 1. `Arg0` = register index
 2. `Arg1` = Addressing mode

```c
Regs[Arg0] -= *resolveAddress(Arg1);
```

| Instruction  | Byte 0                | Byte 1 | Byte 2...|
|--------------|-----------------------|--------|----------|
| Add R, A     | `0x09 \| (Arg0 << 5)` | `Arg1` | Arguments for addressing mode |

### INX (Increment X)

Increments the X register by one.

### NOP (No Operation)

Performs no operation and advances to the next instruction.

### RSH R (Right Shift Register)

Shifts the bits in a register to the right.

### LSH R (Left Shift Register)

Shifts the bits in a register to the left.

### ROR R (Rotate Right Register)

Rotates the bits in a register to the right.

### ROL R (Rotate Left Register)

Rotates the bits in a register to the left.

### ADD R, R (Add Register to Register)

Adds the values of two registers.

### XNOR R, R (XNOR Registers)

Performs a bitwise XNOR operation on two registers.

### SUB R, R (Subtract Register from Register)

Subtracts the value of one register from another.

### XOR R, R (XOR Registers)

Performs a bitwise XOR operation on two registers.

### OR R, R (OR Registers)

Performs a bitwise OR operation on two registers.

### NOR R, R (NOR Registers)

Performs a bitwise NOR operation on two registers.

### NAND R, R (NAND Registers)

Performs a bitwise NAND operation on two registers.

### AND R, R (AND Registers)

Performs a bitwise AND operation on two registers.

### ADC R, R (Add with Carry)

Adds two registers with a carry bit.

### PHR R (Push Register)

Pushes the value of a register onto the stack.

### SBC R, R (Subtract with Carry)

Subtracts one register from another with a carry bit.

### PLR R (Pull Register)

Pulls a value from the stack into a register.

### JSR (Jump to Subroutine)

Jumps to a subroutine and saves the return address.

### CMP R,R (Compare Registers)

Compares the values of two registers.

### DEC R (Decrement Register)

Decrements the value of a register by one.

### INC R (Increment Register)

Increments the value of a register by one.

### JMP A (Jump to Address)

Jumps to a specified memory address.

### CLC (Clear Carry)

Clears the carry flag.

### STC (Set Carry)

Sets the carry flag.

### HLT (Halt)

Halts the processor.

### RTS (Return from Subroutine)

Returns from a subroutine to the calling address.
