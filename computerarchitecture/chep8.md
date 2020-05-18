# 08. Exploiting ILP

课程顺序: 4.1 -> 4.5 -> 4.2 -> 

## 4.1 指令级并行的概念

- 处理机利用流水线来使指令重叠并行执行，以达到提高性能的目的。这种指令之间存在的潜在并行性称为指令级并行(**ILP：InstructionLevel Parallelism**)
- 本章讨论：**如何通过各种可能的技术，获得更多的指令级并行性。**
  - 硬件＋软件: 必须要硬件技术和软件技术互相配合，才能够最大限度地挖掘出程序中存在的指令级并行。
- **流水线处理机的实际CPI**
  - 理想流水线的CPI加上各类停顿的时钟周期数：\
  ```CPI流水线 = CPI理想 + 停顿结构冲突 + 停顿数据冲突 + 停顿控制冲突```
  - 理想CPI是衡量流水线最高性能的一个指标。
  - IPC：Instructions Per Cycle
- **基本程序块 (Basic Block)**
  - 基本程序块：一段除了入口和出口以外不包含其他分支的线性代码段。
  - 据统计：程序平均每5～7条指令就会有一个分支。
- 循环级并行：使一个循环中的不同循环体并行
执行。
  - 开发循环体中存在的并行性
    - 最常见、最基本
  - 是指令级并行研究的重点之一
  - 例如，考虑下述语句：

  ```c
  for （i=1； i<=500； i++）
    a[i]=a[i]＋s；
  ```

  - 每一次循环都可以与其他的循环重叠并行执行；
  - 在每一次循环的内部，却没有任何的并行性。
- **最基本的开发循环级并行的技术**
  - 循环展开（loop unrolling）
  - 采用向量指令和向量数据表示
- **相关与流水线冲突**
  - 流水线冲突是指对于具体的流水线来说，相关导致指令流中的下一条指令不能在指定的时钟周期执行。
    - 流水线冲突类型：结构冲突、数据冲突、控制冲突
  - 相关是程序固有的一种属性，它反映了程序中指令之间的相互依赖关系。
  - 具体的一次相关是否会导致实际冲突的发生以及该冲突会带来多长的停顿，则是流水线的属性。
- **可以从两个方面来解决相关问题：**
  - 保持相关，但避免发生冲突;
  - 通过代码变换，消除相关;
- **程序顺序：由源程序确定的在完全串行方式下指令的执行顺序。**
  - 由于相关的存在可能影响程序结果，所以必须保持程序顺序。
- **控制相关并不是一个必须严格保持的关键属性。**
- **对于正确地执行程序来说，必须保持的最关键的两个属性是：`数据流` 和 `异常行为`。**
  - 保持异常行为是指：无论怎么改变指令的执行顺序，都不能改变程序中异常的发生情况。
    - 即原来程序中是怎么发生的，改变执行顺序后还是怎么发生。
    - 为了提升性能，可以弱化为：指令执行顺序的改变不能导致程序中发生新的异常。
  - 如果我们能做到保持程序的数据相关和控制相关，就能保持程序的数据流和异常行为(从而保证正确)。
- 举例说明

  ```assembly
      DADDU R2，R3，R4
      BEQZ R2，L1
      LW R1，0（R2）
  L1 ：
  ```

  *注: 必须保持对R2的数据相关，否则会影响结果！*

- **数据流**：`指数据值从其产生者指令到其消费者指令的实际流动`。
  - 分支指令使得数据流具有动态性，因为它使得给定指令的数据可以有多个来源。
  - 仅仅保持数据相关性是不够的，只有再加上保持控制相关，才能够保持程序顺序。
- 举例：

  ```assembly
      DADDU  R1，R2，R3
      BEQZ  R4，L1
      DSUBU  R1，R5，R6
  L1 ：…
      OR  R7，`R1`，R8
  ```

  *注: R1的值依赖于数据流（之前多条指令），具体由分支指令决定！*
