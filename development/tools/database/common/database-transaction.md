数据库事务
==========

数据库事务(Database Transaction) ，是指作为单个逻辑工作单元执行的一系列操作，要么完全地执行，要么完全地不执行。 事务处理可以确保除非事务性单元内的所有操作都成功完成，否则不会永久更新面向数据的资源。通过将一组相关操作组合为一个要么全部成功要么全部失败的单元，可以简化错误恢复并使应用程序更加可靠。一个逻辑工作单元要成为事务，必须满足所谓的ACID（原子性、一致性、隔离性和持久性）属性。事务是数据库运行中的一个逻辑工作单位，由DBMS中的事务管理子系统负责事务的处理。

---

## 操作流程

设想网上购物的一次交易，其付款过程至少包括以下几步数据库操作：

一、更新客户所购商品的库存信息

二、保存客户付款信息--可能包括与银行系统的交互

三、生成订单并且保存到数据库中

四、更新用户相关信息，例如购物数量等等

正常的情况下，这些操作将顺利进行，最终交易成功，与交易相关的所有数据库信息也成功地更新。但是，如果在这一系列过程中任何一个环节出了差错，例如在更新商品库存信息时发生异常、该顾客银行帐户存款不足等，都将导致交易失败。一旦交易失败，数据库中所有信息都必须保持交易前的状态不变，比如最后一步更新用户信息时失败而导致交易失败，那么必须保证这笔失败的交易不影响数据库的状态--库存信息没有被更新、用户也没有付款，订单也没有生成。否则，数据库的信息将会一片混乱而不可预测。

数据库事务正是用来保证这种情况下交易的平稳性和可预测性的技术。

---

## 相关属性

### 原子性

（Atomic）（Atomicity)

事务必须是原子工作单元；对于其数据修改，要么全都执行，要么全都不执行。通常，与某个事务关联的操作具有共同的目标，并且是相互依赖的。如果系统只执行这些操作的一个子集，则可能会破坏事务的总体目标。原子性消除了系统处理操作子集的可能性。

### 一致性

（Consistent）(Consistency)

事务在完成时，必须使所有的数据都保持一致状态。在相关数据库中，所有规则都必须应用于事务的修改，以保持所有数据的完整性。事务结束时，所有的内部数据结构（如 B 树索引或双向链表）都必须是正确的。某些维护一致性的责任由应用程序开发人员承担，他们必须确保应用程序已强制所有已知的完整性约束。例如，当开发用于转帐的应用程序时，应避免在转帐过程中任意移动小数点。

### 隔离性

（Insulation）(Isolation)

由并发事务所作的修改必须与任何其它并发事务所作的修改隔离。事务查看数据时数据所处的状态，要么是另一并发事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看中间状态的数据。这称为隔离性，因为它能够重新装载起始数据，并且重播一系列事务，以使数据结束时的状态与原始事务执行的状态相同。当事务可序列化时将获得最高的隔离级别。在此级别上，从一组可并行执行的事务获得的结果与通过连续运行每个事务所获得的结果相同。由于高度隔离会限制可并行执行的事务数，所以一些应用程序降低隔离级别以换取更大的吞吐量。

### 持久性

（Duration）(Durability）

事务完成之后，它对于系统的影响是永久性的。该修改即使出现致命的系统故障也将一直保持。

---

## 责任

企业级的数据库管理系统（DBMS）都有责任提供一种保证事务的物理完整性的机制。就常用的SQL Server2000系统而言，它具备锁定设备隔离事务、记录设备保证事务持久性等机制。因此，我们不必关心数据库事务的物理完整性，而应该关注在什么情况下使用数据库事务、事务对性能的影响，如何使用事务等等。

作为大型的企业级数据库，SQL Server2000对事务提供了很好的支持。我们可以使用SQL语句来定义、提交以及回滚一个事务。

---

## 处理模型

事务有三种模型：

1. 隐式事务是指每一条数据操作语句都自动地成为一个事务，每个事务都有显式的开始和结束标记。

2. 显式事务是指有显式的开始和结束标记的事务，事务的开始是隐式的，事务的结束有明确的标记。

3. 自动事务是系统自动默认的，开始和结束不用标记。

并发控制

1. 数据库系统一个明显的特点是多个用户共享数据库资源，尤其是多个用户可以同时存取相同数据。

  串行控制：如果事务是顺序执行的，即一个事务完成之后，再开始另一个事务

  并行控制：如果DBMS可以同时接受多个事务，并且这些事务在时间上可以重叠执行。

2. 并发控制概述

  事务是并发控制的基本单位，保证事务ACID的特性是事务处理的重要任务，而并发操作有可能会破坏其ACID特性。

  DBMS并发控制机制的责任：

  对并发操作进行正确调度，保证事务的隔离性更一般，确保数据库的一致性。

  如果没有锁定且多个用户同时访问一个数据库，则当他们的事务同时使用相同的数据时可能会发生问题。由于并发操作带来的数据不一致性包括：丢失数据修改、读”脏”数据（脏读）、不可重复读、产生幽灵数据。

  （1）丢失数据修改

  当两个或多个事务选择同一行，然后基于最初选定的值更新该行时，会发生丢失更新问题。每个事务都不知道其它事务的存在。最后的更新将重写由其它事务所做的更新，这将导致数据丢失。如上例。

  再例如，两个编辑人员制作了同一文档的电子复本。每个编辑人员独立地更改其复本，然后保存更改后的复本，这样就覆盖了原始文档。最后保存其更改复本的编辑人员覆盖了第一个编辑人员所做的更改。如果在第一个编辑人员完成之后第二个编辑人员才能进行更改，则可以避免该问题。

  （2）读“脏”数据（脏读）

  读“脏”数据是指事务T1修改某一数据，并将其写回磁盘，事务T2读取同一数据后，T1由于某种原因被除撤消，而此时T1把已修改过的数据又恢复原值，T2读到的数据与数据库的数据不一致，则T2读到的数据就为“脏”数据，即不正确的数据。

  例如：一个编辑人员正在更改电子文档。在更改过程中，另一个编辑人员复制了该文档（该复本包含到目前为止所做的全部更改）并将其分发给预期的用户。此后，第一个编辑人员认为所做的更改是错误的，于是删除了所做的编辑并保存了文档。分发给用户的文档包含不再存在的编辑内容，并且这些编辑内容应认为从未存在过。如果在第一个编辑人员确定最终更改前任何人都不能读取更改的文档，则可以避免该问题。

  （ 3）不可重复读

  指事务T1读取数据后，事务T2执行更新操作，使T1无法读取前一次结果。不可重复读包括三种情况：

  事务T1读取某一数据后，T2对其做了修改，当T1再次读该数据后，得到与前一不同的值。

  （4）产生幽灵数据

  按一定条件从数据库中读取了某些记录后，T2删除了其中部分记录，当T1再次按相同条件读取数据时，发现某些记录消失

  T1按一定条件从数据库中读取某些数据记录后，T2插入了一些记录，当T1再次按相同条件读取数据时，发现多了一些记录。
