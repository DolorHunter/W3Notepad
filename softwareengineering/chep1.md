# 第1章 软件工程学概述

迄今为止，计算机系统已经经历了 `4` 个不同的发展阶段，但是，人们仍然没有彻底摆脱 `“软件危机”` 的困扰，软件已经成为限制计算机系统发展的瓶颈。

为了更有效地开发与维护软件，软件工作者在20世纪60年代后期开始认真研究消除软件危机的途径，从而逐渐形成了一门新兴的工程学科——计算机软件工程学。

## 1.1 软件危机

软件是多种术语和对象的集合，并将这些术语和对象有效地配置在一起。一般包括程序、文档和数据。

角色

1. 首先，软件作为一种服务社会的产品
2. 其次，软件也可以作为其他产品的承载工具

特点

1. 软件是被工程化的逻辑系统；
2. 软件一般没有磨损；
3. 软件具有不同于一般实物系统的复杂性

产品系统

![img 1.1 - 1](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588130710/notepad/2020-04-29_112344_ld7oom.webp)

![img 1.1 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588130711/notepad/2020-04-29_112400_usuaau.webp)

![img 1.1 - 3](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588130710/notepad/2020-04-29_112413_pgzbgx.webp)

软件与软件系统

为实现要求的功能和性能，必须制作或获取一系列软件部件

软件元素分为两类

- 应用软件用来实现信息处理的功能
- 系统软件完成使应用软件能与其它系统元素交互的控制功能

![img 1.1 - 4](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131140/notepad/2020-04-29_112730_mwfbwc.webp)

![img 1.1 - 5](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131139/notepad/2020-04-29_112745_kj7b3p.webp)

![img 1.1 - 6](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131140/notepad/2020-04-29_112800_nqlxbs.webp)

软件分类

- 传统分类
  - 系统软件
  - 应用软件
  - 工程和科学软件
  - 嵌入式软件
  - 专用产品软件
  - Web应用软件
  - 人工智能软件