- 但是，有时不遵守控制相关既不影响异常行为，也不改变数据流。
  - 这时候可以大胆地进行指令调度，把失败分支中的指令调度到分支指令之前。
  - 举例：

  ```assembly
      DADDU  R1，R2，R3
      BEQZ R12，Skipnext
      `DSUBU R4，R5，R6`
      DADDU R5，R4，R9
  skip： OR R7，R8，R9
  ```

  - `假设 skip后R4不再使用，且DSUBU不产生异常，则可以将DSUBU调度到分支之前。`

## 4.2 指令的动态调度

### 指令的动态调度

- **静态调度**
  - 依靠编译器对代码进行静态调度，以减少相关和冲突。
  - 它不是在程序执行的过程中，而是在编译期间进行代码调度和优化。
  - 通过 **把相关的指令拉开距离** 来 `减少` 可能产生的停顿。
- **动态调度**
  - 在程序的执行过程中，依靠专门硬件对代码进行调度，减少数据相关导致的停顿。
- **动态调度的优点：**
  - 能够处理一些在编译时情况不明的相关（比如涉及到存储器访问的相关），并简化了编译器；
  - 能够使本来是面向某一流水线优化编译的代码在其他的流水线（动态调度）上也能高效地执行。
- **动态调度的缺点：**
  - 以硬件复杂性的显著增加为代价
- 到目前为止我们所使用流水线的最大的局限性
  - 指令必须按序流出和执行
  - 考虑下面一段代码：

  ```assembly
  DIV.D F4，F0，F2
  SUB.D F10，F4，F6
  ADD.D F12，F6，F14
  ```

  - SUB.D指令与DIV.D指令关于F4相关，导致流水线停顿；
  - **ADD.D指令与前面的指令都没有关系，但其处理也因此受阻；**
- 在前面的基本流水线中：
  - ![ID](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589785835/notepad/2020-05-18_150902_zzigk0.webp)
    - 检测 `结构` 冲突
    - 检测 `数据` 冲突
  - 一旦一条指令受阻，其后的指令都将停顿，这种处理策略会限制流水线的性能。
- **为了能够乱序执行，可以将5段流水线的译码阶段再分为两个阶段：**
  - 流出（Issue，IS）：指令译码，检查是否存在结构冲突。（**in-order issue**)
  - 读操作数（Read Operands，RO）：等待数据冲突消失，然后读操作数。\
  ![IS-R0](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589785835/notepad/2020-05-18_150936_wthdxb.webp)
- 在前述5段流水线中，是不会发生WAR冲突和WAW冲突的。`但乱序执行就使得它们可能会发生。`
  - 例如，考虑下面的代码\
  ![代码](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589786281/notepad/2020-05-18_151223_ia7xro.webp)
- Tomasulo算法可以通过使用寄存器重命名来消除上面的相关，从而提升指令执行效率。
  - 后续会详细介绍该算法
- 动态调度让指令乱序完成，会大大增加异常处理的难度。
- 动态调度需要保持正确的异常行为：
  - 对于一条会产生异常的指令来说，只有当处理机确切地知道该指令将被执行后，才允许它产生异常。
  - 即使保持了正确的异常行为，动态调度处理机仍可能发生 **不精确异常**。
- 不精确异常：
  - 当执行指令i导致发生异常时，处理机的现场（状态）与严格按程序顺序执行时指令i的现场不同。
  - 当指令i发生异常时：
    - 流水线可能已经执行完按程序顺序是位于指令i之后的指令；
    - 流水线可能还没完成按程序顺序是指令i之前的指令。
  - 不精确异常使得在异常处理后难以接着继续执行程序。
- 精确异常：
  - 如果发生异常时，处理机的现场跟严格按程序顺序执行时指令i的现场相同。

### 记分板调度算法

- 例：数据先读后写（WAR）相关引起的阻塞代码
序列：
  
  ```assembly
  DIVD F0 , F2 , F4
  ADDD F10 , F0 , F8
  SUBD F8 , F8 , F14
  ```

  - 指令乱序执行时就会出现先读后写相关。
