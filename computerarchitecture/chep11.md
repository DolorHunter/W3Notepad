# 11. Optimizing Cache Performance

## Basic Tricks to Improve Performance

- Reducing miss rate
  - More associativity
  - Alternatives/enhancements to associativity
    - Victim caches, hashing, pseudo-associativity, skewed associativity
  - Better replacement/insertion policies
  - Software approaches
- Reducing miss latency/cost
  - Multi-level caches
  - Critical word first
  - Subblocking/sectoring
  - Better replacement/insertion policies
  - Non-blocking caches (multiple cache misses in parallel)
  - Issues in multicore caches

## Ways of Reducing Conflict Misses

- Instead of building highly-associative caches, many other approaches in the literature:
  - Victim Caches
  - Hashed/randomized Index Functions
  - Pseudo Associativity
  - Skewed Associative Caches
  - …

## Victim Cache: Reducing Conflict Misses

![Victim Cache](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419648/notepad/2020-06-06_121018_pdo0fz.webp)

*注: 加入了一个 Victim Cache. 拥有 4 或 8 个条目用于存储被 Cache 踢出的块(避免高几率访问块反复踢出/调用. e.g.ping-pong effect: ABABABAB).*

- Jouppi, “`Improving Direct-Mapped Cache Performance by the
Addition of a Small Fully-Associative Cache and Prefetch Buffers`,”
ISCA 1990. 【想深度理解，请阅读原文。 】
- Idea: *Use a small fully associative buffer (victim cache) to store
evicted blocks*
  - +Can avoid ping ponging of cache blocks mapped to the same set (if two cache
blocks continuously accessed in nearby time conflict with each other)
  - --Increases miss latency if accessed serially with L2;
  - --Adds complexity

## Hashing and Pseudo-Associativity

- Hashing: Use better “randomizing” index functions
  - +can reduce conflict misses
    - by distributing the accessed memory blocks more evenly to sets
    - Example of conflicting accesses: stride access pattern where stride value equals number of sets in cache
  - --More complex to implement: can lengthen critical path
- Pseudo-associativity(伪相连)
  - View each "way" of the set as a separate direct-mapped cache
  - Ways are searched in sequence (first, second, …)
    - On a hit in the kth way, then the line is promoted to the first way, and all lines in caches 1 to k-1 get demoted by one.
    - On a miss, the item is filled in the first way, the item in the nth way is evicted, and all items in the 1 to n-1 way

## Skewed Associative Caches

- Idea: Reduce conflict misses by using *different index functions for each cache way*
- Seznec, “*A Case for Two-Way Skewed-Associative(倾斜相连) Caches*,” ISCA 1993.

Basic 2-way associative cache structure

![Basic 2-way associative cache structure](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419648/notepad/2020-06-06_122259_p0mw6h.webp)

*注: Way0 和 Way1 一一对应.*

Each bank has a different index function(哈希函数)

![Each bank has a different index function](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419648/notepad/2020-06-06_122447_ry6aez.webp)

*注: Way0 和 Way1 使用不同哈希函数, 组的概念是动态的, 更加随机均衡.*

- Idea: Reduce conflict misses by using *different index functions for each cache way*
- Benefit: indices are more randomized (memory blocks are better distributed across sets) due to hashing
  - Less likely two blocks have same index
    - Reduced conflict misses
- Cost: additional latency of hash function

## Example Software Approaches

- Restructuring data access patterns
- Restructuring data layout
  - Loop interchange
  - Data structure separation/merging
  - Blocking
  - …

### Restructuring Data Access Patterns

- Idea: Restructure data layout or access patterns
- Example: If column-major
  - x[i+1,j] follows x[i,j] in memory
  - x[i,j+1] is far away from x[i,j]

  ```c
  // Poor code
  for i = 1, rows
    for j = 1, columns
      sum = sum + x[i, j]
  ```

  ```c
  // Better code
  for j = 1, columns
    for i = 1, rows
      sum = sum + x[i, j]
  ```

  *注: Poor code 按行访问, 不符合列式优先, 效率很低对缓存不友好.*

