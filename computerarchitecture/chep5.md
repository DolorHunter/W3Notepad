# 05.MIPS单周期微架构

## CPU中的状态元素

![CPU中的状态元素](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588503463/notepad/2020-05-03_185333_lqswnx.webp)

## 一些假设 (not practical)

* “Magic” memory and register file
* Combinational read
  * output of the read data port is a **combinational function** of the register file contents and the corresponding\(相应\) read select port
* Synchronous write
  * the selected register is updated on the positive edge clock transition when write enable is asserted
* Cannot affect read output in between clock edges
* Single-cycle, synchronous memory
  * Contrast this with memory that tells when the data is ready
  * i.e., Ready bit: indicating the read or write is done

## MIPS的指令处理过程

5 generic steps

* Instruction fetch \(IF\)\(取指\)
* Instruction decode and register operand fetch \(ID/RF\)
* Execute/Evaluate memory address \(EX/AG\)
* Memory operand fetch \(MEM\)
* Store/writeback result \(WB\)

![MIPS的指令处理过程](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588503463/notepad/2020-05-03_185635_q6mf0w.webp)

完整的MIPS数据通路

![完整的MIPS数据通路](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588503608/notepad/2020-05-03_185938_wjbakd.webp)

## 算术和逻辑指令的单周期数据通路

### R-Type ALU Instructions

* Assembly \(e.g., register-register signed addition\)
  * `ADD rd_reg rs_reg rt_reg`
* Machine encoding

![R-Type](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588518803/notepad/2020-05-03_225256_je98h5.webp)

* Semantics

```verilog
if MEM[PC] == ADD rd rs rt
    GPR[rd] <— GPR[rs] + GPR[rt]
    PC <— PC + 4
```

ALU Datapath

![ALU Datapath](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588518802/notepad/2020-05-03_225552_ffmq2b.webp)

### I-Type ALU Instructions

* Assembly \(e.g., register-immediate signed additions\)
  * `ADDI rt_reg rs_reg immediate_16`
* Machine encoding

![I-Type](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588518891/notepad/2020-05-03_231423_ffvjlc.webp)

* Semantics

```verilog
if MEM[PC] == ADDI rt rs immediate
    GPR[rt] <— GPR[rs] + sign-extend (immediate)
    PC <— PC + 4
```

ALU Datapath

![ALU Datapath](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588519001/notepad/2020-05-03_231606_gcc93i.webp)

### Datapath for R and I-Type ALU Insts

如何完善数据通路？

![Datapath for R and I-Type ALU Insts](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588518803/notepad/2020-05-03_230218_gdzcne.webp)

_注: 增加两个多路选择器, 以完成不同的指令._

## 数据转移指令的单周期数据通路

### Load Instructions

* Assembly \(e.g., load 4-byte word\)
  * `LW rt_reg offset_16 (base_reg)`
* Machine encoding

![I-Type](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588518803/notepad/2020-05-03_230715_a65bz8.webp)

* Semantics

```verilog
if MEM[PC]==LW rt offset_16(base)
    EA = sign-extend(offset) + GPR[base]  // 有效地址 = 偏移量(符号拓展) + 基址寄存器
    GPR[rt] <— MEM[ translate(EA) ]  // rt寄存器 <— 有效地址(地址转换)
    PC <— PC + 4
```

LW 数据通路

![LW 数据通路(连线前)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588519257/notepad/2020-05-03_232004_isbi9y.webp)

![LW 数据通路(连线后)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588519458/notepad/2020-05-03_232340_ahe06z.webp)

### Store Instructions

* Assembly \(e.g., store 4-byte word\)
  * `SW rt_reg offset_16(base_reg)`
* Machine encoding

![I-Type](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588519685/notepad/2020-05-03_232549_ridoj6.webp)

* Semantics

```verilog
if MEM[PC]==SW  rt offset_16(base)
    EA = sign-extend(offset) + GPR[base]
    MEM[ translate(EA) ] <— GPR[rt]
    PC <— PC + 4
```

SW数据通路

![SW 数据通路(连线前)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588520633/notepad/2020-05-03_232840_bdodzy.webp)

![SW 数据通路(连线后)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588520608/notepad/2020-05-03_234239_exizwo.webp)

_注: rt 在 Machine encoding 的第二个 5-bit 位置, 所以 Read data 2 == [rt]._

### Load-Store数据通路

![Load-Store数据通路](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588521513/notepad/2020-05-03_235140_c2ml9e.webp)

![Load和Store的数据通路实例](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588521514/notepad/2020-05-03_235453_stebzy.webp)

### 非控制流指令的数据通路

![非控制流指令的数据通路](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588521514/notepad/2020-05-03_235648_rlawra.webp)

