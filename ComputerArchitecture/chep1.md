# 01.Introduction

## The Computer Revolution

- Progress in computer technology
  - Underpinned by Moore’s Law
- Makes novel applications feasible
  - Computers in automobiles
  - Cell phones
  - Human genome project
  - World Wide Web
  - Search Engines
- Computers are pervasive

## Classes of Computers

- Personal Mobile Device(PMD)
  - e.g. smartphones, tablet computers
  - Emphasis on energy efficiency and real-time
- Desktop Computer
  - Emphasis on price-performance
- Servers
  - Emphasis on availability, scalability, throughput
- Clusters / Warehouse Scale Computers
  - Used for “Software as a Service(SaaS)”
  - Emphasis on availability and price-performance
- Embedded Computers
  - Emphasis: price

## The PostPC Era

![The PostPC Era](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1587971566/notepad/2020-04-27_145700_a34ydu.webp)

- Personal Mobile Device(PMD)
  - BaIery operated
  - Connects to the Internet
  - Hundreds ofdollars
  - Smart phones, tablets, lectronicglasses
- Cloud compucng
  - Warehouse Scale Computers(WSC)
  - Sogware as a Service(SaaS)
  - Porcon of sogware run on a PMD and a porcon run in the Cloud
  - Amazon and Google

## What is Computer Architecture

![What is Computer Architecture](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1587971566/notepad/2020-04-27_150521_xqubgz.webp)

Application <-> Physics

二者之间的巨大差距很难通过一步来连接

> In its broadest definition, computer architecture is the design of the abstraction/implementation layers that allow us to execute information processing
applications efficiently using manufacturing technologies

## Abstractions in Modern Computing Systems

![Abstractions in Modern Computing Systems](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1587971774/notepad/2020-04-27_151507_t78ly3.webp)

*注: 红色部分为计算机体系结构所关注的内容.*

## Great Ideas in Computer Architectures

1. Design for **Moore’s Law**
2. Use **abstraction** to simplify design
3. Make the **common case fast**
4. Performance via **parallelism**
5. Performance via **pipelining**
6. Performance via **prediction**
7. **Hierarchy** of memories
8. **Dependability** via redundancy

## Sequential Processor Performance

![Sequential Processor Performance](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1587972223/notepad/2020-04-27_152135_hzndhn.webp)

## 课程内容

Computer Organization

- Basic Pipelined Processor

Components of a Computer

- Same components for all kinds of computer
  - Desktop, server, embedded
- Input/output includes
  - User-interface devices
    - Display, keyboard, mouse
  - Storage devices
    - Hard disk, CD/DVD, flash
  - Network adapters
    - For communicacng with other computers

Computer Architecture

- Instruction Level Parallelism
  - Superscalar
  - Very Long Instruction Word(VLIW)
- Long Pipelines(Pipeline Parallelism)
- Advanced Memory and Caches
- Data Level Parallelism
  - Vector
  - GPU
- Thread Level Parallelism
  - Multithreading
  - Multiprocessor
  - Multicore
  - Manycore

## Architecture vs. Microarchitecture

“Architecture”/Instruction Set Architecture:

- Programmer visible state(Memory & Register)
- Operations(Instructions and how they work)
- Execution Semantics(interrupts)
- Input/Output
- Data Types/Sizes

Instruction Set Architecture

![Instruction Set Architecture](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1587973131/notepad/2020-04-27_153742_jeymfb.webp)

*注: 一个软件与硬件之间的契约.*

- Properces of a good abstraccon
  - Lasts through many generacons(portability)
  - Used in many different ways(generality)
  - Provides convenient funcconality to higher levels
  - Permits an efficient implementacon at lower levels

Microarchitecture/Organization:

- Tradeoffs on how to implement ISA for some metric(Speed, Energy, Cost)
- Examples: Pipeline depth, number of pipelines, cache size, silicon area, peak power, execution ordering, bus widths, ALU widths

## 软件的发展

