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
 <summary><b>MySQL中InnoDB支持的四种事务隔离级别名称，以及逐级之间的区别？</b></summary>

##### (1)、事物的4种隔离级别

1. 读未提交(read uncommitted)
2. 读已提交(read committed)
3. 可重复读(repeatable read)
4. 串行(serializable)

##### (2)、不同级别的现象

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
 <summary><b></b></summary>



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



<details>
 <summary><b></b></summary>



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