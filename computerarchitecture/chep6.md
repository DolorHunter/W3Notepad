# 06.Pipelining & Hazards

## 复习：流水线的基本思想

- More systematically:
  - Pipelining the execution of multiple instructions
- Idea:
  - `Divide the instruction processing cycle into` **`distinct “stages”`** `of processing`
  - Ensure there are enough hardware resources to process   one instruction in each stage
  - `Process a different instruction in each stage simultaneously`
    - Instructions consecutive in program order are processed in consecutive stages
- Benefit: Increases instruction processing throughput

### 示例：加法操作的处理

- Multi-cycle: 4 cycles per instruction

![Multi-cycle](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746637/notepad/2020-05-06_140638_xz4dux.webp)

- Pipelined: 4 cycles per 4 instructions

![Pipelined](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746637/notepad/2020-05-06_140653_deydqi.webp)

*实际总是如此美好吗？*

## 理想的流水线

- Goal: `Increase throughput with little increase in cost` (hardware cost, in case of instruction processing)
- Repetition of `identical operations`
  - The same operation is repeated on a large number of different inputs
- Repetition of `independent operations`
  - No dependencies between repeated operations
- `Uniformly partitionable suboperations`
  - Processing can be evenly divided into uniform-latency suboperations

![理想的流水线](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746638/notepad/2020-05-06_141144_kzcbm6.webp)

## 现实的流水线

### 现实的流水线: Throughput

- Nonpipelined version with delay T

```plain
BW = 1/(T+S) where S = latch delay
```

![Nonpipelined](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746638/notepad/2020-05-06_141758_nvy2of.webp)

- k-stage pipelined version

```plain
BW_k-stage = 1 / (T/k +S )
BW_max = 1 / (1 gate delay + S )
```

![k-stage pipelined](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746636/notepad/2020-05-06_141817_j3kftf.webp)

*注: 灰色为寄存器, 用于保存数据. 实际流水操作后需要计算保存数据的时间. 因为组合逻辑的时间/延迟不会低于门的延迟, 因此最大 BW 时, T/K = 1 gate delay.*

### 现实的流水线: Cost

- Nonpipelined version with combinational cost G

```plain
Cost = G+L where L = latch cost
```

![Nonpipelined](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746636/notepad/2020-05-06_141958_zwr8yf.webp)

- k-stage pipelined version

```plain
Cost_k-stage = G + Lk
```

![k-stage pipelined](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746636/notepad/2020-05-06_142012_d5lhs3.webp)

## Pipeline: 指令处理过程

1. Instruction fetch (IF)
2. Instruction decode and register operand fetch (ID/RF)
3. Execute/Evaluate memory address (EX/AG)
4. Memory operand fetch (MEM)
5. Store/writeback result (WB)

goto 1

### 记住单周期微架构

![单周期微架构](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746637/notepad/2020-05-06_142139_uqmwkn.webp)

### 分割为阶段

![分割为阶段](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746637/notepad/2020-05-06_142227_tshmsr.webp)

- Is this the correct partitioning?
- Why not 4 or 6 stages?
- Why not different boundaries?

### Instruction Pipeline Throughput

![Instruction Pipeline Throughput](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588746637/notepad/2020-05-06_142508_f2pmnm.webp)

5-stage speedup is 4, not 5 as predicted by the ideal model. Why?\
*各个段不均匀/等长.*

### 流水线的重要元素: Pipeline Registers

![Pipeline Registers](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588747589/notepad/2020-05-06_142653_skgtn9.webp)

*注: 淡黄色部分为 Pipeline Registers.*

## 描述流水操作

### 描述流水操作: Operation View

![Operation View](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588747589/notepad/2020-05-06_143523_uu0wy5.webp)

### 描述流水操作: 时空图

- | t0 | t1 | t2 | t3 | t4 | t5 | t6 | t7 | t8 | t9 | t10
-|-|-|-|-|-|-|-|-|-|-|-
IF | I0 | I1 | I2 | I3 | I4 | I5 | I6 | I7 | I8 | I9 | I10
ID | - | I0 | I1 | I2 | I3 | I4 | I5 | I6 | I7 | I8 | I9
EX | - | - | I0 | I1 | I2 | I3 | I4 | I5 | I6 | I7 | I8
MEM | - | - | - | I0 | I1 | I2 | I3 | I4 | I5 | I6 | I7
WB | - | - | - | - | I0 | I1 | I2 | I3 | I4 | I5 | I6

*注: t4 开始为 full pipeline.*

## 流水线的控制

`Identical set of control points as the single-cycle datapath!!`

- For a given instruction
  - same control signals as single-cycle, but
  - control signals required at different cycles, depending on stage
  1. Option 1: decode once using the same logic as single-cycle and buffer signals until consumed\
  ![Option 1](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588747590/notepad/2020-05-06_144443_ly12zv.webp)
  2. Option 2: carry relevant “instruction word/field” down the pipeline and decode locally within each or in a previous stage
Which one is better?

*注: 第一种更好. 只需要一次译码, 之后只传输控制信号.*

### 流水化的控制信号

![流水化的控制信号](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588748944/notepad/2020-05-06_144915_nhr4j7.webp)

