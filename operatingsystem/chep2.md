# 第二章 进程管理

## 2.1 进程的基本概念

`前驱图：` 用于描述实体\(进程, 程序段\)之间执行次序的DAG\(有向无环图\)

`结点 ：` 代表一条语句，一段程序或一个进程

`有向边：` 两结点之间存在的先后次序关系

进程执行顺序

> [**顺序执行**](chep2.md#顺序执行)
>
> [**并发执行**](chep2.md#并发执行)

### 顺序执行

特征:

* 顺序性

  处理机严格按照程序规定的顺序执行程序

* 封闭性

  程序独占全机资源，不受其他程序的影响

* 可再现性

  程序执行结果 = 初始条件 + 执行时的环境

问题:

* 整体资源利用率低
* 单位时间内算题量少

### 并发执行

基本概念：多道程序设计技术

特征:

* 间断性

  由于资源的共享，程序执行具有"执行-暂停-执行"的特点

* 失去封闭性

  程序不再独占全机资源，运行受到其他程序的影响

* 不可再现性

  程序执行结果受其他程序的影响，结果不定

问题:

程序B因为受到程序A的影响，运行结果出现了不可再现性，即结果不定

### 进程概念

定义: 进程实体\(程序段, 数据段, 进程控制块\)的 `运行` 过程，是系统进行 `资源分配和调度的独立单位`。

特征:

* 动态性

  是程序的一次执行过程，因而是动态的。

* 并发性

  引入进程就是为了和其他程序并发执行，提高资源利用率。

* 独立性

  能独立运行的基本单位，也是资源分配和调度的基本单位。

* 异步性

  进程以各自独立的、不可预知的速度向前推进。

* 结构特征

  程序段

  数据段

  进程控制块（PCB）

进程的三种基本状态:

1. 就绪状态: 进程已经获得除处理机之外的所有资源，一旦获得处理机就可以立即执行
2. 执行状态: 一个进程已获得了必要的资源，正在处理机上运行
3. 阻塞状态: 正在执行的进程，由于发生某事件而暂停无法继续执行，（如等待I/O、时间片到）

进程控制块

* 内容:
  * 进程标志符
  * 处理机状态
  * 进程调度信息
  * 进程控制信息
* 组织形式:
  * 链接方式
  * 索引方式

## 2.2 进程控制

> [**进程的创建**](chep2.md#进程的创建)
>
> [**进程的撤销**](chep2.md#进程的撤销)
>
> [**进程的阻塞**](chep2.md#进程的阻塞)
>
> [**进程的唤醒**](chep2.md#进程的唤醒)
>
> [**进程的挂起**](chep2.md#进程的挂起)
>
> [**进程的激活**](chep2.md#进程的激活)

### 进程的创建

* 引起进程创建的事件
  * 用户登录
  * 作业调度
  * 提供服务

    例如：提供打印服务

  * 应用请求

    例如：程序自行创建
* 进程创建原语

  create\(\)

* 进程创建流程
* 进程树

### 进程的撤销

* 引起进程撤消的事件
  * 正常结束
  * 异常结束
  * 外界干预
* 进程撤消原语

  exit\(\)

### 进程的阻塞

* 引起进程阻塞的事件
  * 请求系统服务
  * 请求操作
  * 等待数据、资源
  * 等待工作任务
* 进程阻塞原语 block\(\)

`进程自己阻塞自己`

### 进程的唤醒

* 引起进程唤醒的事件
  * 允许系统服务
  * 允许操作
  * 数据、资源到达
  * 工作任务到达
* 进程唤醒原语 wakeup\(\)

`被别的进程唤醒`

### 进程的挂起

* 引起进程挂起的事件
  * 调试系统
  * 暂停程序执行
* 进程挂起原语

  suspend\(\)

### 进程的激活

* 引起进程激活的事件
  * 调试系统结束
  * 程序继续执行
* 进程激活原语

  activate\(\)

## 2.3 进程同步

### 进程同步基本概念

#### 1. 什么是进程同步

* 进程同步问题的产生

在多道程序的环境中，系统中的多个进程可以并发执行，同时它们又要共享系统中的资源，这些资源有些是可共享使用的，如磁盘，有些是以独占方式使用的，如打印机。由此将会引起一系列的矛盾，产生错综复杂的相互制约的关系。

* 两种形式的制约关系：
  * 间接相互制约关系：共享某种系统资源。
  * 直接相互制约关系：主要源于进程间合作。

#### 2. 临界资源

* 互斥共享的资源：临界资源

  虽然计算机系统中多个进程可以共享系统中的各种资源，然而有些资源一次只能为一个进程使用。`一次仅能为一个进程所使用的资源称为临界资源。`

* 临界资源例子

  硬件资源：打印机；

  软件资源：内存变量、指针、数组。

#### 3. 临界区

* 临界区的定义:

  进程中访问临界资源的代码段称为临界区。

* 策略:

  临界资源访问互斥 =&gt; 临界区访问互斥

#### 4. 临界区访问控制模型

```text
repeat
  `entry section;` // 进入区
    critical section ;  // 临界区
  `exit section ;` // 退出区
    remainder section ;  // 临界区
until false
```

#### 5. 同步机制遵信原则

* `空闲让进`：当 **没有** 进程处于临界区时，请求进入临界区的进程立即进入
* `忙则等待`：当 **已有** 进程进入临界区时，其他试图进入临界区进程须等待
* 有限等待：对要求访问临界资源进程，保证能在有限时间内进入临界区
* 让权等待：当进程不能进入临界区时，应释放处理机

前两个 `互斥访问` 实现必须, 后两个为优化

### 实现进程同步的早期方法

#### 1. 软件解法1: 按需访问

违背`忙则等待`原则

_解释:_ 不是原语, bsuy:=true前切换资源

```text
repeat
  while(busy);  // busy=true, 空循环等待
  busy:=true;  // 不是原语, busy:=true前切换资源
    critical section;
  busy:=false;
    remainder section;
until false
```

#### 2. 软件解法2：轮询

违背`空闲让进`原则, 严格限制资源访问顺序

_解释:_ P2 先拥有资源时, 不能立即进入临界区.

```text
VAR turn:integer:=1;
P1:
repeat
  while(turn=2);  // 空循环, 等待turn!=2
    critiacl section;
  turn:=2;
    remainder section;
until false

P1:
repeat
  while(turn=1);  // 空循环, 等待turn!=1
    critiacl section;
  turn:=2;
    remainder section;
until false
```

#### 3. 软件解法3：访前先看

违背`空闲让进`

_解释:_ 类似1, 不是原语. 可能会发生进程切换, 导致 P, Q 都被堵住.

```text
VAR pturn,qturn:boolean:=false,false;
P:
repeat
  pturn:=true;
  while(qturn);
    critical section;
  pturn:=false;
    remainder section;
until false

Q:
repeat
  qturn:=true;
  while(pturn);
    critical section;
  qturn:=false;
    remainder section;
until false
```

#### 4. 软件解法4：Peterson算法 1981

```text
P1:
repeat
  enter_region(0);
    critical section;
  leave_region(0);
    remainder section;
until false

P2:
repeat
  enter_region(1);
    critical section;
  leave_region(1);
    remainder section;
until false
```

```text
#define FALSE 0
#define TRUE 1
#define N 2  // 进程的个数
int turn;  // 轮到谁？
int interested[N];  // 兴趣数组，初始值均为FALSE
void enter_region ( int process)  // process = 0 或 1
{
  int other;  // 另外一个进程的进程号
  other = 1  - process;
  interested[process] = TRUE;  // 表明本进程感兴趣
  turn = process;  // 设置标志位
  while( `turn == process` && interested[other] == TRUE);  // 判断标志位, 防止进程切换
}
void leave_region ( int process)
{
  interested[process] = FALSE;  // 本进程已离开临界区
}
```

#### 6. 硬件解法2：SWAP指令

```text
VAR lock :boolean:=false;
P1:
repeat
  enter_region(lock);
    critical section;
  leave_region(lock);
    remainder section;
until false

// P2 与 P1 一致
```

```text
// 交换锁lock和key的值
function SWAP(lock, key ) {
var tmp : boolean := lock ;
lock := key ;
key := tmp ;
}

function enter_region( var lock : boolean )
Var key:boolean ; //局部变量
Begin
  key := true;
  while(key)
    SWAP(lock,key);
end

function leave_region( var lock : boolean )
Begin
  lock := false;
end
```

#### 7. 小结

软件解法

* `忙等待`
* 实现过于复杂，需要高的编程技巧

硬件解法

* 简单、有效，特别适用于多处理机
* 缺点：`忙等待`

### 信号量机制

提出者: Dijkstra

荷兰计算机科学家Edsger Wybe Dijkstra

1972年ACM 图灵奖获得者

#### 1. 整数型信号量，Dijkstra,1965

定义

* 记录型信号量S;
* 两个 `原子操作`：wait/signal\( P/V \)

原则：

* 除初始化外，S只能由wait/signal访问

```text
repeat
  wait(S);
    critical section;
  signal(S);
    remainder section;
until false
```

```text
int S = 1 ;
function Wait ( var S: integer )
Begin
  while ( S <= 0 ) do no_op();
  S := S-1;
end

function Signal( var S : integer )
Begin
  S := S+1;
end
```

#### 2. `记录型信号量`

定义

* 记录型信号量S;
* 两个 `原子操作`：wait/signal\( P/V \)

原则：

* 除初始化外，S只能由wait/signal访问

```text
repeat
  wait(S);
    critical section;
  signal(S);
    remainder section;
until false
```

```text
type semaphore = record
value: integer;  // 资源数量
L: list of process ;  // 阻塞进程队列
end

function wait ( var S ：Semaphore )
Begin
  S.value := S.value-1;
  if ( S.value < 0 ) then block (S.L);
end
function signal( var S ：Semaphore )
Begin
  S.value := S.value+1;
  if ( S.value <= 0 ) then wackup(S.L);
end
```

`信号量S的物理含义`

* 对信号量的每次wait操作，意味着进程请求一个单位的该类资源，因此描述为S.value:= S.value-1；当S.value＜0时，表示该类资源已分配完毕，因此进程应进行自我阻塞，放弃处理机，并插入到信号量链表S.L中。
* 对信号量的每次signal操作，表示执行进程释放一个单位资源，故S.value:= S.value+1操作表示资源数目加1。若加1后仍是S.value＜=0，则表示在该信号量链表中，仍有等待该资源的进程被阻塞，将S.L链表中的第一个等待进程唤醒。
* S.value:
  * S.value&gt;=0: 资源数量；
  * S.value&lt;0 : \|S.value\|为阻塞进程的数量；

### 信号量的应用

#### 1. 信号量用于互斥

例1：2个进程进行堆栈的入栈

```text
VAR top: integer=-1
stack: array[0..n-1] of item;
mutex: semaphore :=1;  // 同时允许1个进程
P1:
while(True)
  wait(mutex);
    top:=(top+1)%n;
    stack[top]:=x;
  signal(mutex);
end while

P2:
while(True)
  wait(mutex);
    top:=(top+1)%n;
    stack[top]:=y;
  signal(mutex);
end while
```

_注:_ 互斥锁 Mutual exclusion，缩写Mutex

#### 2. 信号量用于同步

例3：PI,PO两进程共享缓冲区B,PI负责读入数据并保存在B中，PO负责打印B中数据，要求每个数据必须且只打印一次，不许漏打或重复打印。

```text
VAR S1,S2:semaphore=1,0;
PI:
repeat
  read x from I/O;
  wait(S1);
    B:=x;
  signal(S2);
until false;

PO:
repeat
  wait(S2);
    y:=B;
  signal(S1);
    print(y);
until false;
```

例4：PI,PE,PO三进程通过共享缓冲区B协作,PI负责产生随机数并保存在B中，当B中数据为奇数时由PO负责打印，当B中数据为偶数时由PE负责打印，要求每个数据必须且只打印一次，不许漏打或重复打印。请用信号量进行同步。

```text
var S,SO,SE:semaphore:=1,0,0;
PI:
repeat
  generate x randomly;
  wait(S);
    B := x;
  if ( x is odd ) signal( SO );
  else signal( SE );
until false

PE:
repeat
  wait(SE);
    y := B;
  signal( S );
    print( y);
until false

PO:
repeat
  wait(SO);
    z := B;
  signal(S);
    print(z);
until false
```

## 2.4 经典进程同步问题

> 生产者-消费者问题
>
> 读者-写者问题
>
> 哲学家进餐问题

### 生产者-消费者问题

多个生产者、多个消费者通过共享含n个缓冲区的缓冲池Buffer协作,其中生产者负责生产数据并投入缓冲池，消费者从缓冲池中取数据消费，生产者和消费者，每次生产/消费1个数据，要求每个数据必须且只被消费一次。请用信号量进行同步。

```text
VAR mutex, empty, full: semaphore := 1, n, 0
  in,out: integer := 0, 0 ;
  Buffer: array [0..n-1] of item;
Parbegin
  Producer:
  begin
    repeat
      produce an item in nextp;
      wait( empty );  // 保护有限缓冲池
      wait( mutex );  // 保护多缓冲互斥
        // mutexP     // 保护读写互斥
      Buffer(in) := nextp;
      in := (in + 1 ) mod n;
      signal( mutex );  // 保护多缓冲互斥
          // mutexP     // 保护读写互斥
      signal( full );
    until flase
  end

  Consumer:
  begin
    repeat
      wait( full );
      wait( mutex );  // 保护多缓冲互斥
        // mutexC     // 保护读写互斥
      nextc = Buffer(out);
      out := (out + 1 ) mod n;
      signal( mutex );  // 保护多缓冲互斥
          // mutexC     // 保护读写互斥
      signal( empty );  // 保护有限缓冲池
      consume the item nextc;
    until flase
  end
Parend
```

### 读者-写者问题

多个读者、多个写者共享资源（例如文件、数据等），允许多个读者同时访问资源，写者写时不允许其他进程读或写（互斥访问资源）。请用记录型信号量进行同步。

```text
VAR rmutex,wmutex: semaphore := 1, 1 ;
    readcount: integer := 0 ;
Parbegin
  Reader:
  begin
    repeat
      wait( rmutex );
      if ( readcount = 0 ) wait( wmutex );  // 首读需先写
      readcount := readcount + 1;
      signal( rmutex );
      perform read operation;
      // 并发程序
      wait( rmutex );
      readcount := readcount - 1;
      if ( readcount = 0 ) signal( wmutex );
      signal( rmutex );
    until flase
  end

  Writer:
  begin
    repeat
      wait( wmutex );
      perform write operation;
      signal( wmutex );
    until flase
  end
Parend
```

### 哲学家进餐问题

5位哲学家围绕圆桌而坐，反复思考和进餐。但是只有5只碗和筷子，放置如图所示，只有当哲学家同时拿起碗边的2只筷子时，才能进餐。请用记录型信号量进行同步。

```text
VAR chopsticks: array [0..4] of semaphore := 1,1,1,1,1 ;
Parbegin
  philosopher0-3:
  begin
    repeat
      wait( chopsticks[ i ] );
      wait( chopsticks[ (i+1) mod 5 ] );
      eat;
      signal( chopsticks[ i ] ) ;
      signal( chopsticks[ (i+1) mod 5 ] );
      think;
    until false
  end

  philosopher4:
  begin
    repeat
      wait( chopsticks[ (i+1) mod 5 ] );
      wait( chopsticks[ i ] );
      eat ;
      signal( chopsticks[ i ] ) ;
      signal( chopsticks[ (i+1) mod 5 ] );
      think;
    until false
  end
Parend
```

#### 信号量集机制

> [**AND型信号量集机制**](chep2.md#AND型信号量集机制)
>
> [**一般信号量集机制**](chep2.md#一般信号量集机制)

**AND型信号量集机制**

基本思想：

* 将进程在整个运行过程中需要的所有资源，`一次性全部地分配` 给进程，待进程使用完后再一起释放。
* 对若干个临界资源的分配，采取原子操作方式要么全部分配到进程，要么一个也不分配。
* 为此，在wait操作中，增加了一个“AND”条件，故称为AND同步。

应用示例1：生产者消费者问题

```text
VAR mutex, empty, full: semaphore := 1, n, 0 ;
  in,out: integer := 0, 0 ;
  Buffer: array [0..n-1] of item ;
Parbegin
  Producer:
  begin
    repeat
      produce an item in nextp;
      Swait( empty, mutex );
      Buffer(in) := nextp;
      in := (in + 1 ) mod n;
      Ssignal( mutex, full );
    until false
  end

  Consumer:
  begin
    repeat
      Swait( full,mutex );
      nextc = Buffer(out);
      out := (out + 1 ) mod n;
      Ssignal( mutex,empty );
      consume the item nextc;
    until false
  end
Parend
```

应用示例2：哲学家进餐问题

```text
VAR chopsticks: array [0..4] of semaphore := 1,1,1,1,1 ;
Parbegin
philosopheri:
  begin
    repeat
      Swait( chopsticks[ i ], chopsticks[ (i+1) mod 5 ] ) ;
      eat ;
      Ssignal( chopsticks[ i ], chopsticks[ (i+1) mod 5 ] ) ;
      think;
    until false
  end
Parend
```

**一般信号量集机制**

基本思想：

* 在AND型信号量基础上，`一次可申请多个单位资源`。

```text
type semaphore = record
  value: integer ; // 资源数量
  L: list of process ; //  阻塞进程队列
end

function Swait ( var S1,t1,d1,S2,t2,d2..,Sn,tn,dn ：Semaphore )
Begin
  if ( S1>= t1 and … and Sn>= tn)
    for i := 1 to n do
      Si:= Si-di;
  else
    Place the process in the queue associated with the first Si found with Si< ti, andgoto the beginning of Swait.
end

function Ssignal(var S1, d1,S2,d2, .., Sn,dn ：Semaphore )
Begin
for i:=1 to n do
  Si:= Si+di
  Remove all process waiting in the queue associated with Si into the ready queue.
end
```

应用实例：读者写者问题

```text
VAR RN: integer;  // RN: 允许读者进程上限
  L,mutex: semaphore := RN, 1;
Parbegin
  Reader:
    begin
      repeat
      Swait( L,1,1, mutex,1,0);
      perform read operation;
      Ssignal( L, 1 );
      until false
    end
  Writer:
    begin
      repeat
      Swait( mutex,1,1, L,RN,0 );
      perform write operation;
      Ssignal( mutex,1 );
      until false
    end
Parend
```

## 2.5 进程通信

进程间通信的基本概念

Inter Process Communication, IPC 进程通信指进程之间的信息交换，其所交换的信息量，少者是一个状态或数值，多者则是成千上万个字节。

高级通信

* 通信链路：
  * 点对点/多点/广播
  * 单向/双向
  * 有容量（链路带缓冲区）/无容量（发送方和接收方需自备缓冲区）
* 数据格式：
  * 字节流：各次发送之间的分界，在接收时不被保留，没有格式
  * 报文：各次发送之间的分界，在接收时被保留，通常有格式（如表示类

    型），定长/不定长报文，可靠报文/不可靠报文
* 收发操作的同步方式
  * 发送阻塞（直到被链路容量或接收方所接受）和不阻塞（失败时立即返

    回）

  * 接收阻塞（直到有数据可读）和不阻塞（无数据时立即返回）
  * 由事件驱动收发：在允许发送或有数据可读时，才做发送和接收操作

> [**共享存储器系统**](chep2.md#共享存储器系统)
>
> [**消息通信系统**](chep2.md#消息通信系统)
>
> [**管道通信系统**](chep2.md#管道通信系统)

#### 共享存储器系统

基本思想

相互通信的进程共享数据结构或共享存储区，进程之间通过这些空间进行通信。

实现过程

1. 进程申请一片内存，拿到内存描述符；
2. 如果已分配过，直接获得内存描述符；
3. 利用内存描述符将内存连接到本进程；
4. 读写

共享存储区的访问同步: 共享存储区的资源共享问题

分类

* 基于共享数据结构的通信方式
* 基于共享存储区的通信方式

特点

* 适合大量的数据快速交换；
* 程序员负担重

### 消息通信系统

基本思想

进程间的数据交换，是以格式化的消息\(message\)为单位进行通信。

消息通信分类

直接通信：信息直接传递给接收方

* 在发送时，指定接收方的地址或标识，也可以指定多个接收方或广播式地址
* 在接收时，允许接收来自任意发送方的消息，并在读出消息的同时获取发送方的地址

间接通信：借助于收发双方进程之外的共享数据结构作为通信中转，如消息队列。

通常收方和发方的数目可以是任意的

\(1\) 直接通信方式

OS提供两个通信原语：

1. 发送进程：  Send\(receiver,msg\);
2. 接收进程：  Receive \(sender,msg\);

\(2\) 间接通信方式:信箱通信

1. 发送进程：Send\(mailbox,msg\);
2. 接收进程：Receive\(mailbox,msg\);

信箱分类

* 公有信箱；
* 私有信箱；
* 共享信箱；

特点

* 操作系统隐藏通信细节；
* 通信程序简单；
* 可以跨网络传输；
* 灵活

进程同步

* 发送阻塞、接收阻塞；
* 发送不阻塞、接收阻塞；
* 发送不阻塞、接收不阻塞；

\(3\) 消息缓冲队列通信 Hansan, RC4000

### 管道通信系统

基本思想

在磁盘创造一个文件, 大小固定，如4kB, 写连续字符流, 读连续字符流；

实现技术

* 对管道文件的资源共享：
* 同步、互斥
* UNIX首创

Windows操作系统的管道通信

* Widows server 2003提供无名管道和命名管道两种管道机制
* 利用CreatePipe可创建无名管道并得到两个读写句柄
* 利用ReadFile和WriteFile可并行无名管道的读写

## 2.6 线程

### 线程的基本概念

#### 1. 线程的引入

`线程是进程内的一个函数`

传统进程的两个基本属性

* 拥有资源的独立单位:

  每个进程分配一虚拟地址空间，保存进程映像，控制一些资源（文件，I/O设备），有状态、优先级

* 调度和分派的基本单位: 分配CPU

导致的不利后果

* 身形笨重，管理和运行资源开销大。
* 限制并发度，多道程序设计道数不能太高

传统进程的两个基本属性分解

* 拥有资源的独立单位 `进程`
* 调度和分派的基本单位 `线程`

线程概念

* 有时称轻量级进程
* 进程中的一个轻型运行实体
* 是一个 CPU 调度和分派的基本单位
* 可并发执行
* 共享进程资源

#### 2. 线程与进程的关系

#### 3. 线程的开销

* 线程控制块（TCB）
* 寄存器状态，堆栈，局部变量拷贝
* 线程运行状态（就绪、阻塞、执行）
* 可以创建、撤消另一个线程
* 优先级与信号屏蔽

#### 4. 引入线程的好处

* 创建一个新线程花费时间少（结束亦如此）
* 两个线程的切换花费时间少
* 同一进程内的线程共享内存和文件，因此它们之间相互通信无须调用内核
* 适合多处理机系统

### 线程的实现方式

* 内核支持线程

  Windows 2000/XP、Macintosh 和 OS/2 操作系统

* 用户级线程

  一些数据库管理系统如 infomix

* 两者结合方法

  Solaris、Linux

#### 1. 内核支持线程

* 内核支持线程，是在内核的支持下运行，即无论是用户进程中的线程，还是系统进程中的线程，他们的创建、撤消和切换等，也是依靠内核实现。
* 在内核空间还为每一个内核支持线程设置了一个线程控制块，内核是根据该控制块而感知某线程的存在的，并对其加以控制。
* 对于通常的进程，无论是系统进程还是用户进程，进程的创建、撒消，以及要求由系统设备完成的I/O操作，都是利用系统调用而进入内核，再由内核中的相应处理程序予以完成。进程的切换同样是在内核的支持下实现的。
* 若系统中设置的是内核支持线程，则以线程为单位进行调度。
* 在采用轮转法调度时，是各个线程轮流执行一个时间片。同样假定进程A中只有一个内核支持线程，而在进程B中有100个内核支持线程。此时进程B可以获得的CPU时间是进程A的100倍，且进程B可使100个系统调用并发工作。

优点：

* 对多处理器，核心可以同时调度同一进程的多个线程
* 阻塞是在线程一级完成
* 核心例程是多线程的

缺点：

* 在同一进程内的线程切换调用内核，导致速度下降

#### 2. 用户级线程

* 用户级线程仅存在于用户空间中。
* 对于这种线程的创建、撤消、线程之间的同步与通信等功能，都无须利用系统调用来实现。对于用户级线程的切换，通常是发生在一个应用进程的诸多线程之间，这时，也同样无须内核的支持。
* 可以为一个应用程序建立多个用户级线程。在一个系统中的用户级线程的数目，可以达到数百个至数千个由于这些线程的任务控制块都是设置在用户空间，而线程所执行的操作，也无须内核的帮助，因而内核完全不知道用户级线程的存在。
* 对于设置了用户级线程的系统，其调度仍是以进程为单位进行的。
* 在采用轮转调度算法时，各个进程轮流执行一个时间片。但假如在进程A中包含了一个用户级线，而在另一个进程B中含有100个用户级线程，这祥，进程A中线程的运行时间，将是进程B 中各线程运行时间的100倍，相应地，其速度要决上100倍.

优点：

* 线程切换不调用核心
* 调度是应用程序特定的：可以选择最好的算法
* ULT可运行在任何操作系统上（只需要线程库）

缺点：

* 大多数系统调用是阻塞的，因此核心阻塞进程，故进程中所有线程将被阻塞
* 核心只将处理器分配给进程，同一进程中的两个线程不能同时运行于两个处理器上

