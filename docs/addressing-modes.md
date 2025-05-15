# Addressing modes

| Code | Addressing Mode                          |
|------|------------------------------------------|
| 0    | [abs](#absolute)                         |
| 1    | [abs+X](#absolute-x-indexed)             |
| 2    | [abs+Y](#absolute-y-indexed)             |
| 3    | [zp](#zeropage)                          |
| 4    | [zp+X](#zeropage-x-indexed)              |
| 5    | [zp+Y](#zeropage-y-indexed)              |
| 6    | [immediate](#immediate)                  |
| 7    | [(zp+X)](#indirect-zp-x)                 |
| 8    | [(zp+Y)](#indirect-zp-y)                 |
| 9    | [(zp)+X](#indirect-zeropage-x-indexed)   |
| 10   | [(zp)+Y](#indirect-zeropage-y-indexed)   |
| 11   | [(zp)](#indirect-zeropage)               |
| 12   | illegal                                  |
| 13   | [sp+i](#stack-pointer-immediate-index)   |
| 14   | [sp+X](#stack-pointer-x-indexed)         |
| 15   | [sp+Y](#stack-pointer-y-indexed)         |

## Absolute

2 bytes

The absolute addressing mode specifies a full 16-bit address in the instruction, allowing access to any memory location. The operand is the address itself (LE).

---

## Absolute, X indexed

2 bytes

Adds the value of the X register to a 16-bit base address specified in the instruction. Useful for accessing arrays or tables.

---

## Absolute, Y indexed

2 bytes

Adds the value of the Y register to a 16-bit base address specified in the instruction.

---

## Zeropage

1 byte

Uses an 8-bit address, allowing access to the first 256 bytes of memory (zero page). Instructions are shorter and faster.

---

## Zeropage, X indexed

1 byte

Adds the value of the X register to an 8-bit zero page address. Enables efficient indexed access within the zero page.

---

## Zeropage, Y indexed

1 byte

Adds the value of the Y register to an 8-bit zero page address.

---

## Immediate

1 byte

The operand is a constant value included directly in the instruction, not a memory address.

---

<h2 id="indirect-zp-x">Indirect (Zeropage, X indexed)</h2>

1 byte

Adds X to a zero page address, then uses the resulting address to fetch a 16-bit pointer from zero page. The pointer is then used as the effective address.

---

<h2 id="indirect-zp-y">Indirect (Zeropage, Y indexed)</h2>

1 byte

Fetches a 16-bit pointer from a zero page address, then adds Y to the pointer to form the effective address.

---

## Indirect (Zeropage), X indexed

1 byte

Uses a zero page address, adds X to it, and then uses the resulting address to fetch a 16-bit pointer from zero page. The pointer is the effective address.

---

## Indirect (Zeropage), Y indexed

1 byte

Fetches a 16-bit pointer from a zero page address, then adds Y to the pointer to form the effective address.

---

## Indirect (Zeropage)

1 byte

Fetches a 16-bit pointer from a zero page address. The pointer is used as the effective address.

---

## Stack Pointer, immediate index

0 bytes

Adds an immediate value to the stack pointer (SP) to calculate the effective address.

---

## Stack Pointer, X indexed

0 bytes

Adds the value of the X register to the stack pointer (SP) to form the effective address.

---

## Stack Pointer, Y indexed

0 bytes

Adds the value of the Y register to the stack pointer (SP) to form the effective address.
