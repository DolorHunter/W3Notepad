# 12. Main Memory

## The Main Memory System

![The Main Memory System](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587111/notepad/2020-06-08_104817_emi7r3.webp)

- *Main memory is a critical component of all computing systems*: server, mobile, embedded, desktop, sensor
- *Main memory system must scale* (in size, technology, efficiency, cost, and management algorithms) to match the growing demands of bandwidths

## Memory System: A Shared Resource View

![Memory System: A Shared Resource View](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587111/notepad/2020-06-08_105038_mwbqsx.webp)

## State of the Main Memory System

- Recent technology, architecture, and application trends
  - *lead to new requirements*
  - *exacerbate old requirements*
- `DRAM and memory controllers`, as we know them today, are (will be) `unlikely to satisfy all requirements`
- Some `emerging non-volatile memory technologies` (e.g., PCM) `enable new opportunities`: memory + storage merging
- *We need to rethink/reinvent the main memory system*
  - *to fix DRAM issues and enable emerging technologies*
  - *to satisfy all requirements*

## Major Trends Affecting Main Memory

- *Need for main memory capacity, bandwidth, QoS increasing*
  - `Multi-core`: increasing number of cores
  - `Data-intensive applications`: increasing demand for data
  - `Consolidation`: Cloud computing, GPUs, mobile, heterogeneity
- *Main memory energy/power is a key system design concern*
  - `IBM servers: ~50% energy spent in off-chip memory hierarchy` [Lefurgy, IEEE Computer 2003]
  - `DRAM consumes power when idle and needs periodic refresh`
- *DRAM technology scaling is ending*

### Demand for Memory Capacity

- *More cores -> More concurrency -> Larger working set*

![Demand for Memory Capacity](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587111/notepad/2020-06-08_105557_jg9k5e.webp)

- *Modern applications are (increasingly) data-intensive*
- *Many applications/virtual machines (will) share main memory*
  - `Cloud computing/servers`: Consolidation to improve efficiency
  - `GP-GPUs`: Many threads from multiple parallel applications
  - `Mobile`: Interactive + non-interactive consolidation

### Example: The Memory Capacity Gap

Core count doubling ~ every 2 years

DRAM DIMM capacity doubling ~ every 3 years

![The Memory Capacity Gap](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587111/notepad/2020-06-08_105712_xngyjt.webp)

- *Memory capacity per core expected to drop by 30% every two years*
- *Trends worse for memory bandwidth per core!*

### The DRAM Scaling Problem

- *DRAM stores charge in a capacitor (charge-based memory)*
  - Capacitor must be large enough for reliable sensing
  - Access transistor should be large enough for low leakage and high
retention time
  - Scaling beyond 40-35nm (2013) is challenging [ITRS, 2009]

  ![DRAM](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587110/notepad/2020-06-08_110147_nfkmgn.webp)

- *DRAM capacity, cost, and energy/power hard to scale*

### Evidence of the DRAM Scaling Problem

![Evidence of the DRAM Scaling Problem](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587116/notepad/2020-06-08_110418_jmwqjq.webp)

Repeatedly opening and closing a row enough times within a refresh interval induces `disturbance errors` in adjacent rows in most real DRAM chips you can buy today

Kim+, “*Flipping Bits in Memory Without Accessing Them: An Experimental Study of DRAM Disturbance Errors*,” ISCA 2014.

![Most DRAM Modules Are At Risk](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587116/notepad/2020-06-08_110726_hprmd2.webp)

Observed Errors in Real Systems

CPU Architecture | Errors | Access-Rate
-|-|-
Intel Haswell (2013) | `22.9K` | 12.3M/sec
Intel Ivy Bridge (2012) | `20.7K` | 11.7M/sec
Intel Sandy Bridge (2011) | `16.1K` | 11.6M/sec
AMD Piledriver (2012) | `59` | 6.1M/sec

- `A real reliability & security issue`
- In a more controlled environment, we can induce as many as `ten million` disturbance errors

## DRAM Subsystem Organization

![DRAM Subsystem Organization](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587115/notepad/2020-06-08_111039_egwxnh.webp)

### Page Mode DRAM

- A DRAM bank is a 2D array of cells: rows x columns
- A “DRAM row” is also called a “DRAM page”
- “Sense amplifiers” also called “row buffer”
- Each address is a {row,column} pair
- Access to a “closed row”
  - `Activate` command opens row (placed into row buffer)
  - `Read/write` command reads/writes column in the row buffer
  - `Precharge` command closes the row and prepares the bank for next access
- Access to an “open row”
  - No need for an activate command

### DRAM Bank Operation

![DRAM Bank Operation](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587116/notepad/2020-06-08_111225_uwzxct.webp)

*注: 一个 bank 就是一个二维阵列. 如果数据不在 buffer 内, 需要通过 Row decoder 选择行, 破坏性读(DRAM)到 Row Buffer 中. 通过列译码得到需要的 Data 把数据传出.*

### The DRAM Chip

- Consists of multiple banks (8 is a common number today)
- `Banks share command/address/data buses`
- The chip itself has a narrow interface (4-16 bits per read)
- Changing the number of banks, size of the interface (pins), whether or not command/address/data buses are shared has significant impact on DRAM system cost

### 128M x 8-bit DRAM Chip

![DRAM Chip](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587116/notepad/2020-06-08_111722_wda08f.webp)

### DRAM Rank and Module

- *Rank: Multiple chips operated together to form a wide interface*
- All chips comprising a rank are controlled at the same time
  - Respond to a single command
  - `Share address and command buses, but provide different data`
- A DRAM module consists of one or more ranks
  - E.g., DIMM (dual inline memory module)
  - This is what you plug into your motherboard

### A 64-bit Wide DIMM (One Rank)

![Wide DIMM (One Rank)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587122/notepad/2020-06-08_112123_rtrh03.webp)

![Wide DIMM (One Rank)](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587123/notepad/2020-06-08_112229_dqlxdk.webp)

- Advantages:
  - Acts like a `high-capacity DRAM chip` with a `wide interface`
  - `Flexibility`: memory controller does not need to deal with individual chips
- Disadvantages:
  - `Granularity`: Accesses cannot be smaller than the interface width

### Generalized Memory Structure

![Memory Structure](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587119/notepad/2020-06-08_112420_szz6un.webp)

![Memory Structure](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587120/notepad/2020-06-08_112505_vyi8e8.webp)

The DRAM subsystem

![The DRAM subsystem](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587121/notepad/2020-06-08_112550_cpbfl3.webp)

Breaking down a DIMM

![Breaking down a DIMM](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587134/notepad/2020-06-08_112642_im03vk.webp)

Rank

![Rank](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587134/notepad/2020-06-08_112805_df2wgb.webp)

Breaking down a Rank

![Breaking down a Rank](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587134/notepad/2020-06-08_112844_lo6cnh.webp)

## Example: Transferring a cache block

![Transferring a cache block](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587597/notepad/2020-06-08_113642_q2tilb.webp)

![Transferring a cache block](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587597/notepad/2020-06-08_113807_mz8ncb.webp)

![Transferring a cache block](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591587597/notepad/2020-06-08_113905_keh4uz.webp)