## Remember: 理想的流水线

- Goal: `Increase throughput with little increase in cost` (hardware cost, in case of instruction processing)
- Repetition of `identical operations`
  - The same operation is repeated on a large number of different inputs (e.g., all laundry loads go through the same steps)
- Repetition of `independent operations`
  - No dependencies between repeated operations
- `Uniformly partitionable suboperations`
  - Processing an be evenly divided into uniform-latency suboperations (that do not share resources)

### Instruction Pipeline: 不是理想流水线

- `Identical operations ... NOT!`\
-> **different instructions -> not all need the same stages**\
Forcing different instructions to go through the same pipe stages\
-> *external fragmentation* (some pipe stages idle for some instructions)
- `Uniform suboperations  ... NOT!`\
-> different pipeline stages -> not the same latency\
Need to force each stage to be controlled by the same clock\
-> *internal fragmentation* (some pipe stages are too fast but all take the same clock cycle time)
- `Independent operations ... NOT!`\
-> instructions are not independent of each other\
Need to detect and resolve nter-instruction dependencies to ensure the pipeline provides correct results\
-> *pipeline stalls* (pipeline is not always moving)

## 流水线设计面临的问题

- Balancing work in pipeline stages
  - How many stages and what is done in each stage
- Keeping the pipeline `correct, moving, and full` in the presence of events that disrupt pipeline flow
  - Handling dependences
    - Data
    - Control
  - Handling resource contention
  - Handling long-latency (multi-cycle) operations
- Handling exceptions, interrupts
- Overall: Improving pipeline throughput
  - Minimizing stalls

### 引起流水线停顿的原因

- Stall: A condition when the pipeline stops moving
- Resource contention
- Dependences (between instructions)
  - Data
  - Control
- Long-latency (multi-cycle) operations

### 相关(denpendence)及其类型

- Also called “ dependency” or less desirably “hazard”
- Dependences dictate ordering requirements between instructions
- Two types
  - Data dependence
  - Control dependence
- Resource contention is sometimes called resource dependence
  - However, this is not fundamental to program semantics.

### 处理资源冲突(Resource Contention)

- Happens when instructions in two pipeline stages need the same resource
- Solution 1: `Eliminate the cause of contention`
  - Duplicate the resource or increase its throughput
    - E.g., use separate instruction and data memories (caches)
    - E.g., use multiple ports for memory structures
- Solution 2: `Detect the resource` contention and stall one of the contending stages
  - Which stage do you stall?
  - Example: What if you had a single read and write port for the register file?

### 数据相关(Data Dependence)

- Types of data dependences
  - `Flow dependence` (true dependence - read after write)
  - `Output dependence` (write after write)
  - `Anti dependence` (write after read)
- Which ones cause stalls in a pipelined machine?
  - `For all of them, we need to ensure semantics of the program is correct(底线)`
  - **Flow dependences always need to be obeyed** because they constitute true dependence on a value
  - `Anti and output dependences exist due to limited number of registers in the CPU`
    - They are **dependence on a name**, not a value
    - We will later see what we can do about them

### 数据相关示例

![数据相关示例](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588748945/notepad/2020-05-06_150547_vuhykh.webp)

### 如何处理数据相关

- Anti and output dependences are easier to handle
  - write to the destination in one stage and in program order
- Flow dependences are more interesting
- Four typical ways of handling flow dependences
  - Detect and wait until value is available in register file
  - Detect and forward/bypass data to dependent instruction
  - Detect and eliminate the dependence at the software level
    - No need for the hardware to detect dependence
  - Predict the needed value(s), execute “speculatively ” , and verify

## Pipeline Interlocking (流水线互锁)

- Detection of dependence between instructions in a pipelined processor to guarantee correct execution
- Software based interlocking
- Hardware based interlocking
- MIPS acronym?

### 相关检测的方法 (1)

- `Scoreboarding (记分牌算法)`
  - Each register in register file has a Valid bit associated with it
  - An instruction that is writing to the register resets the Valid bit (set to 0)
  - An instruction in Decode stage checks if all its source and destination registers are Valid (value is 1)
    - Yes: No need to stall… No dependence
    - No: Stall the instruction
- Advantage:
  - Simple, 1 bit per register
- Disadvantage:
  - Need to stall for all types of dependences, not only flow dependence.

如何在输出相关和反相关不停顿？

What changes would you make to the scoreboard to enable this?

### 相关检测的方法 (2)

- `Combinational dependence check logic`
  - Special logic that checks if any instruction in **later stages** (pipeline stages) is supposed to write to any source register of the instruction that is being decoded
  - Yes: stall the instruction/pipeline
  - No: no need to stall… no flow dependence
- Advantage:
  - No need to stall on anti and output dependences
- Disadvantage:
  - Logic is more complex than a scoreboard
  - Logic becomes more complex as we make the pipeline deeper and wider (`think superscalar execution`)

通过硬件检测到相关之后呢……

- What do you do afterwards?
- `Observation: Dependence between two instructions is detected before the communicated data value becomes available`
- Option 1: Stall the dependent instruction right away
- Option 2: Stall the dependent instruction only when necessary  data forwarding/bypassing
- Option 3: …