- This is called *loop interchange*
- Other optimizations can also increase hit rate
  - Loop fusion, array merging, … 【请看张晨曦老师的教材】
- What if multiple arrays? Unknown array size at compile time?

- Blocking for better cache utilization 【示例见教材】
  - Divide loops operating on arrays into computation chunks so that each chunk can hold its data in the cache
  - Avoids cache conflicts between different chunks of computation
  - Essentially: Divide the working set so that each piece fits in the cache
- But, there are still self-conflicts in a block
  1. there can be conflicts among different arrays
  2. array sizes may be unknown at compile/programming time

### Restructuring Data Layout

```c
struct Node {
  struct Node* node;
  int key;
  char [256] name;
  char [256] school;
}

while (node) {
  if (node->key == input-key) {
    // access other fields of node
  }
  node = node->next;
}
```

- Pointer based traversal (e.g., of a linked list)
- Assume a huge linked list (1M nodes) and unique keys
- `Why does the code on the left have poor cache hit rate?`
  - “Other fields” occupy most of the cache line even though rarely accessed!\
  *注: Cache 中有效的只有 Node 部分, 其他部分的数据是无效的.*

```c
struct Node {
  struct Node* node;
  int key;
  struct Node-data* node-data;
}
struct Node-data {
  char [256] name;
  char [256] school;
}
while (node) {
  if (node->key == input-key) {
    // access node->node-data
  }
  node = node->next;
}
```

- Idea: `separate frequentlyused fields of a data structure and pack them into a separate data structure`
- Who should do this?
  - Programmer
  - Compiler
- Profiling vs. dynamic
  - Hardware?
  - `Who can determine what is frequently used?`

## Improving Basic Cache Performance

- Reducing miss rate
  - More associativity
  - Alternatives/enhancements to associativity
- Victim caches, hashing, pseudo-associativity, skewed associativity
  - Better replacement/insertion policies
  - Software approaches
- Reducing miss latency/cost
  - Multi-level caches
  - Critical word first
  - Subblocking/sectoring
  - Better replacement/insertion policies 【若感兴趣，请联系我】
  - Non-blocking caches (multiple cache misses in parallel)
  - Issues in multicore caches

## Miss Latency/Cost

- What is miss latency or miss cost affected by?
  - `Where does the miss get serviced from?`
    - Local vs. remote memory
    - What level of cache in the hierarchy?
    - Row hit versus row miss
    - Queueing delays in the memory controller
    - …
  - `How much does the miss stall the processor?`
    - Is it overlapped with other latencies?
    - Is the data immediately needed?
    - …

## Demo: Memory Level Parallelism (MLP)

![MLP](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419648/notepad/2020-06-06_124450_jc4vix.webp)

*注: B 和 C 是不同的两个缺失, 可以同时处理. 相比 A 效率更佳.*

- Memory Level Parallelism (MLP) means generating and servicing multiple memory accesses in parallel [Glew’98]
- Several techniques to improve MLP (e.g., out-of-order execution)
- `MLP varies`. Some misses are isolated and some parallel
- How does this affect cache replacement?

## Traditional Cache Replacement Policies

- Traditional cache replacement policies try to reduce miss count
- Implicit assumption: Reducing miss count reduces memory-related stall time
- Misses with varying cost/MLP breaks this assumption!
  - `Eliminating an isolated miss helps performance more than eliminating a parallel miss`
  - `Eliminating a higher-latency miss could help performance more than eliminating a lower-latency miss`

### An Example

![Traditional Cache Replacement Policies](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419648/notepad/2020-06-06_124839_bqehuu.webp)

Misses to blocks P1, P2, P3, P4 can be parallel

