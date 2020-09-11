<details>
 <summary><b>select * from table where (ID = 10) or (ID = 32) or (ID = 22) 让结果按10, 32, 22的顺序检索出来？</b></summary>

```
Select *
from user_info
Where (ID IN (10, 32, 22))

order BY FIND_IN_SET(ID, '10, 32, 22')
 ```

</details>



<details>
 <summary><b>MySQL中myisam与innodb的区别，至少5点</b></summary>

1. InnoDB支持事物，而MyISAM不支持事物
2. InnoDB支持行级锁，而MyISAM支持表级锁
3. InnoDB支持MVCC, 而MyISAM不支持
4. InnoDB支持外键，而MyISAM不支持
5. InnoDB不支持全文索引，而MyISAM支持。
6. InnoDB不能通过直接拷贝表文件的方法拷贝表到另外一台机器， myisam 支持
7. InnoDB表支持多种行格式， myisam 不支持
8. InnoDB是索引组织表， myisam 是堆表

</details>



<details>
 <summary><b>myisam与innodb select  count(*)哪个更快，为什么</b></summary>

>myisam更快，因为myisam内部维护了一个计数器，可以直接调取。

</details>



<details>
 <summary><b>myisam与innodb 的索引的实现方式</b></summary>

>都是 B+树索引， Innodb 是索引组织表， myisam 是堆表， 索引组织表和堆表的区别要熟悉

</details>



<details>
 <summary><b>MySQL中InnoDB支持的四种事务隔离基本要素，级别名称，以及逐级之间的区别？</b></summary>

##### (1)、事务的基本要素（ACID）
1. 原子性（Atomicity）：事务开始后所有操作，要么全部做完，要么全部不做，不可能停滞在中间环节。事务执行过程中出错，会回滚到事务开始前的状态，所有的操作就像没有发生一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位。
2. 一致性（Consistency）：事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到。
3. 隔离性（Isolation）：同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。
4. 持久性（Durability）：事务完成后，事务对数据库的所有更新将被保存到数据库，不能回滚。

##### (2)、事物的4种隔离级别

1. 读未提交(read uncommitted)
2. 读已提交(read committed)
3. 可重复读(repeatable read)
4. 串行(serializable)

##### (3)、不同级别的现象

1. Read Uncommitted:可以读取其他 session 未提交的脏数据。
2. Read Committed:允许不可重复读取，但不允许脏读取。提交后，其他会话可以看到提交的数据。
3. Repeatable Read: 禁止不可重复读取和脏读取、以及幻读(innodb 独有)。
4. Serializable: 事务只能一个接着一个地执行，但不能并发执行。事务隔离级别最高。
>mysql默认的事务隔离级别为repeatable-read，不同的隔离级别有不同的现象，并有不同的锁定/并发机制，隔离级别越高，数据库的并发性就越差。

<img src="./images/386.png" width="700" height="240" align=center> 

1. 脏读：一个事务读取到另一事务未提交的更新数据
2. 不可重复读 : 在同一事务中,多次读取同一数据返回的结果有所不同, 换句话说, 后续读取可以读到另一事务已提交的更新数据. 
3. 可重复读：在同一事务中多次读取数据时, 能够保证所读数据一样, 也就是后续读取不能读到另一事务已提交的更新数据。
4. 幻读：一个事务读到另一个事务已提交的insert数据
</details>



<details>
 <summary><b>MySQL都有哪些锁呢</b></summary>

```
从锁的类别上来讲,有共享锁和排他锁.

共享锁: 又叫做读锁. 当用户要进行数据的读取时,对数据加上共享锁.共享锁可以同时加上多个.

排他锁: 又叫做写锁. 当用户要进行数据的写入时,对数据加上排他锁.排他锁只可以加一个,他和其他的排他锁,共享锁都相斥.

用上面的例子来说就是用户的行为有两种,一种是来看房,多个用户一起看房是可以接受的. 一种是真正的入住一晚,在这期间,无论是想入住的还是想看房的都不可以.

锁的粒度取决于具体的存储引擎,InnoDB实现了行级锁,页级锁,表级锁.

他们的加锁开销从大到小,并发能力也是从大到小.
```

</details>



<details>
 <summary><b>字段为什么要求定义为not null?</b></summary>

MySQL官网这样介绍:
>NULL columns require additional space in the rowto record whether their values are NULL. For MyISAM tables, each NULL columntakes one bit extra, rounded up to the nearest byte.
null值会占用更多的字节,且会在程序中造成很多与预期不符的情况.


</details>



<details>
 <summary><b>关心过业务系统里面的sql耗时吗?统计过慢查询吗?对慢查询都怎么优化过?</b></summary>

1. 首先分析语句,看看是否load了额外的数据,可能是查询了多余的行并且抛弃掉了,可能是加载了许多结果中并不需要的列,对语句进行分析以及重写.
2. 分析语句的执行计划,然后获得其使用索引的情况,之后修改语句或者修改索引,使得语句可以尽可能的命中索引.
3. 如果对语句的优化已经无法进行,可以考虑表中的数据量是否太大,如果是的话可以进行横向或者纵向的分表.

</details>



<details>
 <summary><b>横向分表和纵向分表,可以分别举一个适合他们的例子吗?</b></summary>

>横向分表是按行分表.假设我们有一张用户表,主键是自增ID且同时是用户的ID.数据量较大,有1亿多条,那么此时放在一张表里的查询效果就不太理想.我们可以根据主键ID进行分表,无论是按尾号分,或者按ID的区间分都是可以的. 假设按照尾号0-99分为100个表,那么每张表中的数据就仅有100w.这时的查询效率无疑是可以满足要求的.

>纵向分表是按列分表.假设我们现在有一张文章表.包含字段id-摘要-内容.而系统中的展示形式是刷新出一个列表,列表中仅包含标题和摘要,当用户点击某篇文章进入详情时才需要正文内容.此时,如果数据量大,将内容这个很大且不经常使用的列放在一起会拖慢原表的查询速度.我们可以将上面的表分为两张.id-摘要,id-内容.当用户点击详情,那主键再来取一次内容即可.而增加的存储量只是很小的主键字段.代价很小.

</details>



<details>
 <summary><b>三个范式</b></summary>

1. 第一范式: 每个列都不可以再拆分. 
2. 第二范式: 非主键列完全依赖于主键,而不能是依赖于主键的一部分. 
3. 第三范式: 非主键列只依赖于主键,不依赖于其他非主键.
>在设计数据库结构的时候,要尽量遵守三范式,如果不遵守,必须有足够的理由.比如性能. 事实上我们经常会为了性能而妥协数据库的设计.

</details>



<details>
 <summary><b></b></summary>



</details>



<details>
 <summary><b></b></summary>



</details>



<details>
 <summary><b></b></summary>



</details>