- 记分板调度算法的目标：
  - 在资源充足时，尽可能早地执行没有数据阻塞的指令，达到每个时钟周期执行一条指令。

### CDC 6600

![CDC 6600](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589786915/notepad/2020-05-18_152630_kzqgzt.webp)

![CDC 6600 结构简图](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589786914/notepad/2020-05-18_152709_h2tjjf.webp)

### 记分板体系结构

![记分板体系结构](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589786914/notepad/2020-05-18_152740_gg0jdp.webp)

### 记分板的含义

- 指令乱序执行 => WAR, WAW冒险?
- 对WAR的解决方案：
  - 排队等待操作以及它们操作数的拷贝
  - 只在读操作数段才读取寄存器
- 对WAW的解决方案，必须检测冒险：暂停等待到其他指令
完成；
- 在执行阶段可能有多个指令 => 设置多个执行部件或者流
水化执行部件；（消除瓶颈）
- 记分板跟踪相关、状态或操作；
- 记分板用四个流水段代替ID、EX、WB三段；

### 记分板控制的四级

1. **Issue—decode instructions & check for structural hazards.**\
If a functional unit for the instruction is `free` and no other active instruction has the same destination register (`WAW`), the scoreboard issues the instruction to the functional unit and updates its internal data structure. If a structural or WAW hazard exists, then the instruction issue `stalls`, and no further instructions will issue until these hazards are cleared (`in-order issue`).
2. **Read operands—wait until no data hazards, then read operands.**\
A source operand is available if `no earlier issued` active instruction is going to write it, or if the register containing the operand is being written by a `currently active` functional unit. When the source operands are available, the scoreboard tells the functional unit to proceed to read the operands from the registers and begin execution. The scoreboard resolves `RAW` hazards dynamically in this step, and `instructions may be sent into execution out of order`.
3. **Execution—operate on operands (EX)**
The functional unit begins execution upon receiving operands. When the result is ready, it notifies the scoreboard that it has completed execution.
4. **Write result—finish execution (WB)**
Once the scoreboard is aware that the functional unit has completed execution, the scoreboard checks for WAR hazards. If none, it writes results. If `WAR`, then it stalls the instruction.\
Example:

```assembly
DIVD F0,F2,F4
ADDD F10,F0,F8
SUBD F8,F8,F14
```

CDC 6600 scoreboard would stall SUBD until ADDD reads operands

### 记分板需要记录的信息

## 4.3 动态分支预测技术

## 4.4 多指令流出技术

## 4.5 循环展开和指令调度

### 循环展开和指令调度基本方法

- **充分开发指令之间存在的并行性，找出不相关的指令序列，让它们在流水线上重叠并行执行。**
- **增加指令间并行性最简单和最常用的方法**
  - 开发循环级并行性——循环的不同迭代之间存在的并行性。
  - 在把循环展开后，通过 **寄存器重命名** 和 **指令调度** 来开发更多的并行性。
- **编译器完成这种指令调度的能力受限于两个特性：**
  - 程序固有的指令级并行性；
  - 流水线功能部件的执行延迟。
- **本小节中，我们使用的 `浮点流水线` 延迟如下：**

产生结果的指令 | 使用结果的指令 | 延迟（时钟周期数）
-|-|-
浮点计算 | 另一个浮点计算 | `3`
浮点计算 | 浮点store（S.D） | `2`
浮点load（L.D） | 浮点计算 | `1`
浮点load（L.D） | 浮点store（S.D） | `0`

- **假设采用MIPS的5段流水线：**
  - 分支的延迟：`1` 个时钟周期。
  - 整数load指令的延迟：`1` 个时钟周期。
  - 整数运算部件是全流水或者重复设置了足够的份数。

- **例4.6** 对于下面的源代码，转换成MIPS汇编语言，在不进行指令调度和进行指令调度两种情况下，分析其代码一次循环所需的执行时间。

  ```c
  for (i=1； i<=1000； i++)
    x[i] = x[i] + s；
  ```

  **解**： 把该程序翻译成MIPS汇编语言代码：\