Misses to blocks S1, S2, and S3 are isolated

Two replacement algorithms:

1. Minimizes miss count (Belady’s OPT)
2. Reduces isolated miss (MLP-Aware)

For a fully associative cache containing 4 blocks

![Traditional Cache Replacement Policies](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419655/notepad/2020-06-06_125122_jikr8e.webp)

Fewest Misses != Best Performance

## MLP-Aware Cache Replacement

- How do we incorporate MLP into replacement decisions?
- Qureshi et al., “*A Case for MLP-Aware Cache Replacement*,” ISCA 2006.
  - Suggested reading for homework

## Enabling Multiple Outstanding Misses

### Handling Multiple Outstanding Accesses

- Question: If the processor can generate multiple cache accesses, can the later accesses be handled while a previous miss is outstanding?
- Goal I: Enable cache access when there is a pending miss
- Goal II: Enable multiple misses in parallel
  - *Memory-level parallelism (MLP)*
- Solution: *Non-blocking* or *lockup-free* caches
  - Kroft, “*Lockup-Free Instruction Fetch/Prefetch Cache Organization*," ISCA 1981.

- Idea: *Keep track of the status/data of misses that are being handled using Miss Status Handling Registers (MSHR)*
  - A cache access checks MSHR to see if a miss to the same block is already pending.
    - If pending, a new request is `not` generated
    - If pending and the needed data available, data forwarded to later load
  - Requires buffering of outstanding miss requests

### Miss Status Handling Register

- Also called “miss buffer”
- Keeps track of
  - Outstanding cache misses
  - Pending load/store accesses that refer to the missing cache block
- Fields of a single MSHR entry
  - Valid bit
  - Cache block address (to match incoming accesses)
  - Control/status bits (prefetch, issued to memory, which subblocks have arrived, etc)
  - Data for each sub-block
  - For each pending load/store， keep track of
    - Valid, type, data size, byte in block, destination register or store buffer entry address

### MSHR Entry Demo

![MSHR Entry Demo](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591419655/notepad/2020-06-06_125819_cexgfk.webp)

### MSHR Operation

- On a cache miss:
  - Search MSHRs for a pending access to the same block
    - Found: Allocate a load/store entry in the same MSHR entry
    - Not found: Allocate a new MSHR
    - `No free entry: stall   - structure hazard`
- When a sub-block returns from the next level in memory
  - Check which loads/stores waiting for it
    - Forward data to the load/store unit
    - Deallocate load/store entry in the MSHR entry
  - Write sub-block in cache or MSHR
  - If last sub-block, deallaocate MSHR (after writing the block in cache)

### Non-Blocking Cache Implementation

- When to access the MSHR?
  - In parallel with the cache?
  - After cache access is complete?
- MSHR need not be on the critical path of hit requests
  - Which one below is the common case?
- Cache miss, MSHR hit
- Cache hit

## Multi-Core Issues in Caching

### Caches in Multi-Core Systems

- Cache efficiency becomes even more important in a multi-core/multi-threaded system
  - *Memory bandwidth is at premium*
  - *Cache space is a limited resource*
- How do we design the caches in a multi-core system?
- Many decisions
  - Shared vs. private caches
  - How to maximize performance of the entire system?
  - How to provide QoS to different threads in a shared cache?
  - Should cache management algorithms be aware of threads?
  - How should space be allocated to threads in a shared cache?

### Private vs. Shared Caches

- *Private* cache: Cache belongs to one core (a shared block can be in multiple caches)
- *Shared* cache: Cache is shared by multiple cores

![Private vs. Shared Caches](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591428536/notepad/2020-06-06_152515_srdcum.webp)

### Resource Sharing and Its Advantages

- Idea: *Instead of dedicating a hardware resource to a hardware context, allow multiple contexts to use it*
  - Example resources: functional units, pipeline, caches, buses, memory