up to 1955, Libraries of numerical routines

- Floating point operations
- Transcendental functions
- Matrix manipulation, equation solvers, ...

1955-60, High level Languages - Fortran 1956
Operating Systems

- Assemblers, Loaders, Linkers, Compilers
- Accounting programs to keep track of usage and charges

IBM的兼容性问题

By early 1960’s, IBM had 4 incompatible lines of computers!

701 ⇒ 7094
650 ⇒ 7074
702 ⇒ 7080
1401 ⇒ 7010

Each system had its own

- Instruction set
- I/O system and Secondary Storage: magnetic tapes, drums and disks
- assemblers, compilers, libraries, ...
- market niche business, scientific, real time, ...

这会导致什么问题？ 软件兼容性问题

=> **IBM 360** 解决该问题

IBM 360: A General-Purpose Register(GPR) Machine

- Processor State
  - 16 General-Purpose 32-bit Registers
    - may be used as index and base register
    - Register 0 has some special properties
  - 4 Floating Point 64-bit Registers
  - A Program Status Word(PSW)
    - PC, Condition codes, Control flags
- A 32-bit machine with 24-bit addresses
  - But no instruction contains a 24-bit address!
- Data Formats
  - 8-bit bytes, 16-bit half-words, 32-bit words, 64-bit double-words

IBM 360: Initial Implementations

-|Model 30 | Model 70
-|-|-
Storage | 8K - 64 KB | 256K - 512 KB
Datapath | 8-bit | 64-bit
Circuit Delay | 30 nsec/level | 5 nsec/level
Local Store | Main Store | Transistor Registers
Control Store | Read only 1µsec | Conventional circuits

IBM 360 instruction set architecture(ISA) completely hid the underlying technological differences between various models.

Milestone: The first true ISA designed as portable hardware-software interface!

With minor modifications it still survives today!

IBM 360: Over 50 years later…

The zSeries z14 Microprocessor

![The zSeries z14 Microprocessor](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1587974522/notepad/2020-04-27_160118_dpg9z6.webp)

- 5.2 GHz in IBM 14nm SOI CMOS technology
- 6.1 billion transistors in 696 mm
2
- 64-bit virtual addressing
  - original S/360 was 24-bit, and S/370 was 31-bit extension
- 10-core design
- 6-fetch/cycle
- 10-issue/cycle out-of-order superscalar pipeline
- Out-of-order memory accesses
- Redundant datapaths
  - every instruction performed in two arallel datapaths and
results compared
- 128KB L1 I-cache, 128KB L1 D-cache on-chip
- 2MB private I-cache L2 per core
- 4MB private D-cache L2 per core
- On-Chip 128MB eDRAM L3 cache
- Up to 672MB eDRAM L4

## Same Architecture Different Microarchitecture

AMDPhenomX4 | IntelAtom | IBMPOWER7
-|-|-
X86InstructionSet | X86InstructionSet | PowerInstructionSet
QuadCore | SingleCore | EightCore
125W | 2W | 200W
Decode3Instructions/Cycle/Core |  Decode2Instructions/Cycle/Core | Decode6Instructions/Cycle/Core
64KBL1ICache,64KBL1DCache | 32KBL1ICache,24KBL1DCache | 32KBL1ICache,32KBL1DCache
512KBL2Cache | 512KBL2Cache | 256KBL2Cache
Out-of-order | In-order | Out-of-order
2.6GHz | 1.6GHz | 4.25GHz

## Architectural Challenges

![Architectural Challenges](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588140686/notepad/2020-04-29_140940_kzrkuc.webp)

- Massive(ca.4X)increaseinconcurrency
  - Mulccore(4-<100)àManycores(100s–1ks)
- Heterogeneity
  - System-level(accelerators)vschiplevel(embedded)
- Computepowerandmemoryspeedchallenges(twowalls)
  - 500xcomputepowerand30xmemoryof2PFHW
  - Memoryaccess'melagsfurtherbehind
