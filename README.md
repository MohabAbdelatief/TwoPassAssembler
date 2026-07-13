# Two-Pass Assembler (Swift Edition)

> A two-pass assembler that translates assembly source code into machine code, written entirely in Swift.

![Swift](https://img.shields.io/badge/Swift-5-orange) ![Platform](https://img.shields.io/badge/platform-macOS-lightgrey)

## Overview

This project implements a classic **two-pass assembler** — the same architecture used by real assemblers to turn human-readable assembly into machine code. Rather than translating in a single scan, it processes the source **twice**: the first pass discovers where everything lives in memory, and the second pass emits the final machine code. Splitting the work this way cleanly solves the *forward-reference problem* (using a label before it's defined).

Built as a systems-programming project to explore how assemblers, symbol tables, and address resolution actually work under the hood.

## How It Works

```
Assembly Source ──► [ PASS 1 ] ──► Symbol Table + Location Counter
                                          │
                                          ▼
                    [ PASS 2 ] ──► Machine Code / Object Program
```

**Pass 1 — Analysis**
- Scans the source line by line, tracking addresses with a **location counter (LOCCTR)**
- Assigns an address to every label and records it in the **symbol table (SYMTAB)**
- Processes directives that affect addressing and computes the total program length
- Detects errors like duplicate labels
- *No machine code is generated yet* — this pass only figures out the memory layout

**Pass 2 — Synthesis**
- Scans the source a second time, now with a complete symbol table
- Translates each mnemonic to its opcode via the **operation table (OPTAB)**
- Resolves symbol/label references to their addresses from SYMTAB
- Assembles the final machine code and reports errors (undefined symbols, invalid opcodes)

## Features

- Symbol-table generation and label resolution
- Location-counter management across the program
- Opcode translation via an operation table
- Handling of assembler directives: 「list yours — e.g. START, END, WORD, RESW, RESB, BYTE」
- Error detection for 「e.g. undefined symbols, duplicate labels, invalid opcodes」

## Target Machine / Instruction Set

「Describe the ISA your assembler targets — e.g. SIC/SIC-XE, or a custom instruction set. List the supported instructions and their formats. This is the key section that tells a reader exactly what your assembler understands.」

## Getting Started

### Requirements
- Swift 「6.1」 (Xcode 「16.4)

### Build & Run
```bash
「exact steps — e.g.
git clone https://github.com/MohabAbdelatief/Two-Pass-Assembler.git
cd Two-Pass-Assembler
swift run   # or: open in Xcode and Run
」
```

## Project Structure

```
Two-Pass-Assembler/
├── TwoPassAssemberV1/     
├── Project Documentation.pdf
└── README.md
```

## Concepts Demonstrated

- Two-pass assembly architecture and why it solves forward references
- Symbol tables, operation tables, and location counters
- Lexical parsing of assembly source
- Clean separation of concerns between analysis (Pass 1) and synthesis (Pass 2)

## Documentation

A full design write-up is available in [`Project Documentation.pdf`](./Project%20Documentation.pdf).
