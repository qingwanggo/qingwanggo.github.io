- [分布式系统](#分布式系统)
  - [什么是一致性？](#什么是一致性)
  - [一致性要求](#一致性要求)
  - [一致性的分类](#一致性的分类)
    - [严格一致性](#严格一致性)
    - [强一致性](#强一致性)
    - [弱一致性](#弱一致性)
    - [最终一致性](#最终一致性)
  - [共识性](#共识性)
  - [共识性与一致性](#共识性与一致性)
  - [水平扩展（横向扩展）](#水平扩展横向扩展)
  - [垂直扩展（纵向扩展）](#垂直扩展纵向扩展)
  - [一致性问题的模式和思路](#一致性问题的模式和思路)
    - [酸碱平衡理论](#酸碱平衡理论)
      - [ACID（酸）](#acid酸)
      - [CAP（帽子原理）](#cap帽子原理)
      - [BASE（碱）](#base碱)
# 分布式系统

## 什么是一致性？

- `定义`：在分布式系统中，运行着`多个相互关联的服务节点`。一致性指分布式系统中的多个节点，给定的一系列操作，在约定协议的保障下，使它们`对外呈现状态是一致的`。换言之即是`保证集群中所有服务节点的数据完全相同`并且能够`对某个提案(Proposal)达成一致`。

- 分布式事务一致性与分布式数据一致性：从不同角度对一致性进行描述。

    - 事务（数据库事务的简称）是数据库管理系统中执行过程中的一个逻辑单位，由一个有限的数据库操作序列构成。

- 分布式事务一致性：指的是`操作序列在多个服务节点中执行的顺序是一致的`。

- 分布式数据一致性：指的是`数据在多份副本中存储时，各副本中的数据是一致的`。

- 综上保证了分布式事务的一致性，也就保证的数据的一致性。

## 一致性要求

- 有限性：达成一致的结果在`有限的时间内`完成。

- 约同性：不同的节点最终完成决略的结果是相同的。

- 合法性：决策的结果必须是系统中某个节点提出来的。

## 一致性的分类

### 严格一致性

- 是在系统不发生任何故障，而且所有节点之间的通信无需任何时间这种理想的条件下，才能达到。这个时候整个系统其实就等价于一台机器了。在现实中，是不可能达到的。

- **越强的一致性要求往往会造成越弱的处理性能，以及越差的可扩展性。**

- 严格一致性的标准是，对于数据项x的任何读操作将返回最近一次对x进行的写操作的结果所对应的值。

- 严格一致性中存在的问题是它依赖于绝对的全局时间。对于所有的进程来说，所有写操作都是瞬间可见的，系统维持着一个绝对的全局时间顺序。如果一个数据项被改变了，那么无论数据项改变之后多久执行读操作，无论哪些进程执行读操作，无论这些进程的位置如何，所有在该数据项上执行的后续读操作都将得到新数值。

### 强一致性

- 当分布式系统中更新操作完成之后，任何多个进程或线程，访问系统都会获得最新的值。

- `顺序一致性`是指任何执行结果都是相同的，就好像所有进程对数据存储的读、写操作是按某种序列顺序执行的，并且每个进程的操作按照程序所指定的顺序出现在这个序列中 。[参考](https://zhuanlan.zhihu.com/p/35596768)

- `线性一致性`假设操作具有一个**全局有效时钟的时间戳**，但是这个时钟仅具有有限的精确度。要求时间戳在前的进程先执行。线性化是**根据一系列同步时钟**确定序列顺序的 。

### 弱一致性

- 指系统并不保证后续进程或线程的访问都会返回最新的更新的值。系统在数据成功吸入之后，不承诺立即可以读到最新写入的值，也不会具体承诺多久读到。但是会尽可能保证在某个时间级别（秒级）之后。可以让数据达到一致性状态。**也就是说**，如果能容忍后续的部分或者全部访问不到，则是弱一致性。

- 当代互联网一致性就是指分布式服务化系统之间的弱一致性

### 最终一致性

- 是弱一致性的特定形式。系统保证在没有后续更新的前提下，系统最终返回上一次更新操作的值。 **也就是说**，如果经过一段时间后要求能访问到更新后的数据，则是最终一致性。

## 共识性

- 定义：`共识性`描述了分布式系统中多个节点之间，彼此对某个状态达成一致结果的过程。 在实践中，要保障系统满足不同程度的一致性，核心过程往往需要通过共识算法来达成。

## 共识性与一致性

- **一致性**往往指分布式系统中多个副本**对外呈现的数据的状态**。如前面提到的顺序一致性、线性一致性，描述了多个节点对数据状态的维护能力。

- **共识性**则描述了分布式系统中多个节点之间，彼此对某个状态达成一致结果的过程。

- 综上，一致性描述的是结果状态，共识则是一种手段。达成某种共识并不意味着就保障了一致性（这里的一致性指强一致性）。只能说共识机制，能够实现某种程度上的一致性。

## 水平扩展（横向扩展）

- 指由于单一节点无法满足性能需求，需要扩展为多个节点，多个节点具有一致的功能，组成一个服务池，一个节点服务一部分的请求量，所有节点共同处理大规模高并发的请求量

## 垂直扩展（纵向扩展）

- 指按功能拆分，秉着`专业的人干专业的事`的原则，把一个复杂的功能拆分为多个单一、简单功能，不同的单一功能组合在一起，和未拆分前完成的功能是一样的。

## 一致性问题的模式和思路

### 酸碱平衡理论

- ACID英文中意思是“酸”，BASE的意思是“碱”。

#### ACID（酸）

- A：Atomicity，原子性。

- C：Consistency，一致性。

- I：Isolation，隔离性。

- D：Durability，持久性。

- 具有ACID特性的数据库支持强一致性，强一致性代表数据库本身不会出现不一致，每个事务都是原子的，或者成功或者失败，事务间是隔离的，互相完全不受影响，而且最终状态是持久化的。因此，数据库会从一个明确的状态过渡到另一个明确的状态，中间的临时状态是不会出现的，如果出现也会及时地自动修复，因此是强一致的。三个典型的关系型数据库Oracle、Mysql、DB2都能保证强一致性，通常是通过多版本控制协议（MVCC）来实现的。

#### CAP（帽子原理）

- C：Consistency，一致性。在分布式系统中的所有数据备份，在同一时刻具有同样的值，所有节点在同一时刻读取的数据都是最新的数据副本。

- A：Availability，可用性，好的响应性能。完全的可用性指的是在任何故障模型下，服务都会在有限的时间内处理完成并进行响应。

- P：Partition tolerance，分区容忍性。尽管网络上有部分消息丢失，但系统仍然可继续工作。

`CAP原理证明，任何分布式系统只可满足以上两点，无法兼顾三者`

- 关系型数据库是单点无复制的，因此不具备分区容忍性；

- `分布式的服务化系统`都需要满足分区容忍性，因此`必须在一致性和可用性之间进行权衡选择`。

#### BASE（碱）

- BASE思想解决了CAP提出的分布式系统的一致性和可用性不可兼得的问题，具体参考：[Eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency)

- BASE思想与ACID原理截然不同，满足CAP原理，通过牺牲强一致性来获得可用性；通过最终一致性来尽量满足业务的绝大数需求。

- BA：Basiclly Available，基本可用。

- S：Soft State，软状态，状态可以在一段时间内不同步。

- E：Eventual consistency，最终一致，在一定的窗口内，最终数据达成一致即可。

- BASE思想基础，对复杂的分布式事务进行拆解，对其中的每个步骤都记录其状态，有问题时可以根据记录的状态来继续执行任务，达成最终一致。