_注: 整合了所有的指令. 不同之处在于多了下方的多路选择器(MemtoReg)._

## 控制流指令的单周期数据通路

### 无条件跳转指令

* Assembly
  * `J immediate_26`
* Machine encoding

![J-Type](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588522105/notepad/2020-05-04_000131_rilryh.webp)

* Semantics

```verilog
if MEM[PC]==J immediate_26
target = { (PC+4)[31:28], immediate_26, 2’b00 }
PC <— target
```

无条件跳转指令的数据通路

![无条件跳转指令的数据通路](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588522105/notepad/2020-05-04_000239_wbbbay.webp)

![无条件跳转指令的数据通路实例](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588522105/notepad/2020-05-04_000655_wt33ut.webp)

_注: 取 PC+4 的高四位 [31:28] 和指令的 26 位立即数合并, 再补 00 (也可以左移两位), 最后赋值给 PC._

### 条件分支指令

* Assembly \(e.g., branch if equal\)
  * `BEQ rs_reg rt_reg immediate_16`
* Machine encoding

![I-Type](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588522529/notepad/2020-05-04_001320_k2dluj.webp)

* Semantics \(assuming no branch delay slot\)

```verilog
if MEM[PC]==BEQ rs rt immediate_16
    target = PC + 4 + sign-extend(immediate) x 4
    if GPR[rs]==GPR[rt] then  PC <— target
    else  PC <— PC + 4
```

Conditional Branch Datapath \(for you to finish\)

![Conditional Branch Datapath (for you to finish)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588522762/notepad/2020-05-04_001645_kujxu6.webp)

![Conditional Branch Datapath (finished)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588522762/notepad/2020-05-04_001844_akbj5n.webp)

_注: 先给潜在目标\(PC\) +4, 与低16位做符号扩展, 扩展前先 x4 移 2 位. 得到的结果为潜在的 target, 赋值给 PCSrc. 译码寄存器读两个寄存器的值, ALU 做减法比较两者是否相等\(结果是否为 0\). 如果相等, target 赋值给 PC, 否则 PC + 4._

## 单周期控制逻辑

### 单周期硬布线控制

* As combinational function of Inst=MEM[PC]

![R/I/J Types](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588523424/notepad/2020-05-04_002922_rdctwa.webp)

_注: 通常由 opcode 和 funct 组合产生控制信号._

* Consider
  * All R-type and I-type ALU instructions
  * LW and SW
  * BEQ, BNE, BLEZ, BGTZ
  * J, JR, JAL, JALR

### 单个比特控制信号

| - | When De-asserted | When asserted | Equation |
| :--- | :--- | :--- | :--- |
| RegDest | GPR write select according to rt, i.e., inst[20:16] | GPR write select according to `rd`, i.e., inst[15:11] | `opcode`==0 |
| ALUSrc | 2^nd ALU input from 2^nd GPR read port | 2^nd ALU input from signextended 16-bit immediate | (`opcode`!=0) && (`opcode`!=BEQ) && (`opcode`!=BNE) |
| MemtoReg | Steer ALU result to GPR write port | steer memory load to GPR wr. port | `opcode`==LW |
| RegWrite | GPR write disabled | GPR write enabled | (`opcode`!=SW) && (`opcode`!=Bxx)  && (`opcode`!=J) && (`opcode`!=JR)) |
| MemRead | Memory read disabled | Memory read port return load value | `opcode`==LW |
| MemWrite | Memory write disabled | Memory write enabled | `opcode`==SW |
| PCSrc_1 | According to PCSrc_2 | next PC is based on 26-bit immediate jump target | (`opcode`==J) || (`opcode`==JAL) |
| PCSrc_2 | next PC = PC + 4 | next PC is based on 16-bit immediate branch target | (`opcode`==Bxx)  && “bcond is satisfied” |

### ALU Control

* case opcode
  * ‘0’ -> select operation according to funct
  * ‘ALUi’ -> selection operation according to opcode
  * ‘LW’ -> select addition
  * ‘SW’ -> select addition
  * ‘Bxx’ -> select bcond generation function
  * __ -> don’t care
* Example ALU operations
  * ADD, SUB, AND, OR, XOR, NOR, etc.
  * bcond on equal, not equal, LE zero, GT zero, etc.

### R-Type ALU

![R-Type ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574068/notepad/2020-05-04_142951_quvb1u.webp)

![R-Type ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588575348/notepad/2020-05-04_143751_o5bdmv.webp)

_注: R 类指令 PC+4, PCsrc==0. 不是 JMP 指令, PCsrc2=Br Trken 回传 PC. 寄存器目标为 1-15, 读出 Reg data 1 和 2, 结合功能码进行译码操作. 不需要写入 MEM, 写回至 Write Data 结束._

