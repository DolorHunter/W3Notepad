# 07.Dependence Handling

## Data Dependence Types

![Data Dependence Types](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588748945/notepad/2020-05-06_150547_vuhykh.webp)

*注: 真相关.*

## 记住如何处理数据相关

- Anti and output dependences are easier to handle
  - `write to the destination in one stage and in program order`
- Flow dependences are more interesting
- *Four typical ways of handling flow dependences*
  - `Detect and wait` until value is available in register file
  - `Detect and forward/bypass` data to dependent instruction
  - `Detect and eliminate` the dependence at the software level
    - No need for the hardware to detect dependence
  - `Predict` the needed value(s), execute “speculatively ” , `and verify`

## RAW相关处理

- Which one of the following flow dependences lead to conflicts in the 5-stage pipeline?

![conflicts in the 5-stage pipeline](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354390/notepad/2020-05-13_141104_ulmhac.webp)

## 寄存器数据相关分析

- | R/I-Type | LW | SW | Br | J | Jr
-|-|-|-|-|-|-
IF | - | - | - | - | - | -
ID | read RF | read RF | read RF | read RF | read RF
EX | - | - | - | - | - | -
MEM | - | - | - | - | - | -
WB | write RF | write RF

- For a given pipeline, when is there a potential conflict between two data dependent instructions?
  - dependence type: RAW, WAR, WAW?
  - instruction types involved?
  - distance between the two instructions?

### Safe and Unsafe Movement of Pipeline

![Safe and Unsafe Movement of Pipeline](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354391/notepad/2020-05-13_141613_n4i5od.webp)

- dist(i,j) <= dist(X,Y) => `Unsafe to keep j moving`
- dist(i,j) > dist(X,Y) => `Safe`

*注: 读写距离为3(见表 - 寄存器数据相关分析), 故小于3不安全.*

*上接表 - 寄存器数据相关分析.*

- Instructions IA and IB(where IA comes before IB) have RAW dependence `iff`
  - IB`(R/I, LW, SW, Br or JR)` reads a register written by IA`(R/I or LW)`
  - dist(IA, IB) <= dist(ID, WB) = 3

What about WAW and WAR dependence?

## Pipeline Stall: 解决数据相关

![Pipeline Stall: 解决数据相关](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354391/notepad/2020-05-13_142834_dzhpte.webp)

*注: bubble相当于空指令, 加入 bubble 是为了使 dist>3.*

## 如何实现Pipeline Stall

![如何实现Pipeline Stall](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354391/notepad/2020-05-13_143027_fusjn5.webp)

- `Stall`
  - disable `PC` and `IR` latching; ensure stalled instruction stays in its stage
  - Insert “invalid” `instructions/nops` into the stage following the stalled one (called “bubbles”)

## Stall Conditions

- Instructions IA and IB(where IA comes before IB) have RAW dependence `iff`
  - IB`(R/I, LW, SW, Br or JR)` reads a register written by IA`(R/I or LW)`
  - dist(IA, IB) <= dist(ID, WB) = 3
- Must stall the ID stage when IB in ID stage wants to read a register to be written by IA in EX, MEM or WB stage

## 暂停条件检测逻辑

![暂停条件检测逻辑](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354391/notepad/2020-05-13_143546_fe7bxb.webp)

*注: 六个条件满足任意都会冲突.*

- It is crucial that the EX, MEM and WB stages continue to advance normally during stall cycles

## Pipeline Stall对性能的影响

- Each stall cycle corresponds to `one lost cycle` in which no instruction can be completed
- For a program with N instructions and S stall cycles, ```Average CPI=(N+S)/N```
- S depends on
  - frequency of RAW dependences
  - exact distance between the dependent instructions
  - distance between dependences

`suppose i1,i2 and i3 all depend on i0, once i1’s dependence is resolved, i2 and i3 must be okay too`

## Sample Assembly (P&H)

- for (j=i-1; j>=0 && v[j] > v[j+1]; j-=1) { ...... }

![Sample Assembly (P&H)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354390/notepad/2020-05-13_143902_op21j3.webp)

*注: 判断真相关来确定 stalls 数量. e.g. slti 用到 addi 的 $s1, 是真相关, stalls += 3.(此例子中的所有真相关距离均为1, 方便计算).*

## 通过Data Forwarding减少Stall

- Also called *Data Bypassing*
- *Forward the value to the dependent instruction as soon as* `it is available`
- Dataflow model
  - Data value supplied to dependent instruction as soon as it is available
  - Instruction executes when all its operands are available
- `Data forwarding brings a pipeline closer to data flow execution principles`

## Data Forwarding (Bypassing)

- 将RF看做状态 `(state)` 更加直观
  - “*add rx ry rz*” literally means get values from *RF[ry]* and *RF[rz]* respectively and put result in *RF[rx]*
