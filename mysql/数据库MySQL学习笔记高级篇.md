# 1. mysql架构介绍

## 1.1 mysql简介

MySQL是一个关系型数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle公司。

MySQL是一种关联数据库管理系统，将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

MySQL是开源的，所以你不需要支付额外的费用。

MySQL支持大型的数据库。可以处理拥有上千万条记录的大型数据库。MySQL使用标准的SQL数据语言形式。

MySQL可以允许于多个系统上，并且支持多种语言。这些编程语言包括C、C++、Python、Java、Per、PHP、Eifel、Ruby和Tcl等。Mysql对PHP有很好的支持，PHP是目前最流行的Web开发语言。

MySQL支持大型数据库，支持5000万条记录的数据仓库，32位系统表文件最大可支持4GB，64位系统支持最大的表文件为8TB。Mysql是可以定制的，采用了GPL协议，你可以修改源码来开发自己的MySQL系统。

> 完整的my[sql优化](https://so.csdn.net/so/search?q=sql优化&spm=1001.2101.3001.7020)需要很深的功底，大公司甚至有专门的DBA负责。
>
> - mysql内核
> - sql优化工程师
> - mysql服务器的优化
> - 各种参数常量设定
> - 查询语句优化
> - 主从复制
> - 软硬件升级
> - 容灾备份
> - sql编程

## 1.2 mysql逻辑架构介绍

和其它数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎的架构上，**插件式的存储引擎架构将查询处理和其它的系统任务以及数据的存储提取相分离。**这种架构可以根据业务的需求和时机需要选择合适的存储引擎。

![image-20240927175047947](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240927175047947.png)

从上到下，网络连接层，数据库服务层，存储引擎层，系统文件层。

**网络连接层**

最上层是一些客户端和连接服务，包含本地sock通信和大多数基于客户端/服务端工具实现的类似于tcpp的通信。主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。

**数据库服务层**

第二层架构主要完成大多少的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化及部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。在该层，服务器会解析査询并创建相应的内部解析树，并对其完成相应的优化如确定查询表的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部的缓存。如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

**存储引擎层**

存储引擎层，存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API与存储引擎进行通信。不同的存储引擎具有的功能不同，这样我们可以根据自己的实际需要进行选取。后面介绍MyISAM和InnoDB。

**系统文件层**

系统文件层主要包括MySQL中存储数据的底层文件，将数据存储在运行于裸设备的文件系统之上，与上层的存储引擎进行交互。其存储的文件主要有：日志文件、数据文件、配置文件、MySQL进程pid文件和socket文件等。

**mysql的执行过程大概如下图：**

![image-20240927175703803](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240927175703803.png)

客户端先发送查询语句给服务器，进行sql解析，生成解析树，再预处理，生成第二个解析树，最后经过优化器，生成真正的执行计划根据执行计划，调用存储引擎的API来执行查询将结果返回给客户端。

## 1.3 mysql存储引擎

看你的mysql现在已提供什么存储引擎：`show engines;`
看你的mysql当前默认的存储引擎：`show variables like ‘%storage_engine%’;`

**MyISAM vs InnoDB**

![img](https://i-blog.csdnimg.cn/blog_migrate/7ef21d3c2bcefd7b72e6584dba05f3fd.png#pic_center)

<br/>

阿里巴巴、淘宝用哪个?![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/26733f6115bd6d81452c02331dfc3c62.png#pic_center)

> InnoDB是⼀个将表中的数据存储到磁盘上的存储引擎，所以即使关机后重启我们的数据还是存在的。⽽真正处理数据的过程是发⽣在内存中的，所以需要把磁盘中的数据加载到内存中，如果是处理写⼊或修改请求的话，还需要把内存中的内容刷新到磁盘上。⽽我们知道读写磁盘的速度⾮常慢，和内存读写差了⼏个数量级，所以当我们想从表中获取某些记录时，InnoDB采取的⽅式是：将数据划分为若⼲个”⻚“，以⻚作为磁盘和内存之间交互的基本单位，InnoDB中⻚的⼤⼩⼀般为 16K,也就是在⼀般情况下，⼀次最少从磁盘中读取16KB的内容到内存中，⼀次最少把内存中的16KB内容刷新到磁盘中。
>



## 1.4 SQL 的 7 种 Join 查询



![](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/img1.png)



## 1.5 mysql加载顺序

mysql执行顺序，是先从from开始读:

```sql
FROM <left_table>
ON <join_condition>
<join_type> JOIN <right_table>
WHERE <where_condition>
GROUP BY <group_by_list>
HAVING <having_condition>
SELECT 
DISTINCT <select_list>
ORDER BY <order_by_condition>
LIMIT <limit_number>
```

![image-20240926200542067](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240926200542067.png)

> **笛卡尔乘积（笛卡尔积）**
>
> 例如，A={a,b}, B={0,1,2}，则：
>
> A×B={(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}
>
> B×A={(0, a), (0, b), (1, a), (1, b), (2, a), (2, b)}

# 2. mysql索引

## 2.1 索引简介

Mysql官方定义：索引（index）是帮助 mysql 高效获取数据的数据结构。

索引的目的在于提高查询效率，可以类比字典。如果要查 “mysql” 这个单词，我们肯定需要定位到字母 m，然后从下往下找到字母 y ，再找到剩下的字母s、q、l。如果没有索引，那么你可能需要a…z挨个找，如果我想找到Java开头的单词呢?或者Oracle开头的单词呢? 是不是觉得如果没有索引，这个事情根本无法完成?

>  **索引本质:  索引是数据结构， 排好序的快速查找数据结构。**

**数据本身之外，数据库还维护着一个满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，这样就可以在这些数据结构的基础上实现高级查找算法，这种数据结构就是索引**。下图就是一种可能得索引方式示例：

![image-20240926201715924](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240926201715924.png)

左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址。

为了加快 Col2 的查找，可以维护一个右边所示的二叉查找树，每个节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这样就可以运用二叉查找法在一定的复杂度内获取到相应数据，从而快速检索出符合条件的记录。

一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上。

我们平常所说的索引，如果没有特别指明，都是指B树（多路搜索树，并不一定是二叉的）结构组织的索引。其中聚集索引、次要索引、覆盖索引、复合索引、前缀索引、唯一索引默认的都是使用 `B+` 树索引，统称索引。当然，除了 `B+` 树这种类型的索引之外，还有哈希索引（ `hash index` ）等。



## 2.2 索引的优势和劣势

**索引的优势**

* 类似大学图书馆建书目索引，提高数据检索的效率，降低数据库的IO成本。
* 索引能极大的减少存储引擎需要扫描的数据量。
* 通过索引列对数据进行排序，降低数据排序的成本，降低了CPU的消耗。

 **索引的劣势**

* 实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占用空间的。
* 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE、DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，都会调整因为更新所带来的键值变化后的索引信息。
* 索引只是提高效率的一个因素，如果你的MySQL有大数据量的表，就需要花时间研究建立最优秀的索引，或者优化查询。

## 2.3 索引分类

* **单值索引：**一个索引只包含单个列，一张表可以有多个单值索引。（建议一张表索引不要超过5个 优先考虑复合索引）
* **唯一索引：**索引列的值必须唯一，但允许有空值。
* **复合索引：**一个索引包含多个列。



## 2.4 添加索引方式

有四种方式来添加数据表的索引:

```sql
ALTER TABLE tbl_name ADD PRIMARY KEY(column_list). -- 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL.

ALTER TABLE tbl_name ADD UNIQUE index_name (column_list). -- 这条语句创建索引的值必须是唯一的(除了NULL外，NULL可能会出现多次)

ALTER TABLE tbl_name ADD INDEX index_name (column_list). -- 添加普通索引，索引值可出现多次。

ALTER TABLE tbl_name ADD FULLTEXT index_name(column_list). -- 该语句指定了索引为FULLTEXT，用于全文索引。
```



## 2.5 Mysql索引结构

mysql 选用 `B+Tree` 作为索引结构。

为什么索引能提升数据库查询效率呢？根本原因就在于**索引减少了查询过程中的 IO 次数**。那么它是如何做到的呢？使用 B+ 树。下面先简单了解一下 B 树和 B+ 树。

**B 树简略示意图：**

![image-20240930102105287](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240930102105287.png)

观察上图可见 B 树的两个特点：

1. 树内的每个节点都存储数据。
2. 叶子节点之间无指针连接。



**B+ 树简略示意图：**

![image-20240930102120319](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240930102120319.png)

再看 B+ 树相对于 B 树的两个特点：

1. 数据只出现在叶子节点
2. 所有叶子节点增加了一个链指针

> 叶子结点是离散数学中的概念。一棵树当中没有子结点（即度为0）的结点称为叶子结点，简称“叶子”。 叶子是指出度为0的结点，又称为终端结点。



**但是，Mysql为什么选择 B+ 树而不是 B 树呢？** 原因有两点：

* B 树每个节点中不仅包含数据的 key 值，还有 data 值。而每一个页的存储空间是有限的，如果 data 数据较大时将会导致每个节点能存储的 key 的数量很小，要保存同样多的 key ，就需要增加树的高度。树的高度每增加一层，查询时的磁盘 I/O 次数就增加一次，进而影响查询效率。而在 B+Tree 中，所有数据记录节点都是按照键值大小顺序存放在同一层的叶子节点上，而非叶子节点上只存储 key 值信息，这样可以大大加大每个节点存储的 key 值数量，降低 B+ 树的高度。
* B+树的叶子节点上有指针进行相连，因此在做数据遍历的时候，只需要对叶子节点进行遍历即可，这个特性使得B+树非常适合做范围查询。



## 2.6 聚簇索引

**聚集索引也称聚簇索引**，这种索引中，数据库表行中数据的物理顺序与键值的逻辑（索引）顺序相同。一个表的物理顺序只有一种情况，因此对应的聚集索引只能有一个。如果某索引不是聚集索引，则表中的行物理顺序与索引顺序不匹配，与非聚集索引相比，聚集索引有着更快的检索速度。 

1. 在`Innodb`中，聚簇索引默认就是主键索引。
2. 如果表中没有定义主键，那么该表的第一个唯一非空索引被作为聚集索引。
3. 如果没有主键也没有合适的唯一索引，那么innodb内部会生成一个隐藏的主键作为聚集索引，这个隐藏的主键是一个6个字节的列，改列的值会随着数据的插入自增。

> 扩展 1：`InnoDB`引擎是将根节点常驻内存的.
>
> 扩展 2：自增主键和uuid作为主键的区别 ?
>
> 由于主键使用了聚集索引，如果主键是自增id，那么对应的数据一定也是相邻地存放在磁盘上的，写入性能比较高。如果是uuid的形式，频繁的插入会使innodb频繁地移动磁盘块，写入性能就比较低了。



## 2.7 二级索引

聚簇索引只能在搜索条件是主键值时才能发挥作用，因为B+树中的数据都是按照主键进行排序的。那如果我们想以别的列作为搜索条件该咋办呢？难道只能从头到尾沿着链表依次遍历记录么？

不，我们可以多建几棵B+树，不同的B+树中的数据采用不同的排序规则.

![image-20240930115601365](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240930115601365.png)

如上图，多加一个以 name 字段建立的索引，就会多生成一颗非聚簇索引树。非聚集索引叶子节点上不再是真实数据，而是存储了索引字段自身值和主键索引。因此，当我们执行以下SQL语句时：

```sql
SELECT id,name FROM student WHERE name='叶良辰';
```

整个查询过程与聚集索引的过程一样，只需要扫描一次索引树（n次磁盘I/O和内存查询），即可拿到想要的数据。

但是，如果查询`name`索引树没有的数据时，情况就不一样了：

```sql
SELECT score FROM student WHERE name='叶良辰';
```

![image-20240930115809246](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240930115809246.png)

注意看上图中的红色箭头，因为扫描完`name`索引后，Mysql只能获取到对应的`id`和`name`，然后用id的值再去聚集索引中去查询`score`的值。这个过程相对于聚集索引查询的效率下降，可以理解了吧。

> 这就是通常所说的回表或者二次查询：使用聚集索引查询可以直接定位到记录，而普通索引通常需要扫描两遍索引树，即先通过普通索引定位到主键值，在通过聚集索引定位到行记录，这就是所谓的回表查询，它的性能比扫描一遍索引树低。

既然普通索引会导致回表二次查询，那么有什么办法可以应对呢？**建立联合索引**。



## 2.8 联合索引

联合索引，也称多列索引，就是同时为多个列建立索引，这个概念是跟单列索引相对的。联合索引依然是B+树，但联合索引的健值数量不是一个，而是多个。构建一颗B+树只能根据一个值来构建，因此数据库依据联合索引最左的字段来构建 B+ 树。

例如在 a 和 b 字段上建立联合索引，索引结构将如下图所示：

![image-20240930115955587](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20240930115955587.png)

一目了然，当我们再执行 `SELECT score FROM student WHERE name='叶良辰'; ` 时，可以直接通过扫描非聚集索引直接获取 score 的值，而不再需要到聚集索引上二次扫描了。



## 2.9 最左前缀匹配

联合索引中有一个重要的课题，就是最左前缀匹配。

**最左前缀匹配原则**：在 MySQL 建立联合索引时会遵守最左前缀匹配原则，即最左优先，在检索数据时从联合索引的最左边开始匹配。

这是为什么呢？我们再仔细观察索引结构，可以看到索引 key 在排序上，首先按 a 排序，a 相等的节点中，再按 b 排序。因此，如果查询条件是 a 或 a 和 b 联查时，是可以应用到索引的。如果查询条件是单独使用 b ，因为无法确定 a 的值，因此无法使用索引。



假如在 table 表的 a, b, c 三个列上建立联合索引，简要分类分析下联合索引的最左前缀匹配。

首先看等值查询：

1、全值匹配查询时（ where 子句搜索条件顺序调换不影响索引使用，因为查询优化器会自动优化查询顺序 ），可以用到联合索引

```sql
SELECT * FROM table WHERE a=1 AND b=3 AND c=2
SELECT * FROM table WHERE b=3 AND c=4 AND a=2
```

2、匹配左边的列时，可以用到联合索引

```sql
SELECT * FROM table WHERE a=1
SELECT * FROM table WHERE a=1 AND b=3
```

3、从最左列开始时，无法用到联合索引

```sql
SELECT * FROM table WHERE b=1 AND b=3
```

4、查询列不连续时，无法使用联合索引（会用到 a 列索引，但 c 排序依赖于 b，所以会先通过 a 列的索引筛选出a=1的记录，再在这些记录中遍历筛选 c=3 的值，是一种不完全使用索引的情况）

```sql
SELECT * FROM table WHERE a=1 AND c=3
```

再看范围查询：

1、范围查询最左列，可以使用联合索引

```sql
SELECT * FROM table WHERE a>1 AND a<5;
```

2、精确匹配最左列并范围匹配其右一列（a值确定时，b是有序的，因此可以使用联合索引）

```sql
SELECT * FROM table WHERE a=1 AND b>3;
```

3、精确匹配最左列并范围匹配非右一列（a值确定时，c排序依赖b，因此无法使用联合索引，但会使用a列索引筛选出a>2的记录行，再在这些行中条件 c >3逐条过滤）

```sql
SELECT * FROM table WHERE a>2 AND c>5;
```





## 2.10 InnoDB的B+索引注意事项

B+树的形成过程：

1. 每当为某个表创建一个B+树索引（聚簇索引不是人为创建的，默认就有）的时候，都会为这个索引创建一个根节点页面。最开始表中没有数据的时候，每个B+树索引对应的根节点中既没有用户记录，也没有目录项记录。
2. 随后向表中插入用户记录时，先把用户记录存储到这个根节点中。
3. 当根节点中的可用空间用完时继续插入记录，此时会将根节点中的所有记录复制到一个新分配的页，比如页a中，然后对这个新页进行页分裂的操作，得到另一个新页，比如页b。这时新插入的记录根据键值（也就是聚簇索引中的主键值，二级索引中对应的索引列的值）的大小就会被分配到页a或者页b中，而根节点便升级为存储目录项记录的页。

> 个过程需要大家特别注意的是：一个B+树索引的根节点自诞生之日起，便不会再移动。这样只要我们对某个表建立一个索引，那么它的根节点的页号便会被记录到某个地方，然后凡是InnoDB存储引擎需要用到这个索引的时候，都会从那个固定的地方取出根节点的页号，从而来访问这个索引。

## 2.11 总结

我们来简单做下总结：

* 每个索引都对应一棵B+树，B+树分为好多层，最末级是叶子节点，其余的是内节点。所有用户记录都存储在B+树的叶子节点，所有目录项记录都存储在内节点。

* InnoDB存储引擎会自动为主键（如果没有它会自动帮我们添加）建立聚簇索引，聚簇索引的叶子节点包含完整的用户记录。

* 我们可以为自己感兴趣的列建立二级索引，二级索引的叶子节点包含的用户记录由索引列 + 主键组成，所以如果想通过二级索引来查找完整的用户记录的话，需要通过回表操作，也就是在通过二级索引找到主键值之后再到聚簇索引中查找完整的用户记录。

* B+树中每层节点都是按照索引列值从小到大的顺序排序而组成了双向链表，而且每个页内的记录（不论是用户记录还是目录项记录）都是按照索引列的值从小到大的顺序而形成了一个单链表。如果是联合索引的话，则页面和记录先按照联合索引前边的列排序，如果该列值相同，再按照联合索引后边的列排序。

* 通过索引查找记录是从B+树的根节点开始，一层一层向下搜索。由于每个页面都按照索引列的值建立了Page Directory（页目录），所以在这些页面中的查找非常快。

* MyISAM会单独为表的主键创建一个索引，只不过在索引的叶子节点中存储的不是完整的用户记录，而是主键值 + 行号的组合。在InnoDB存储引擎中，我们只需要根据主键值对聚簇索引进行一次查找就能找到对应的记录，而在MyISAM中却需要进行一次回表操作，意味着MyISAM中建立的索引相当于全部都是二级索引。

* InnoDB的B+树中的内节点中目录项记录唯一；

* 一个数据页最少存储2条记录。
* InnoDB中的索引即数据，数据即索引，而MyISAM中是索引是索引、数据是数据。



> **哪些情况需要创建索引**
>
> 1. 主键自动建立唯一索引 .
>
> 2. 频繁作为查询条件的字段应该创建索引 .
>
> 3. 查询中与其它表关联的字段，外键关系建立索引 .
> 4. 频繁更新的字段不适合创建索引，因为每次更新不单单是更新了记录，还会更新索引，加重IO负担.
> 5. where条件里用不到的字段不创建索引 .
> 6. 单键/组合索引的选择问题，who？（在高并发下倾向创建组合索引）.
> 7. 查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度 .
> 8. 查询中统计或者分组字段.



>  **哪些情况不需要创建索引**
>
> 1. 表记录太少 
>
> 2. 经常增删改的表     
>
>    Why：提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。 数据重复且分布平均的表字段，因此应该只为最经常查询和最经常排序的数据列建立索引。注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果。



# 3. 性能分析

## 3.1 MySQL Query Optimizer

* Mysql中有专门负责优化 `SELECT ` 语句的优化器模块，主要功能: 通过计算分析系统中收集到的统计信息，为客户端请求的Query提供他认为最优的执行计划 (他认为最优的数据检索方式，但不见得是DBA认为是最优的，这部分最耗费时间)。

* 当客户端向 MySQL 请求一条 Query，命令解析器模块完成请求分类，区别出是 `SELECT` 并转发给MYSQL
  Query Optimizer 时，MVSQL Query Optimizer 首先会对整条 Query 进行优化，处理掉一些常量表达式的预算, 直接换算成常量值。并对 Query 中的查询条件进行简化和转换，如去掉一些无用或显而易见的条件、结构调整等。然后分析 Queny 中的 Hint 信息(如果有)，看显示 Hint 信息是否可以完全确定该 Query 的执行计划。如果没有 Hint 或 Hint 信息还不足以完全确定执行计划，则会读取所涉及对象的统计信息，根据 Query 进行写相应的计算分析，然后再得出最后的执行计划。



## 3.2 MySQL常见瓶颈

* CPU：CPU在饱和的时候一般发生在数据装入内存或从磁盘上读取数据时候。

* IO：磁盘I/O瓶颈发生在装入数据远大于内存容量的时候。

* 服务器硬件的性能瓶颈：top，free，iostat和vmstat来查看系统的性能状态。



## 3.3 Explain

**Explain是什么？**

查看执行计划。使用 `EXPLAIN` 关键字可以模拟优化器执行 SQL 查询语句，从而知道 MySQL 是如何处理你的 SQL 语句的。分析你的查询语句或是表结构的性能瓶颈。

**Explain能干嘛？**

- 表的读取顺序
- 数据读取操作的操作类型
- 哪些索引可以使用
- 哪些索引被实际使用
- 表之间的应用
- 每张表有多少行被优化器查询

**Explain怎么玩？**

- Explain+SQL语句
- 执行计划包含的信息

![](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/20241009203319.png)

**各字段解释**

* **id**：select 查询的序列号，包含一组数字，表示查询中执行 select 子句或操作表的顺序。

  * id相同，执行顺序由上至下。
  * id不同，如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行。
  * id相同不同，同时存在。
  * 衍生：DERIVED

* **select_type**：查询的类型，主要是用于区别普通查询、联合查询、子查询等的复杂查询

  * `SIMPLE`：简单的select查询，查询中不包含子查询或者UNION。
  * `PRIMARY`：查询中包含任何复杂的子部分，最外层查询则被标记为PRIMARY
  * `SUBQUERY`：在FROM列表中包含的子查询被标记为DERIVED（衍生），MySQL会递归执行这些子查询，把结果放在临时表里。
  * `DERIVED`：在FROM列表中包含的子查询被标记为DERIVED（衍生）。MySQL会递归执行这些子查询，把结果放在临时表里。
  * `UNION`：若第二个SELECT出现在UNION之后，则被标记为UNION；若UNION包含在FROM子句的子查询中，外层SELECT将被标记为：DERIVED。
  * `UNION RESULT`：从UNION表中获取结果的SELECT。

* **table**：显示这一行的数据是关于哪些表的。

* **type**：访问类型，是较为重要的一个指标，结果值从最好到最坏依次是：

  ```sql
  system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > All
  ```

  - `system`：表只有一行记录（等于系统表），这是const类型的特例，平时不会出现，这个也可以忽略不计。

  - `const`：表示通过索引一次就找到了，const用于比较primary key或则unique索引。因为只匹配一行数据，所以很快。如将主键置于where列表中，MySQL 就能将该查询转换为一个常量。

  - `eq_ref`：唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描。

  - `ref`：非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，它可能会找到多个符合条件的行，所以它应该属于查找和扫描的混合体。

  - `range`：只检索给定范围的行，使用一个索引来选择行。key 列显示使用了哪个索引。一般就是在你的where 语句中出现了between、<、>、in等的查询。这种范围扫描索引扫描比全表扫描要好，因为它只需要开始于索引的某一点，而结束于另一点，不会扫描全部索引。

  - `index`：Full Index Scan，index 与 All 区别为 index 类型只遍历索引树。这通常比 All 快，因为索引文件通常比数据文件小。（也就是说虽然 all 和 index 都是读全表，但 index 是从索引中读取的，而 all 是从硬盘中读的）

  - `all`：Full Table Scan，将遍历全表以找到匹配的行。

    > 一般来说，得保证查询至少达到range级别，最好能达到ref。

* **possible_keys**：显示可能应用在这张表中的索引，一个或多个。查询涉及到的字段上若存在索引，则该索引将被列出。但不一定被查询实际使用。

* **key**：实际使用的索引。如果为 NULL，则没有使用索引。查询中若使用了覆盖索引，则该索引仅出现在 key 列表中，不会出现在 possible_keys 列表中。（覆盖索引：查询的字段与建立的复合索引的个数一一吻合）

* **key_len**：表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。在不损失精确性的情况下，长度越短越好。key_len 显示的值为索引字段的最大可能长度，并非实际使用长度，即 key_len 是根据表定义计算而得，不是通过表内检索出的。

* **ref**：显示索引的哪一列被使用了，如果可能的话，是一个常数。哪些列或常量被用于查找索引列上的值。查询中与其它表关联的字段，外键关系建立索引。

* **rows**：根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数。

* **Extra**：包含不适合在其他列中显示但十分重要的额外信息。

  * `Using filesort`：说明mysql会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取。MySQL中无法利用索引完成的排序操作成为“文件排序”。
  * `Using temporary`：使用了临时表保存中间结果，MySQL 在对查询结果排序时使用临时表。常见于排序 order by 和分组查询 group by 。
  * `Using index`：表示相应的select操作中使用了覆盖索引（Covering Index），避免访问了表的数据行，效率不错！如果同时出现 using where ，表明索引被用来执行索引键值的查找；如果没有同时出现 using where ，表明索引用来读取数据而非执行查找动作。
  * `Using where`：表明使用了 where 过滤。
  * `Using join buffer`：使用了连接缓存。
  * `impossible where`：where 子句的值总是 false ，不能用来获取任何元组。（查询语句中 where 的条件不可能被满足，恒为False）。
  * `select tables optimized away`：在没有GROUPBY子句的情况下，基于索引优化MIN/MAX操作或者对于MyISAM存储引擎优化COUNT(*)操作，不必等到执行阶段再进行计算，查询执行计划生成的阶段即完成优化。
  * `distinct`：优化 distinct 操作，在找到第一匹配的元组后即停止找相同值的动作。



## 3.4 热身Case

![](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/20241009205133.png)

![](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/20241009205148.png)



# 4. 索引优化

## 4.1 索引分析

### 4.1.1单表分析

建表语句:

```sql
CREATE TABLE IF NOT EXISTS `article` (
    `Id`          INT(10) UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
    `author_id`   INT(10) UNSIGNED NOT NULL,
    `category_id` INT(10) UNSIGNED NOT NULL,
    `views`       INT(10) UNSIGNED NOT NULL,
    `comments`    INT(10) UNSIGNED NOT NULL,
    `title`       VARBINARY(255)   NOT NULL,
    `content`     TEXT             NOT NULL
);
```

插入数据：

```sql
INSERT INTO article(author_id, category_id, views, comments, title, content) VALUES (1, 1, 1, 1,'1','1'),(2, 2, 2, 2, '2', '2'),(1, 1, 3, 3, '3','3');
```

查询 category_id为 1 且 comments 大于1的情况下, views 最多的 article_id。

```sql
EXPLAIN SELEcT id,author_id FROM article WHERE category_id = 1 AND comments > 1 ORDER BY views DESC LIMIT 1;
```

![image-20241010141513827](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010141513827.png)

 很显然, type 是 ALL,即最坏的情况。Extra 里还出现了 Using filesort,也是最坏的情况。优化是必须的。



**开始优化 >> **

新建索引：

```sql
create index idx_article_ccv on article(category_id, comments, views);
```

重新 explain：

```sql
EXPLAIN SELEcT id,author_id FROM article WHERE category_id = 1 AND comments > 1 ORDER BY views DESC LIMIT 1;
```

![image-20241010150340960](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010150340960.png)

结论:

type 变成了 range,这是可以忍受的。但是 extra 里使用 Using filesort 仍是无法接受的。但是我们已经建立了素引，为啥没用呢？这是因为按照 BTree 索引的工作原理，先排序 category_id，如果遇到相同的 category_id 则再排序 comments, 如果遇到相同的 comments 则再排序 views。当 comments 字段在联合素引里处于中间位置时，因comments>1条件是一个范围值(所谓 range)，MySQL 无法利用索引再对后面的 views 部分进行检索,即 range 类型查询字段后面的索引无效。



### 4.1.2 两表分析

建表语句：

```sql
CREATE TABLE IF NOT EXISTS `class`(
  `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `card` INT(10) UNSIGNED NOT NULL,
  PRIMARY KEY(`id`)
);

CREATE TABLE IF NOT EXISTS `book`(
  `bookId` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `Card` INT(10) UNSIGNED NOT NULL,
  PRIMARY KEY (`bookid`)
);
```

插入数据：

```sql
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
INSERT INTO class(card) VALUES(FLOOR(1 +(RAND()*20)));
```

下面开始explain分析：

```sql
EXPLAIN SELECT * FROM class LEFT JOIN book ON class.card = book.card;
```

![image-20241010151530978](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010151530978.png)

我们发现 type 有 AII。



接下来，添加索引优化：

```sql
ALTER TABLE `book` ADD INDEX Y(`card`);
```

第2次explain:

```sql
EXPLAIN SELECT * FROM class LEFT JOIN book ON class.card = book.card;
```

![image-20241010151744320](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010151744320.png)

可以看到第二行的 type 变为了 ref，rows 也变成了优化比较明显。这是由左连接特性决定的。LEFT JOIN 条件用于确定如何从右表搜索行,左边一定都有。所以右边是我们的关键点，一定需要建立索引。



接下来，删除旧索引 + 新建 + 第3次 explain。

```sql
DROP INDEX Y ON book;

ALTER TABLE class ADD INDEX X(card);

EXPLAIN SELECT * FROM class LEFT JOIN book ON class.card = book.card;
```

![image-20241010152257580](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010152257580.png)



然后来看一个右连接查询:

```sql
EXPLAIN SELECT * FROM class RIGHT JOIN book ON class.card = book.card;
```

![image-20241010152726175](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010152726175.png)

优化较明显。这是因为 right join 条件用于确定如何从左表搜索行，右边一定都有，所以左边是我们的关键点，一定需要建立索引。



换成右表建立索引：

```sql
DROP INDEX X ON class;
ALTER TABLE book ADD INDEX Y(card);
EXPLAIN SELECT * FROM class RIGHT JOIN book ON class.card = book.card;
```

![image-20241010152913267](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010152913267.png)

右连接，基本无变化。



> **左连接建右表，右连接建左表**。以左连接为例，左表的信息全都有，所以右表需要查找，所以为右表建立索引。



### 4.1.3 join语句的优化

- 尽可能减少 join 语句中的 NestedLoop 的循环总次数：“永远用小结果集驱动大的结果集”。
- 优先优化 NestedLoop 的内层循环。
- 保证 join 语句中被驱动表上 join 条件字段已经被索引。
- 当无法保证被驱动表的 join 条件字段被索引且内存资源充足的前提下，不要太吝惜 joinBuffer 的设置。



1. 如果索引了多列，要遵守最左前缀法则。指的是查询从索引的最左前列开始并且不跳过索引中的列。（带头大哥不能死，中间兄弟不能断）
2. 不在索引列上作任何操作（计算、函数、（自动or手动）类型转换），会导致索引失效而转向全表扫描
3. 存储引擎不能使用索引中范围条件右边的列。
4. 中间兄弟别搞范围，要搞等值
5. 尽量使用覆盖索引（只访问索引的查询（索引列和查询列一致）），减少select *
6. mysql在使用不等于（！=或者<>）的时候无法使用索引会导致全表扫描
7. is null，is not null也无法使用索引
8. like以通配符开头(‘%abc…’)mysql索引失效会变成全表扫描的操作。
9. or 会索引失效。

![img](https://i-blog.csdnimg.cn/blog_migrate/0ce5c5a250ac92f3ea205c54e019ecae.png#pic_center)



### 4.1.4 order by优化

ORDER BY 子句，尽量使用 Index 方式排序(指排序走索引)，避免使用 FileSort 方式排序。

如果对 where 条件字段未建索引，只对排序字段建索引，是不会使用索引的。



MySQL支持二种方式的排序，FileSort和Index，**Index效率高，它指MySQL扫描索引本身完成排序**，FileSort方式效率较低。

ORDER BY满足两情况（最佳左前缀原则），会使用Index方式排序。

ORDER BY语句使用索引最左前列。

使用where子句与OrderBy子句条件列组合满足索引最左前列。

尽可能在索引列上完成排序操作，遵照索引建的**最佳左前缀.**

**如果未在索引列上完成排序，mysql 会启动 filesort 的两种算法：双路排序和单路排序**。



**双路排序**:

MySQL4.1之前是使用双路排序，字面意思是两次扫描磁盘，最终得到数据。读取行指针和将要进行 order by 操作的列，对他们进行排序，然后扫描已经排序好的列表，按照列表中的值重新从列表中读取对应的数据传输。

从磁盘取排序字段，在buffer进行排序，再从磁盘取其他字段。



**单路排序**

取一批数据，要对磁盘进行两次扫描，众所周知，I/O是很耗时的，所以在mysql4.1之后，出现了改进的算法，就是单路排序。

从磁盘读取查询需要的所有列，按照将要进行 order by 的列，在 sort buffer 对它们进行排序，然后扫描排序后的列表进行输出，它的效率更快一些，避免了第二次读取数据，并且把随机 IO 变成顺序 IO，但是它会使用更多的空间，因为它把每一行都保存在内存中了。



结论及引申出的问题：

1. 由于单路是改进的算法，总体而言好过双路。
2. n 在 sort_buffer 中，方法 B 比方法 A 要多占用很多空间，因为方法 B 是把所有字段都取出，所以有可能取出的数据的总大小超出了 sort_buffer 的容量，导致每次只能取 sort_buffer 容量大小的数据，进行排序（创建 tmp 文件，多路合并），排完再取取 sort_buffer 容量大小，再排…… 从而会导致多次 I/O。
3. 本来想省一次I/O操作，反而导致了大量的/O操作，反而得不偿失。



**遵循如下规则，可提高Order By的速度**

order by 时 select * 是一个大忌，只 Query 需要的字段，这点非常重要。
当 Query 的字段大小总和小于 max_length_for_sort_data，而且排序字段不是 TEXT|BLOB 类型时，会用改进后的算法——单路排序，否则用老算法——多路排序。

l两种算法的数据都有可能超出 sort_buffer 的容量，超出之后，会创建 tmp 文件进行合并排序，导致多次 I/O ，但是用单路排序算法的风险会更大一些，所以要提高 sort_buffer_size 。

尝试提高 sort_buffer_size不管用哪种算法，提高这个参数都会提高效率，当然，要根据系统的能力去提高，因为这个参数是针对每个进程的。

尝试提高max_length_for_sort_data提高这个参数，会增加用改进算法的概率。但是如果设的太高，数据总容量超出sort_buffer_size的概率就增大，明显症状是高的磁盘I/O活动和低的处理器使用率。

![image-20241011005224027](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241011005224027.png)



### 4.1.5 group by 优化

group by 实质是**先排序后进行分组，遵照索引的最佳左前缀**。

当无法使用索引列，增大max_length_for_sort_data参数的设置+增大sort_buffer_size参数的设置。

where高于having，能写在where限定的条件就不要去having限定了。

其余的规则均和 order by 一致。



# 5. 慢查询日志

MySQL 的慢查询日志是 MySQL 提供的一种日志记录，它用来记录在 MySQL 中响应时间超过阈值的语句，具体指运行时间超过 long_query_time 值的SQL，则会被记录到慢查询日志中。long_query_time 的默认值是10，意思是运行10秒以上的语句。由它来查看哪些 SQL 超出了我们的最大忍耐时间值，比如一条 sql 执行超过 5 秒钟，我们就算慢 SQL ，希望能收集超过 5 秒的 sql ，结合之前的 explain 进行全面分析。

默认情况下，MySQL数据库没有开启慢查询日志，需要我们手动来设置这个参数。当然，如果不是调优需要的话，一般不建议启动该参数，因为开启慢查询日志会或多或少带来一定的性能影响。慢查询日志支持将日志记录写入文件。

**查看是否开启慢查询日志：**

```sql
SHOW VARIABLES LIKE ‘%slow_query_log%’;
```

**开启慢查询日志：**

```sql
set global slow_query_log=1;
```

使用 `set global slow_query_log=1`  开启慢查询日志，只对当前数据库生效。如果 Mysql 重启后则会失效。

如果要永久生效，就必须修改配置文件my.cnf(其它系统变量也是如此)。

修改 my.cnf 文件，[mysqld] 下增加或修改参数 `slow_query_log` 和 `slow_query_log_file` 后，然后重启    MySQL 服务器。也即将如下两行配置进 my.cnf 文件。

```sql
slow query log =1
slow query log file=/var/lib/mysql/atguigu-slow.log
```

关于慢查询的参数 `slow_query_log_file`，它指定慢查询日志文件的存放路径，系统默认会给一个缺省的文件host_name-slow.log (如果没有指定参数 `slow_query_log_file` 的话)。

那么开启了慢查询日志后，什么样的SQL才会记录到慢查询日志里面呢？

这个是由参数 `long_query_time` 控制，默认情况下 `long_query_time` 的值为 10 秒,

```sql
SHOW VARIABLES LIKE 'long_query_time%';
```

![image-20241010232605546](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241010232605546.png)

可以使用命令修改，也可以在 my.cnf 参数里面修改。

假如运行时间正好等于 `long_query_time` 的情况，并不会被记录下来。也就是说在 mysql 源码里是判断大于`long_query_time`，而非大于等于。

**设置慢查询的阈值时间：**

```sql
set global long_query_time=3;
```

**查询当前系统中有多少条慢查询记录：**

```sql
show global status like '%slow_queries%';
```



#  6. 日志分析工具mysqldumpslow

在生产环境中，如果要手工分析日志，查找、分析SQL，显然是个体力活，MySQL提供了日志分析工具 mysqldumpslow。

查看mysqldumpslow的帮助信息：

```bash
mysqldumpslow --help
```

- s：是表示按照何种方式排序
- c：访问次数
- I：锁定时间
- r：返回记录
- t：查询时间
- al：平均锁定时间
- ar：平均返回记录数
- at：平均查询时间
- t：即为返回前面多少条的数据
- g：后边搭配一个正则匹配模式，大小写不敏感

工作常用参考:

```bash
# 得到返回记录集最多的10个SQL
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log

#得到访问次数最多的10个SQL
mysqldumpslow -s c -t 10 /var/lib/mysql/atguigu-slow.log
#得到按照时间排序的前10条里面含有左连接的查询语句
mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/atguigu-slow.log
#另外建议在使用这些命令时结合|和 more 使用，否则有可能出现爆屏情况
mysqldumpslow -s r -t 10 /var/lib/mysgl/atguigu-slow.log | more
```



# 7. Show Profile

Show Profile 是 mysql 提供的可以用来分析当前会话中语句执行的资源消耗情况。可以用于 SQL 调优的测量。

默认情况下，参数处于关闭状态，并保存最近15次的运行结果。

**分析步骤：**

1. 是否支持，看看当前的mysql版本是否支持

   ```sql
   show variables like 'profiling';
   ```

2. 开启

   ```sql
   set profiling = on;
   ```

3. 运行SQL

   ```sql
   select * from emp group by id%10 limit 150000;
   select * from emp group by id%20 order by 5;
   ```

4. 查看结果：`show profiles;`

5. 诊断 SQL：`show profile cpu, block io for query [上一步前面的问题SQL数字号码];`

**type:**

* `ALL`: 显示所有的开销信息CONTEXT SWITCHES -上下文切换相关开销。
* `BLOCK IO`：显示块IO相关开销。
* `CONTEXT SWITCHES`: 上下文切换相关开销。
* `CPU`: 显示CPU相关开销信息。
* `IPC`: 显示发送和接收相关开销信息
* `MEMORY`: 显示内存相关开销信息。
* `PAGE FAULTS`: 显示页面错误相关开销信息。
* `SOURCE`: 显示和Source function, Source file, Source line 相关的开销信息。
* `SWAPS`: 显示交换次数相关开销的信息。



# 8. 全局查询日志

**通过配置启用：**

​	在 mysql 的 my.cnf 中，设置如下:

​	开启: `general_log=1`

​	记录日志文件的路径: `general_log_file=/path/logfile`

​	输出格式: `log_output=FILE`

**通过编码启用:**

```sql
set global_general_log=1;
set global_log_output='TABLE';
```

此后 ，你所编写的 sql 语句，将会记录到 mysql 库里的 generallog 表，可以用下面的命令查看:

```sql
select * from mysgl.general_log;
```

> **永远不要在生产环境开启这个功能！**



# 9. MySQL锁机制

锁是计算机协调多个进程或线程并发访问某一资源的机制。

在数据库中，除传统的计算资源 (如 CPU、RAM、I/0 等) 的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。



## 9.1 锁的分类

**从对数据操作的类型（读/写）分：**

- 读锁（共享锁）：针对同一份数据，多个读操作可以同时进行而不会互相影响。
- 写锁（排它锁）：当前写操作没有完成前，它会阻断其他写锁和读锁。

**从对数据操作的粒度分：**

- 表锁
- 行锁

为了尽可能提高数据库的并发度，每次锁定的数据范围越小越好，理论上每次只锁定当前操作的数据的方案会得到最大的并发度，但是管理锁是很耗资源的事情（涉及获取，检查，释放锁等动作），因此数据库系统需要在高并发响应和系统性能两方面进行平衡，这样就产生了“锁粒度（Lock granularity）”的概念。

一种提高共享资源并发性的方式是让锁定对象更有选择性。尽量只锁定需要修改的部分数据，而不是所有的资源。更理想的方式是只对修改的数据片进行精确的锁定。任何时候，在给定的资源上，锁定的数据量越少，则系统的并发程度越高，只要相互之间不发生冲突即可。



## 9.2 表锁（偏读）

特点：偏向 MyISAM 存储引擎，开销小，加锁快；无死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。

> 读锁会阻塞写，但是不会阻塞读。而写锁则会把读和写都阻塞



## 9.3 行锁（偏写）

特点：偏向Innodb存储引擎，开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率最低，并发度也最高。

Innodb 与 MyISAM 的最大不同有两点：

- 一是支持事务（TRANSACTION）
- 二是采用了行级锁

> **没有索引或者索引失效时，InnoDB 的行锁会升级为表锁！**
>
> **原因：Mysql 的行锁是通过索引实现的！**



## 9.4 事务

事务是由一组SQL语句组成的逻辑处理单元，事务具有以下4个属性，通常简称为事务的ACID属性

* **原子性**（Atomicity）：事务是一个原子操作单元，其对数据的修改，要么全都执行，要么全都不执行。
* **一致性**（Consistent）：在事务开始和完成时，数据都必须保持一致状态。这意味着所有相关的数据规则都必须应用于事务的修改，以保持数据的完整性；事务结束时，所有的内部数据结构（如B树索引或双向链表）也都必须是正确的。
* **隔离性**（Isolation）：数据库系统提供一定的隔离机制，保证事务在不受外部并发操作影响的“独立”环境执行。这意味着事务处理过程中的中间状态对外部是不可见的，反之亦然。
* **持久性**（Durable）：事务完成之后，它对于数据的修改是永久性的，即使出现系统故障也能够保持。

由于行锁支持事务，因此并发事务处理就会随之带来问题。


**更新丢失(Lost Update)**

当两个或多个事务选择同一行，然后基于最初选定的值更新该行时，由于每个事务都不知道其他事务的存在，就会发生丢失更新问题－－最后的更新覆盖了由其他事务所做的更新。

例如，两个程序员修改同一java文件。每程序员独立地更改其副本，然后保存更改后的副本，这样就覆盖了原始文档。最后保存其更改副本的编辑人员覆盖前一个程序员所做的更改。

如果在一个程序员完成并提交事务之前，另一个程序员不能访问同一文件，则可避免此问题。



**脏读(Dirty Reads)**

一个事务正在对一条记录做修改，在这个事务完成并提交前，这条记录的数据就处于不一致状态；这时，另一个事务也来读取同一条记录，如果不加控制，第二个事务读取了这些“脏”数据，并据此做进一步的处理，就会产生未提交的数据依赖关系。这种现象被形象地叫做”脏读”。

一句话：事务A读取到了事务B已修改但尚未提交的的数据，还在这个数据基础上做了操作。此时，如果B事务回滚，A读取的数据无效，不符合一致性要求。



**不可重复读(Non-Repeatable Reads)**

在一个事务内，多次读同一个数据。在这个事务还没有结束时，另一个事务也访问该同一数据。那么，在第一个事务的两次读数据之间。由于第二个事务的修改，那么第一个事务读到的数据可能不一样，这样就发生了在一个事务内两次读到的数据是不一样的，因此称为不可重复读，即原始读取不可重复。

一句话：一个事务范围内两个相同的查询却返回了不同数据。



**幻读(Phantom Reads)**

一个事务按相同的查询条件重新读取以前检索过的数据，却发现其他事务插入了满足其查询条件的新数据，这种现象就称为“幻读”。

一句话：事务A 读取到了事务 B 提交的新增数据，不符合隔离性。



> 脏读是读到了别人修改未提交的数据（可能随时回滚），
>
> 幻读是读到了其他线程新增的数据



## 9.5 事务隔离级别

“脏读”、“不可重复读”和“幻读”，其实都是数据库读一致性问题，必须由数据库提供一定的事务隔离机制来解决。

| 隔离级别 | 读数据一致性                             | 脏读 | 不可重复读 | 幻读 |
| -------- | ---------------------------------------- | ---- | ---------- | ---- |
| 未提交读 | 最低级别，只能保证不读取物理上损坏的数据 | 是   | 是         | 是   |
| 已提交读 | 语句级                                   | 否   | 是         | 是   |
| 可重复读 | 事务级                                   | 否   | 否         | 是   |
| 可序列化 | 最高级别，事务级                         | 否   | 否         | 否   |

数据库的事务隔离越严格，并发副作用越小，但付出的代价也就越大，因为事务隔离实质上就是使事务在一定程度上 “串行化”进行，这显然与“并发”是矛盾的。同时，不同的应用对读一致性和事务隔离程度的要求也是不同的，比如许多应用对“不可重复读”和“幻读”并不敏感，可能更关心数据并发访问的能力。

查看当前数据库的事务隔离级别：

```sql
show variables like 'tx_isolation';
```

> **mysql** **默认是可重复读 RR（** **实现原理（MVCC [ 多版本并发控制 ]）**



**MVCC扩展：**

InnoDB 在每行记录后面保存两个隐藏的列来，分别保存了这个行的创建时间和行的更新时间。这里存储的并不是实际的时间值, 而是系统版本号，当数据被修改时，版本号加1。

在读取事务开始时，系统会给当前读事务一个版本号，事务会读取版本号<=当前版本号的数据。

此时如果其他写事务修改了这条数据，那么这条数据的版本号就会加1，从而比当前读事务的版本号高，读事务自然而然的就读不到更新后的数据了。



**注意**：在mysql中，提供了两种事务隔离技术，第一个是 mvcc，第二个是 next-key 技术。这个在使用不同的语句的时候可以动态选择。不加 lock in share mode 之类的就使用 mvcc。否则使用 next-key。mvcc 的优势是不加锁，并发性高。缺点是不是实时数据。next-key 的优势是获取实时数据，但是需要加锁。



在RR级别下，**快照读**是通过MVVC(多版本控制)和undo log来实现的，**当前读**是通过加record lock(记录锁)和gap lock(间隙锁)来实现的。

以从上面的显示来看，如果需要实时显示数据，还是需要通过加锁来实现。这个时候会使用next-key技术来实现。

**快照读**：

​      简单的select操作。

**当前读** 

​      select ... lock in share mode

​      select ... for update

​      insert

​      update

​      delete



## 9.6 间隙锁

**什么是间隙锁?**

当我们用范围条件而不是等值条件检索数据，并请求共享或排他锁时，InnoDB 会给符合条件的已有数据记录的索引项加锁；对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”，InnoDB 也会对这个“间隙”加锁，这种锁机制就是所谓的间隙锁（GAP Lock）。

**间隙锁的危害？**
因为 Query 执行过程中通过范围查找的话，他会锁定整个范围内所有的索引键值，即使这个键值并不存在。间隙锁有一个比较致命的弱点，就是当锁定一个范围键值之后，即使某些不存在的键值也会被无辜的锁定，而造成在锁定的时候无法插入锁定键值范围内的任何数据。在某些场景下这可能会对性能造成很大的危害。



## 9.7 读锁(共享锁）

共享锁又称读锁，是读取操作创建的锁。其他用户可以并发读取数据，但任何事务都不能对数据进行修改（获取数据上的排他锁），直到已释放所有共享锁。

如果事务 T 对数据 A 加上共享锁后，则其他事务只能对 A 再加共享锁，不能加排他锁。获取到共享锁的事务只能读数据，不能修改数据。

用法：

```sql
SELECT ... LOCK IN SHARE MODE;
```

在查询语句后面增加 `LOCK IN SHARE MODE` ，Mysql 会对查询结果中的每行都加共享锁***，***当没有其他线程对查询结果集中的任何一行使用排他锁时，可以成功申请共享锁，否则会被阻塞。其他线程也可以读取使用了共享锁的行，而且这些线程读取的是同一个版本的数据。



## 9.8 写锁(排他锁)

排他锁又称写锁，如果事务 T 对数据 A 加上排他锁后，则其他事务不能再对 A 加任任何类型的锁。获取到排他锁的事务既能读数据，又能修改数据。

用法：

```sql
SELECT ... FOR UPDATE;
```

在查询语句后面增加 `FOR UPDATE` ，Mysql 会对查询结果中的每行都加排他锁，当没有其他线程对查询结果集中的任何一行使用排他锁时，可以成功申请排他锁，否则会被阻塞。



## 9.9 行锁分析

如何分析行锁定?

通过检查 InnoDB_row_lock 状态变量来分析系统上的行锁的争夺情况：

```sql
show status like 'innodb_row_lock%';
```

对各个状态量的说明如下：

* `Innodb_row_lock_current_waits`：当前正在等待锁定的数量；
* `Innodb_row_lock_time`：从系统启动到现在锁定总时间长度；
* `Innodb_row_lock_time_avg`：每次等待所花平均时间；
* `Innodb_row_lock_time_max`：从系统启动到现在等待最常的一次所花的时间；
* `Innodb_row_lock_waits`：系统启动后到现在总共等待的次数；

对于这5个状态变量，比较重要的主要是:

* `Innodb_row_lock_time_avg`（等待平均时长），

* `Innodb_row_lock_waits`（等待总次数）

* `Innodb_row_lock_time`（等待总时长）这三项。



尤其是当等待次数很高，而且每次等待时长也不小的时候，我们就需要分析系统中为什么会有如此多的等待，然后根据分析结果着手指定优化计划。

最后可以通过 `SELECT * FROM information_schema.INNODB_TRX\G;` 来查询正在被锁阻塞的 sql 语句。

**优化建议**:

* 尽可能让所有数据检索都通过索引来完成，避免无索引行锁升级为表锁。
* 尽可能较少检索条件，避免间隙锁
* 尽量控制事务大小，减少锁定资源量和时间长度
* 锁住某行后，尽量不要去调别的行或表，赶紧处理被锁住的行然后释放掉锁。
* 涉及相同表的事务，对于调用表的顺序尽量保持一致。
* 在业务环境允许的情况下,尽可能低级别事务隔离



## 9.10 页锁

开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。



# 10. 主从复制

**主从复制的基本原理**

![image-20241011005619687](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/image-20241011005619687.png)

MySQL复制过程分成三步:

1. master将改变记录到二进制日志(binary log)。这些记录过程叫做二进制日志事件，binary log events。

2. slave 将 master 的 binary log events 拷贝到它的中继日志(relay log)。
3. slave重做中继日志中的事件，将改变应用到自己的数据库中。 MySQL复制是异步的且串行化的。



**复制的基本原则**

- 每个slave只有一个master。
- 每个slave只能有一个唯一的服务器ID。

- 每个master可以有多个slave。
- 
- 
  <font style="color:#325db1;">Java8则只将永久代变成了元空间</font>