- 现代分类
  - 随处计算软件（Ubiquitous computing)；
  - 网络源软件（Netsourcing)；
  - 开源软件；
  - 数据挖掘（data mining)；
  - 网格计算(grid computing)；
  - 认知机器(cognitive machines)；
  - 软件新经济研究（电子商务技术）；
  - SOA(多种老应用的整合和新应用的灵活性）；

### 1.1.1 软件危机的介绍

在计算机软件的开发和维护过程中所遇到的一系列严重问题。这些问题不是在解决具体问题时遇到的，而是软件开发过程所面临的具有普适性的问题。

软件危机的典型表现

1. 对软件开发成本和进度的评估常常很不准确；
2. 用户对“已完成的”软件系统不满意；
3. 软件产品的质量无法保证；
4. 软件难以维护；
5. 相关的开发文档不健全；
6. 软件的重要性在不断提高；
7. 软件开发工作量的提高；
8. 软件需求越来越复杂；

`注：概括说，开发周期长、成本高、质量差、适应性差和难维护等四大难题.`

软件问题

- 为什么需要这么长的时间去获取一个可用的软件；
- 为什么软件开发的费用这么高；
- 为什么不能在将软件提交给我们的用户之前，发现所有的软件错误并解决它们；
- 为什么需要花费那么多的时间和努力来维护已经在运行的系统；
- 为什么无论在软件被开发还是在维护阶段我们都那么困难来度量它；

软件管理者

1. 对于软件开发有一些通用的能够适应所有需求的准则或程序，可满足所有的开发需求；
2. 如果软件产品的开发周期拖后了，可以通过增加人手来加快软件的开发速度；
3. 通过从第三方采购软件项目，就可以轻松地什么都不用做地完成项目；

软件用户

1. 一般对于需求的描述就足够开始编写程序了，详细的细节将由开发人员在开发过程中补充完善；
2. 项目需求在不断改变，但由于软件是灵活的因此这种变化可以轻易地被在软件中进行调整；

软件开发者

1. 一旦完成软件的编写，并成功上线运行，那么软件开发的工作就完成了；
2. 对于软件的好坏，只有到软件编写完成后才可以看到。
3. 仅仅可运行的软件产品才是用户需要的东西
4. 在编写软件过程中编写文档和其他一些工作都是在浪费时间

遗留软件与软件进化

所谓遗留软件是指多年之前开发的，能够继续被修改以满足商业需要和计算平台的系统，对于这些系统的增殖处理常常是让一些大的组织头痛的事情，系统的维护费用和风险都将增大。

### 1.1.2 产生软件危机的原因

与软件本身特点有关

![img 1.1.2 - 1](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131705/notepad/2020-04-29_113557_zt0t8z.webp)
![img 1.1.2 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131704/notepad/2020-04-29_113629_o84sed.webp)

软件开发与维护的方法不正确有关

![img 1.1.2 - 3](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131705/notepad/2020-04-29_113743_xs80qo.webp)

在软件开发的不同阶段进行修改需要付出的代价

![img 1.1.2 - 4](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131703/notepad/2020-04-29_113824_jyomzl.webp)

### 1.1.3 消除软件危机的途径

![img 1.1.3](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588131703/notepad/2020-04-29_113908_toag1d.webp)

>思考题
>
>1.什么是软件；
>
>2.软件的特点及其问题；
>
>3.今天软件危机是否已经解决了，你认为软件危机是否可以最终解决；
>
>4.例举一些在现实生活中的软件观点；

## 1.2 软件工程

### 1.2.1 软件工程介绍

软件工程概述

软件工程是指导计算机软件 `开发` 和 `维护` 的一门 `工程学科`。采用工程的概念、原理、技术和方法来开发与维护软件，把经过时间考验而证明正确的 `管理技术` 和当前能够得到的最好 `技术` 方法结合起来，以 `经济` 地开发出高质量的软件并有效地维护它，这就是软件工程。

任何学科从产生到成熟必须经历的四个层次：

- 解决哲学问题；
- 基础科学建立；
- 技术科学建立；
- 系统的管理工程方法（学科成熟的标志）；

软件工程定义

1. 把系统的、规范的、可度量的方法应用于软件开发、运行和维护过程，也就是把工程应用于软件；
2. 将第一点提到的方法作为对象的研究活动；

1968年在第一届NATO会议上曾经给出了软件工程的一个早期定义：“软件工程就是为了 `经济` 地获得 `可靠的` 且能在实际机器上 `有效地` 运行的软件，而建立和使用完善的工程原理。”

1993年IEEE进一步给出了一个更全面更具体的定义：“软件工程是： ①把 `系统的、规范的、可度量的` 途径应用于软件开发、运行和维护过程，也就是把工程应用于软件； ②研究①中提到的 `途径`。

PressMan的软件工程定义

![img 1.2.1](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588134204/notepad/2020-04-29_114649_zs2sql.webp)

`注：软件工程的三个基本要素：方法、工具和过程`

`- 过程：规定了完成各项任务的过程；`

`- 方法：完成软件开发的各项任务的技术方法；`

`- 工具：软件工程的支撑环境；`

软件的本质特性

1. 关注于大型程序的构造；
2. 可控制的复杂性；
3. 软件经常变化；
4. 提高软件的开发效率；
5. 协作开发产品；
6. 有效支持用户；
7. 服务特色领域；

### 1.2.2 软件工程的基本原理

1. 用分阶段的生命周期计划严格管理；
2. 坚持进行阶段评审（早发现问题）；
3. 实行严格的产品控制（适应需求变化，并控制变化） ；
4. 采用现代程序设计技术；
5. 结果应该能够清楚地审查；
6. 开发小组的人员应该少而精；
7. 承认不断改进软件工程实践的必要性；

### 1.2.3 软件工程方法学

- __[传统方法学](#传统方法学)__
- __[面向对象方法学](#面向对象方法学)__
- __[面向组件的软件工程方法](#基于组件的开发模型)__
- __[面向方面的软件工程方法](#面向方面的软件开发方法)__
- 面向Agent的软件开发方法
- 敏捷软件工程开发方法

#### 传统方法学

传统方法学也称为生命周期方法学或 `结构化范型`。它采用结构化技术（结构化分析、结构化设计和结构化实现）来完成软件开发的各项任务，并使用适当的软件工具或软件工程环境来支持结构化技术的运用。

特点

- 传统方法学把软件生命周期的全过程依次 `划分为若干个阶段`，然后顺序地完成每个阶段的任务。
- 每个阶段的开始和结束都有严格标准，对于任何两个相邻的阶段而言，前一阶段的结束标准就是后一阶段的开始标准。
- 在每一个阶段结束之前都必须进行正式严格的 `技术审查` 和 `管理复审`。
- 审查的一条主要标准就是每个阶段都应该交出“最新式的”（即和所开发的软件完全一致的）高质量的 `文档资料`，从而保证在软件开发工程结束时有一个完整准确的软件配置交付使用。
- 采用生命周期方法学可以大大提高软件开发的成功率，软件开发的生产率也能明显提高。
- 目前，传统方法学仍然是人们在开发软件时使用得十分广泛的软件工程方法学。

#### 面向对象方法学

与传统方法相反，`面向对象方法` 把 `数据` 和 `行为` 看成是同等重要的，它是一种以数据为主线，把数据和对数据的操作紧密地结合起来的方法。

面向对象方法学基本原则

尽量 `模拟人类习惯的思维方式`，使开发软件的方法与过程尽可能接近人类认识世界、解决问题的方法与过程，从而使描述问题的 `问题空间`（也称为问题域）与实现解法的 `解空间`（也称为求解域）`在结构上尽可能一致`。

优点

- 降低了软件产品的复杂性，提高了软件的可理解性，简化了软件的开发和维护工作。
- 面向对象方法特有的 `继承性` 和 `多态性`，进一步提高了面向对象软件的 `可重用性`。

#### 基于组件的开发模型

这种模型结合了一些螺旋模型的特性，应用该模型的主要目的是对现有组件对象的复用

主要步骤：

1. 研究可用的基于问题领域的组件产品；
2. 怎样集成组件；
3. 设计合适组件应用的软件体系结构；
4. 将组件集成进软件架构；
5. 对于组件功能的综合测试工作；

#### 面向方面的软件开发方法

面向方面直观的理解就是对软件组件做一次垂直的分解，提取其中的那些具有交叉性的功能和一些非功能属性，建立方面；

一些公共的系统方面有：用户接口、协作工作、分布、内存管理、安全管理等。

>思考题
>
>通过以上学习,说说你理解的软件工程概念?
>
>说说软件工程三个要素之间的关系?
>
>例举出您所知道的一些软件工程方法?

## 1.3 软件生命周期

软件过程

当建造一个产品或系统时，采用一系列可推断的步骤是非常重要的，这样一个路径表能够帮助你建立一个及时的、高品质的结果。这个所谓的路径表就是我们所说的软件过程。

软件过程框架

软件过程框架通过封装一些阶段性行为，并将这些行为普遍应用到各类软件项目中，而不需要考虑该项目的大小和复杂性等。

软件生命周期

三个阶段，七个环节

软件生命周期由 `软件定义`、`软件开发` 和 `运行维护`（也称为软件维护）3个时期组成，每个时期又进一步划分成若干个阶段。

- 软件定义阶段：可行性研究和需求分析
- 软件开发阶段：概要设计、详细设计、编码和测试和综合测试
- 软件维护：-

`软件定义` 时期的任务是：确定软件开发工程必须完成的 `总目标`；确定工程的 `可行性`；导出实现工程目标应该采用的 `策略` 及系统必须完成的 `功能`；估计完成该项工程需要的 `资源和成本`，并且制定 `工程进度表`。这个时期的工作通常又称为 `系统分析`，由系统分析员负责完成。

软件定义时期通常进一步划分成3个阶段，即 `问题定义`、`可行性研究` 和 `需求分析`。开发时期具体设计和实现在前一个时期定义的软件，它通常由下述4个阶段组成：`总体设计`，`详细设计`，`编码` 和 `单元测试`，`综合测试`。其中前两个阶段又称为系统设计，后两个阶段又称为系统实现。维护时期的主要任务是使软件持久地满足用户的需要。

一般性框架

1. 通讯（问题定义、可行性研究、需求分析）
2. 计划（总体设计）
3. 建模（详细设计）
4. 构造（编码和测试、综合测试）
5. 部署（综合测试和软件维护）

`注：这些过程在具体实施时可能会有些不同，但过程的框架行为始终不变。`

软件过程中的雨伞行为

1. 软件项目的跟踪和控制；
2. 风险管理；
3. 软件品质保障；
4. 形式化技术分析；
5. 软件度量；
6. 软件配置管理；
7. 重用管理；

## 1.4 软件过程

典型的软件过程模型

通过使用模型简洁地描述软件过程中的各项活动、任务、中间产品和里程碑的完成过程，如软件生命周期。

包括两类软件过程模型，`说明性过程模型` 和 `敏捷过程模型`

软件过程是为了获得高质量软件所需要完成的 `一系列任务的框架`，它规定了完成各项任务的 `工作步骤`。

软件过程描述为了开发出客户需要的软件，什么人（who）、在什么时候（when）、做什么事（what）以及怎样（how）做这些事以实现某一个特定的具体目标。

### 1.4.1 瀑布模型

`瀑布模型` 一直是唯一被广泛采用的生命周期模型，现在它仍然是软件工程中应用得最广泛的过程模型。如下图所示为传统的瀑布模型。

![img 1.4.1](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588219529/notepad/2020-04-30_115853_juqq53.webp)

#### 瀑布模型特点

- a) 阶段间具有 `顺序性` 和 `依赖性`

两重含义：

①必须等前一阶段的工作完成之后，才能开始后一阶段的工作；

②前一阶段的输出 `文档` 就是后一阶段的输入文档，因此，只有前一阶段的输出文档正确，后一阶段的工作才能获得正确的结果。

- b) `推迟实现` 的观点

瀑布模型在编码之前设置了系统分析与系统设计的各个阶段，分析与设计阶段的基本任务规定，在这两个阶段主要考虑目标系统的 `逻辑模型`，不涉及软件的 `物理实现`。

- c) `质量保证` 的观点：

软件工程的基本目标是优质、高产。为了保证所开发的软件的质量，在瀑布模型的每个阶段都应坚持两个重要做法。

①每个阶段都 `必须完成规定的文档`，没有交出合格的文档就是没有完成该阶段的任务。

②每个阶段结束前都要 `对所完成的文档进行评审`，以便尽早发现问题，改正错误。

#### 瀑布模型优点

1. 可强迫开发人员采用规范的方法（例如，结构化技术）；
2. 严格地规定了每个阶段必须提交的文档；
3. 要求每个阶段交出的所有产品都必须经过质量保证小组的仔细验证。

#### 实际的瀑布模型

传统的瀑布模型过于理想化了，事实上，人在工作过程中不可能不犯错误。实际的瀑布模型是带 `“反馈环”` 的，如后图所示。

![img 1.4.1 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588219529/notepad/2020-04-30_120346_i4regh.webp)

1. 图中实线箭头表示开发过程，虚线箭头表示维护过程。
2. 实际的瀑布模型当在后面阶段发现前面阶段的错误时，需要沿图中左侧的反馈线返回前面的阶段，修正前面阶段的产品之后再回来继续完成后面阶段的任务。

### 1.4.2 快速原型模型

快速原型是 `快速` 建立起来的可以在计算机上运行的程序，它所能完成的功能往往是最终产品能完成的功能的一个子集。

![img 1.4.2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588219731/notepad/2020-04-30_120811_blkkwy.webp)

图中实线箭头表示开发过程，虚线箭头表示维护过程。

快速原型模型是 `不带反馈环` 的，这正是这种过程模型的主要优点： 软件产品的开发基本上是 `线性` 顺序进行的。

能基本上做到线性顺序开发的主要原因如下：

1. 原型系统已经通过与用户交互而得到验证，据此产生的规格说明文档正确地描述了用户需求，因此，在开发过程的后续阶段不会因为发现了规格说明文档的错误而进行较大的返工。
2. 开发人员通过建立原型系统已经学到了许多东西，因此，在设计和编码阶段发生错误的可能性也比较小，这自然减少了在后续阶段需要改正前面阶段所犯错误的可能性。

### 1.4.3 增量模型

增量模型也称为渐增模型。使用增量模型开发软件时，把软件产品作为 `一系列的增量构件` 来设计、编码、集成和测试。每个构件由多个相互作用的模块构成，并且能够完成特定的功能。使用增量模型时，第一个增量构件往往实现软件的基本需求，提供最核心的功能。

![img 1.4.3](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588219922/notepad/2020-04-30_121022_dxhb32.webp)

#### 增量模型优点

1. 能在较短时间内向用户提交可完成部分工作的产品。
2. 逐步增加产品功能可以使用户有较充裕的时间学习和适应新产品，从而减少一个全新的软件可能给客户组织带来的冲击。

#### 增量模型难点

1. 在把每个新的增量构件集成到现有软件体系结构中时，必须不破坏原来已经开发出的产品。
2. 必须把软件的体系结构设计得便于按这种方式进行扩充，向现有产品中加入新构件的过程必须简单、方便，也就是说，软件体系结构必须是开放的。

#### 风险更大的增量模型

![img 1.4.3 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588220109/notepad/2020-04-30_121418_c9wjpv.webp)

### 1.4.4 螺旋模型

螺旋模型的基本思想是，使用原型及其他方法来尽量降低风险。理解这种模型的一个简便方法，是把它看作 `在每个阶段之前都增加了风险分析过程的快速原型模型`。

![img 1.4.4](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588219923/notepad/2020-04-30_121102_nsvclo.webp)
![img 1.4.4 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588219922/notepad/2020-04-30_121120_q9zseo.webp)

简单螺旋模型 & 完整螺旋模型

形式化方法

该方法强调采用严格的数学方法来描述软件的需求和设计任务。

>思考题
>
>1.软件过程及其框架的含义？
>
>2.什么是软件过程中的雨伞行为？
>
>3.例举一个软件过程模型，并解释其中每个阶段的意义；

### 1.4.7 敏捷过程与极限编程

`敏捷过程` 为了使软件开发团队具有高效工作和快速响应变化的能力，17位著名的软件专家于2001年2月联合起草了敏捷软件开发宣言。敏捷软件开发宣言由下述 `4` 个简单的价值观声明组成。

#### 敏捷软件工程

背景：在现代经济条件下，预测一个基于计算的系统怎样随着时间而不断地变化和发展是非常困难的。市场变化非常快，用户的需求变化也是非常快的，并且一些新的竞争的出现是没有警告的。在这种情况下，在软件项目开始之初就能够充分定义好需求是非常困难的。软件工程必须能够足够的敏捷来适应这种不断变化的商业环境

##### 敏捷软件工程定义

敏捷软件工程包含了一种哲学思想和一套开发指导原则。指导原则主要追求：

1. 客户的满意和软件开发过程中的早期增量;
2. 小的高激励的项目团队；
3. 非形式化的方法；
4. 最小限度的软件工程产品；
5. 简化的整个开发过程；
6. 强调开发过程中开发者和客户的不断交流；

##### 敏捷开发的体现

1. 快速的响应变化；
2. 在各方之间建立有效的沟通；
3. 吸引客户参与到团队中；
4. 组织团队控制整个工作的执行；
5. 快速的增量式的生产软件；

##### 敏捷团队的特点

敏捷开发的基础是一个敏捷团队的建立，该团队核心的特点是自组织，自组织包括三层含义：

1. 敏捷的团队组织自己去完成工作;
2. 该团队组织过程最好地适应本地的环境；
3. 团队组织工作任务的调度来最好地适应软件增量；

##### 敏捷软件过程

敏捷软件工程过程的三个假设：

1. 对于用户需求的变化难以预测;
2. 软件设计和软件构造之间是相互间隔的，即在构造是被使用证明设计的正确性之前，很难确定那种设计模型是被需要的；
3. 分析、设计、构造和测试都是无法预期的；

`注：敏捷软件过程的适应力，是敏捷软件过程的核心。所以增量的适应是敏捷软件过程的必然。`

1. 以客户描述的需求内容为驱动;
2. 充分认识到一切计划都是短暂的；
3. 使用迭代方法重点强调结构行为；
4. 采用多样的软件增量行为；
5. 适应变化的发生；

#### 极限编程

极限编程是一种最广泛使用的敏捷软件过程，其主要针对的是面向对象的开发方法。主要包括四个一般性框架：计划、设计、代码和任务

极限编程（eXtreme Programming, XP）是敏捷过程中最富盛名的一个，其名称中 `“极限”` 二字的含义是指把好的开发实践运用到极致。

目前，极限编程已经成为一种典型的开发方法，广泛应用于 `需求模糊且经常改变` 的场合。

##### 极限编程的整体开发过程

![img 1.4.7](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588230284/notepad/2020-04-30_145107_svuo3g.webp)

##### 极限编程的迭代过程

下图描述了极限编程的整体开发过程。首先，项目组针对客户代表提出的“用户故事” 进行讨论，提出隐喻，在此项活动中可能需要对体系结构进行“试探”。然后，项目组在隐喻和用户故事的基础上，根据客户设定的优先级制订交付计划。接下来开始多个迭代过程（通常每个迭代历时1~3周），在迭代期内产生的新用户故事不在本次迭代内解决，以保证本次开发过程不受干扰。开发出的新版本软件通过验收测试之后交付用户使用。

![img 1.4.7 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588230284/notepad/2020-04-30_145203_dcwwms.webp)

##### 极限编程计划

关于制定计划的实现方法中可以反映出大多数迭代式RAD项目（快速原型开发）的特点。短期的，每三周为一个循环，频繁地更新，按优先级划分任务与技术，分配stories（一个story定义了一个特殊的功能需求并以一种简单的方式记录在卡片上），所有的这些就是构成了XP中的计划。

##### 极限编程设计

简单的设计包含两个部分：

1. 为已定义的功能进行设计，而不是为潜在地未来可能的功能进行设计。
2. 创建最佳的可以实现功能的设计。换句话说，不用管未来会是怎样，只创建一个目前为止可以实现的最好的设计。“如果你相信未来是不确定的，并且你相信你可以很方便的改变你的主意的话，那么对未来功能的考虑是危险的。即是：只有在你真正需要的时候才去做。

##### 极限编程重构

如果我不得不找出一个能够将XP和其他方法区别开来的东西那就是重构――不断的软件再设计以改进它对于变化的反应。RAD方法常常很少甚至根本不与设计相关；XP应当被看作持续设计。当变化既快而且频繁的时候，应投入更多的精力于重构之上。在重构中一个特别的观点是结对编程（Pair Programming)

##### 结对编程

结对编程通常意味着两名开发者共享一台计算机。一个键入代码，另一个帮他指路。

优点

1. 所有的设计决定都用了至少两个人的智慧
2. 至少有两个人熟悉系统的每一个部分。
3. 两个人都忽略测试或其他任务的可能性更小。
4. 改变各对的组合可以在团队范围内传播知识。
5. 代码通常至少被一个人复查。

##### 极限编程测试

XP充满发人深思的有趣的难题。例如：什么是“先测试后编码”？一般的软件公司和一些IT机构是通过代码的行数来对程序员的绩效加以考核，而测试的绩效则是通过发现的缺陷的数量来考核的。这两种方法都不能鼓励减少测试前产生的缺陷的数量。XP使用两种测试：单元测试和功能测试。单元测试要求在写代码之前就开发出相应功能的测试方法，并测试应当是自动化的。代码一完成，它就被立即用有关测试集加以测试，从而能立即得到反馈。

##### 传统方法与极限编程的比较

极限编程并不是否认传统的开发方法。传统方法可用于开发那些变化程度不大并可预期最终结果的软件。然而，对于一些轻量级的软件，并且传统开发方法已无法满足现在的快速变化软件需求的要求时，比较适合采用极限编程技术进行开发。轻量级软件开发实践的创始人Bob Charette认为"由于软件工程研究所(SEI)这样组织的官僚化、顽固性，以及诸如CMM的实践，使得他们日益脱离当今的软件开发。

>思考题
>
>1.什么是敏捷软件工程？
>
>2.极限编程的过程是什么？
>
>3.极限编程与传统软件过程之间的区别是什么？

### 1.4.8 微软过程

#### a) 微软过程准则

- 项目计划应该兼顾未来的不确定因素。
- 用有效的 `风险管理` 来减少不确定因素的影响。
- 经常生成并快速地测试软件的过渡版本，从而提高产品的 `稳定性` 和 `可预测性`。
- 采用 `快速循环`、`递进` 的开发过程。
- 用创造性的工作来平衡产品特性和产品成本。
- 项目进度表应该具有较高稳定性和权威性。
- 使用小型项目组并发地完成开发工作。
- 在项目早期把软件配置项 `基线化`，项目后期则冻结产品。
- 使用原型验证概念，对项目进行早期论证。
- 把零缺陷作为追求的目标。
- `里程碑评审会` 的目的是改进工作，切忌相互指责。

#### b) 微软软件生命周期

![img 1.4.8](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588230282/notepad/2020-04-30_150304_bzijbh.webp)

- 规划阶段
- 设计阶段
- 开发阶段
- 稳定阶段
- 发布阶段

#### c) 微软过程模型

微软过程的每一个生命周期发布一个递进的软件版本，各个生命周期持续、快速地 `迭代循环`。如下图所示：

![img 1.4.8 - 2](https://res.cloudinary.com/dfb5w2ccj/image/upload/v1588230283/notepad/2020-04-30_150326_ly7m3o.webp)

## 本章小结

1. 软件；
2. 软件工程；
3. 软件过程；
4. 敏捷软件工程；
5. 敏捷软件工程的软件过程；