- 但是，RF仅仅是通信抽象的一部分 (协助通信)。
  - “*add rx ry rz*” **actually means**:
    1. get the results of the last instructions to define the values of *RF[ry]* and *RF[rz]*, respectively,
    2. until another instruction redefines *RF[rx]*, younger instructions that refer to *RF[rx]* should use this instruction’s result
- `What matters is to maintain the correct “data flow” between operations`, *thus*

![thus](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354390/notepad/2020-05-13_150438_c7ljrl.webp)

## 利用Forwarding解决RAW相关

- Instructions IA and IB(where IA comes before IB) have RAW dependence iff
  - IB`(R/I, LW, SW, Br or JR)` reads a egister written by IA`(R/I or LW)`
  - dist(IA, IB) <= dist(ID, WB) = 3
- In other words, if IB in ID stage reads a register written by IA in EX, MEM or WB stage, then the operand required by IB is not yet in RF
- How forwarding works?
  - *=> retrieve operand from `datapath` instead of the RF*
  - *=> retrieve operand from the `youngest definition` if multiple definitions are outstanding*

## Data Forwarding Paths (v1)

![Data Forwarding Paths (v1)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354390/notepad/2020-05-13_151147_dt0gvp.webp)

## Data Forwarding Paths (v2)

![Data Forwarding Paths (v2)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354390/notepad/2020-05-13_151238_atyzeo.webp)

## Data Forwarding Logic (for v2)

![Data Forwarding Logic (v2)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589354390/notepad/2020-05-13_151517_pk67ae.webp)

Ordering matters!! Must check youngest match first

Why doesn’t `use_rs( )` appear in the forwarding logic?

## Data Forwarding (相关分析)

- | R/I-Type | LW | SW | Br | J | Jr
-|-|-|-|-|-|-
IF | - | - | - | - | - | -
ID | - | - | - | - | - | use
EX | use/produce | use | use | use | - | -
MEM | - | produce (use) | - | - | - | -
WB | - | - | - | - | - | -

- Even with data-forwarding, RAW dependence on an immediately preceding LW instruction requires a stall

## 回顾: Control Dependence

- Question: What should the fetch PC be in the next cycle?
- Answer: The address of the next instruction
  - All instructions are control dependent on previous ones. Why?
- If the fetched instruction is a non-control-flow instruction:
  - Next Fetch PC is the address of the next-sequential instruction
  - Easy to determine if we know the size of the fetched instruction
- *If the instruction that is fetched is a control-flow instruction:*
  - *How do we determine the next Fetch PC?*
- `In fact, how do we even know whether or not the fetched instruction is a control-flow instruction?`

## 分支类型

Type | Direction at fetch time | Number of possible next fetch addresses? | When is next fetch address resolved?
-|-|-|-
Conditional | Unknown | 2 | Execution (register dependent)
Unconditional | Always taken | 1 | Decode (PC + offset)
Call | Always taken | 1 | Decode (PC + offset)
Return | Always taken | Many | Execution (register dependent)
Indirect | Always taken | Many | Execution (register dependent)

Different branch types can be handled differently

## 如何处理控制相关

- Critical to keep the pipeline full with correct sequence of dynamic instructions.
- Potential solutions if the instruction is a control-flow instruction:
  - *Stall* the pipeline until we know the next fetch address
  - Guess the next fetch address (*branch prediction*)
  - Employ delayed branching (*branch delay slot*)
  - ……

## Stall Fetch Until Next PC is Available: Good Idea or not

![Stall Fetch Until Next PC](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589355894/notepad/2020-05-13_153048_fcuoik.webp)

*This is the case with non-control-flow and unconditional br instructions!*

## Doing Better than Stalling Fetch …

- Rather than waiting for true-dependence on PC to resolve, *just guess nextPC = PC+4 to keep fetching every cycle*
  - `Is this a good guess?`
  - `What do you lose if you guessed incorrectly?`
- ~20% of the instruction mix is control flow
  - ~50 % of “forward” control flow `(i.e., if-then-else)` is taken
  - ~90% of “backward” control flow `(i.e., loop back)` is taken\
  **Overall, typically ~70% taken and ~30% not taken [Lee and Smith, 1984]**
- Expect “nextPC = PC+4” ~86% of the time, but what about the remaining 14%?

## Guessing NextPC = PC + 4

- Always predict the next sequential instruction is the next instruction to be executed
- This is a form of *next fetch address prediction* (and branch prediction)
- How can you make this more effective?
- Idea: *Maximize the chances that the next sequential instruction is the next instruction to be executed*
  - Software: Lay out the control flow graph such that the “likely next instruction” is on the not-taken path of a branch
    - Profile guided code positioning -> Pettis & Hansen, PLDI 1990.
  - Hardware: ??? (how can you do this in hardware…)
    - Cache traces of executed instructions -> Trace cache
