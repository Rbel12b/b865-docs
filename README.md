# B865: 8-bit Breadboard Computer

The **B865** is a homebrew 8-bit computer built on a breadboard, featuring a custom CPU, 64KB of addressable memory, and 7 8-bit general-purpose registers (2 can be used for indexing), 8-bit stack pointer, 16-bit program counter.

## Specifications

- **Data Bus**: 8-bit
- **Address Bus**: 16-bit
- **Registers**:
  - 8-bit: A, SP, X, Y, R0, R1, R2, R3
  - 16-bit: PC
  - Status register(S): (3-bit: Carry, Zero, Negative)
- **Memory**: 64KB (32k ROM + 32k RAM)

## Instruction Set

Full [Instruction Set Documentation](instruction-set.md)

## Memory Map

| Address Range | Usage       |
|---------------|-------------|
| 0x0000-0x7FFF | RAM         |
| 0x8000-0xFFFF | ROM         |

Pheripherials registers in RAM:

| Address Range | Pheripherial |
|---------------|--------------|
| 0x0200-0x0207 | GPIO         |

## How It Works

The CPU is built using 74-series logic, with a microcoded control unit stored in EEPROM. It executes instructions from ROM, with RAM mapped to lower memory for stack and variable storage.

## License

[CC BY_SA 4.0](LICENSE)