### I-Type ALU

![I-Type ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574065/notepad/2020-05-04_143026_kyqxi1.webp)

![I-Type ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588575347/notepad/2020-05-04_144922_bmg49v.webp)

_注: I 类 PC+4 的部分与 R 类类似, 不介绍. ALU 内容来自reg和立即数. 结果也是写回至 Write data._

### LW

![LW](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574072/notepad/2020-05-04_143204_ukggyr.webp)

![LW](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588575347/notepad/2020-05-04_145338_zuf2go.webp)

_注: 不改变数据流, PCsrc 处相同. ALU 输入来自 reg1\(基址\) 和 立即数\(偏移量\). 此时进行访存, read data\(MEM\) 写回至 Write data._

### SW

![SW](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574055/notepad/2020-05-04_143223_jccav7.webp)

![SW](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588576371/notepad/2020-05-04_150022_buodd4.webp)

_注: SW 与 LW 方向相反. PCsrc 处相同. SW 不需要写寄存器\(Write reg/data\). ALU 输入数据来自 reg1 和 立即数, 赋值给 add\(MEM\), reg2 的五位为写入 data\(MEM\) 的内容._

### Branch (Not Taken)

![Branch (Not Taken)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574066/notepad/2020-05-04_143238_mhpszc.webp)

![Branch (Not Taken)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588576371/notepad/2020-05-04_150320_wl6eid.webp)

_注: 条件分支的不跳\(条件不满足\)称为 not taken. 先对 reg 中的 data 1 和 2 比较, 不满足结果为 0. 仍然为 PC+4\(不跳转\)._

### Branch (Taken)

![Branch (Taken)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574069/notepad/2020-05-04_143256_swp3pd.webp)

![Branch (Taken)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588576371/notepad/2020-05-04_150729_devemm.webp)

_注: 跳转即为 reg data 1 和 2 相等, 结果为 1. 所以 PCsrc2 = BrTaken\(BrTaken 为低 16 位符号拓展左移两位, 再加上 PC+4\). 最后 PCsrc2 -> PC._

### Jump

![Jump](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588574087/notepad/2020-05-04_143150_rxbsoo.webp)

![Jump](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588576371/notepad/2020-05-04_151201_wp9ebt.webp)

_注: JMP 比较简单. 取指译码 PCsrc1==JMP. 把 PC 的高 4 位和指令的第 26 位合并, 再补两位, 组成 32 位跳转目标地址. 最后 PCsrc1 -> PC._

### What is in the Control Box

注: `控制信号是怎么产生的?` **组合逻辑** 和 **时序逻辑**.

* Combinational Logic -> `Hardwired Control`
  * Idea: Control signals generated combinationally based on instruction
  * Necessary in a single-cycle microarchitecture…
* Sequential Logic -> `Sequential/Microprogrammed Control`
  * Idea: A memory structure contains the control signals associated with an instruction
  * Control Store

## MIPS单周期微架构的评测

### A Single-Cycle Microarchitecture: Analysis

* Every instruction takes 1 cycle to execute
  * CPI (Cycles per instruction) is strictly 1
* How long each instruction takes is determined by how long the slowest instruction takes to execute
  * Even though many instructions do not need that long to execute
* `Clock cycle time of the microarchitecture is determined by how long it takes to complete the slowest instruction`
  * Critical path of the design is determined by the processing time of the slowest instruction

### What is the Slowest Instruction to Process

* Let ’ s go back to the basics
* All five phases of the instruction processing cycle take a single machine clock cycle to complete
  * `Instruction fetch (IF)`
  * `Instruction decode and register operand fetch (ID/RF)`
  * `Execute/Evaluate memory address (EX/AG)`
  * `Memory operand fetch (MEM)`
  * `Store/writeback result (WB)`
* Do each of the above phases take the same time (latency) for all instructions?

### 单周期数据通路分析

* Assume
  * memory units \(read or write\): 200 ps
  * ALU and adders: 100 ps
  * register file \(read or write\): 50  ps
  * other combinational logic: 0 ps

![单周期数据通路分析](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588577168/notepad/2020-05-04_152149_kuuk42.webp)

### 找出关键路径

### R-Type and I-Type ALU

![R-Type and I-Type ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588577168/notepad/2020-05-04_152342_cwk6rh.webp)

### LW ALU

![LW ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588577169/notepad/2020-05-04_152418_q9pptt.webp)

### SW ALU

![SW ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588577169/notepad/2020-05-04_152437_hee4ts.webp)

### Branch Taken ALU

![Branch Taken ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588577169/notepad/2020-05-04_152456_ayfekp.webp)

### Jump ALU

![Jump ALU](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588577169/notepad/2020-05-04_152517_a4qsfs.webp)