- Why?
  - +Resource sharing improves utilization/efficiency -> throughput
    - When a resource is left idle by one thread, another thread can use it; no need to replicate shared data
  - +Reduces communication latency
    - For example, shared data kept in the same cache in multithreaded processors
  - +Compatible with the **shared memory model**

### Resource Sharing Disadvantages

- Resource sharing results in contention for resources
  - When the resource is not idle, another thread cannot use it
  - If space is occupied by one thread, another thread needs to re-occupy it
- Sometimes reduces each or some thread’s performance
  - Thread performance can be worse than when it is run alone
- Eliminates performance isolation -> **inconsistent performance across runs 【实时系统】**
  - Thread performance depends on co-executing threads
- Uncontrolled (free-for-all) sharing degrades QoS
  - Causes unfairness, starvation
  - `Need to efficiently and fairly utilize shared resources`
  - `This is crucial for modern data center`

### Shared Caches Between Cores

- Advantages:
  - High effective capacity
  - *Dynamic partitioning* of available cache space
- No fragmentation due to static partitioning
  - *Easier to maintain coherence (single copy)*
  - *Shared data and locks do not ping pong between caches*
- Disadvantages
  - Slower access
  - Cores incur *conflict misses due to other cores’ accesses*
- Misses due to inter-core interference
- Some cores can destroy the hit rate of other cores
  - Guaranteeing a minimum level of service (or fairness) to each
core is harder (how much space, how much bandwidth?)

### Shared Caches: How to Share

- Free-for-all sharing
  - Placement/replacement policies are the same as a single core system (usually LRU or pseudo-LRU)
  - Not thread/application aware
  - An incoming block evicts a block regardless of which threads the blocks belong to
- Problems
  - Inefficient utilization of cache: LRU is not the best policy
  - A cache-unfriendly application can destroy the performance of a cache friendly application
  - Not all applications benefit equally from the same amount of cache: free-for-all might prioritize those that do not benefit
  - Reduced performance, reduced fairness

### Optimization: Utility Based Cache Partitioning

- Goal: Maximize system throughput
- Observation: Not all threads/applications benefit equally from caching -> simple LRU replacement not good for system throughput
- Idea: *Allocate more cache space to applications that obtain the most benefit from more space*
- The high-level idea can be applied to other shared resources as well.
- Qureshi and Patt, “*Utility-Based Cache Partitioning: A Low-Overhead, HighPerformance, Runtime Mechanism to Partition Shared Caches*,” MICRO 2006.
- Suh et al., “*A New Memory Monitoring Scheme for Memory-Aware Scheduling and Partitioning*,” HPCA 2002

### The Multi-Core System: A Shared Resource View

![The Multi-Core System](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1591428536/notepad/2020-06-06_152815_h9tez5.webp)

### Need for QoS and Shared Resource Mgmt

- Why is unpredictable performance (or lack of QoS) bad? (`think about it!`)
- Makes programmer’s life difficult
  - An optimized program can get low performance (and performance varies widely depending on co-runners)
- Causes discomfort to user
  - An important program can starve
  - Examples from shared software resources
- Makes system management difficult
  - How do we enforce a Service Level Agreement when hardware resources are sharing is uncontrollable?

### Resource Sharing vs. Partitioning

- Sharing improves throughput
  - Better utilization of space
- Partitioning provides performance isolation (predictable performance)
  - Dedicated space
- Can we get the benefits of both?
- Idea: Design shared resources such that they are efficiently utilized, controllable and partitionable
  - *No wasted resource + QoS mechanisms for threads*

### Shared Hardware Resources

- Memory subsystem (in both multithreaded and multicore systems)
  - Non-private caches
  - Interconnects
  - Memory controllers, buses, banks
- I/O subsystem (in both multithreaded and multi-core
systems)
  - I/O, DMA controllers
  - Ethernet controllers
- Processor (**in multithreaded systems**)
  - Pipeline resources
  - L1 caches