假设：

  1. R1的初值是指向第一个元素(s1000)
  2. 8（R2）指向最后一个元素。(s1)
  3. 浮点寄存器F2：用于保存常数s。

  ```assembly
  Loop：L.D `F0`,0（R1）
      ADD.D `F4`,`F0`,F2
      S.D `F4`, 0（R1）
      DADDIU R1,R1，#-8
      BNE R1,R2,Loop
  ```

  其中：\
  整数寄存器R1：指向向量中的当前元素。（`初值为向量中最高端元素的地址`）

- 不进行指令调度的情况下，程序的实际执行情况：\
![不进行指令调度的情况下，程序的实际执行情况](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589785054/notepad/2020-05-18_143710_mdkgpq.webp)
  - 每个元素的操作需要10个时钟周期，其中5个是空转周期。
- 如果进行如下的指令调度：
  - 把DADDIU指令调度到L.D指令和ADD.D指令之间的“空转”拍；
  - 把S.D指令放到了分支指令的延迟槽中；
  - 对存储器地址偏移量进行调整；\
  ![进行如下的指令调度，程序的实际执行情况](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589785053/notepad/2020-05-18_143858_g59p4f.webp)
  - 一轮循环的操作时间从10个时钟周期减少到6个, 其中5个周期是有指令执行的，1个为空转周期。
- **例子中的问题及解决方案**
  - **问题**：只有L.D、ADD.D和S.D这3条指令是有效操作。
    - 占用3个时钟周期。
    - 而DADDIU、空转和BEN这3个时钟周期都是附加的循环控制开销。
  - **解决方案**：循环展开技术
    - 把循环体的代码复制多次并按顺序排列，然后相应调整循环的结束条件。
    - 这给编译器进行指令调度带来了更大的空间。

- **例4.7**：将上述例子中的循环展开4次得到4个循环体，然后对展开后的指令序列在不调度和调度两种情况下，分析代码的性能。假定R1的初值为32的倍数，即循环次数为4的倍数。消除冗余的指令，并且不要重复使用寄存器。

  **解**：无需在循环体后面增加补偿代码，分配寄存器（不重复使用寄存器 ）如下：

  - F0、F4：用于展开后的第1个循环体
  - F2：保存常数s
  - F6、F8：第2个循环体
  - F10、F12：第3个循环体
  - F14、F16：第4个循环体

- 展开后没有调度的代码如下：

![展开后没有调度的代码如下](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589785054/notepad/2020-05-18_144754_mjv9qc.webp)

- 结果分析:
  - 这个循环每遍共使用了28个时钟周期。
  - 有4个循环体，完成4个元素的操作。平均每个元素使用28/4=7个时钟周期
  - 原始循环的每个元素需要10个时钟周期。节省的时间：从减少循环控制的开销中获得的。
  - 在整个展开后的循环中，实际指令只有14条，其他14个周期都是空转。
- 对指令序列进行优化调度，以减少空转周期：

![对指令序列进行优化调度，以减少空转周期](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1589785055/notepad/2020-05-18_145257_qfnals.webp)

*注: 对指令序列进行优化调度, 把相关指令的距离拉开, 以减少空转周期.*

- **结果分析：**
  - 没有数据相关引起的空转等待。
    - 整个循环仅仅使用了`14`个时钟周期。
    - 平均每个元素的操作使用`14/4=3.5`个时钟周期。
  - **结论**：通过循环展开、寄存器重命名和指令调度，可以有效地开发出指令级并行。
- **循环展开和指令调度时要注意以下几个方面：**
- 保证正确性。
  - 在循环展开和调度过程中尤其要注意两个地方的正确性：\
  **循环控制** 和 **操作数偏移量的修改**。
- 注意有效性。
  - 只有能够找到不同循环体之间的无关性，才能有效地使用循环展开。
- 使用不同的寄存器,否则可能导致新的冲突。
- 注意对存储器数据的相关性分析。
- 注意新的相关性。
  - 由于原循环不同次的迭代在展开后都到了同一次循环体中，因此可能带来新的相关性。
