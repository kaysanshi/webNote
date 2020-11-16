## Mysql：

### 字符集和排序规则

排序规则名称以与其关联的字符集的名称开头，通常后跟一个或多个表示其他排序规则特征的后缀。 例如，utf8 general ci 和 latin1瑞典 ci 分别是 utf8和 latin1字符集的排序规则。 二进制字符集有一个排序规则，也称为二进制，没有后缀。

特定于语言的排序规则包括语言名称。 例如，utf8土耳其 ci 和 utf8匈牙利 ci sort 字符用于 utf8字符集，分别使用土耳其和匈牙利的规则

排序规则后缀表示排序规则是区分大小写、区分重音、区分假名(或者它们的某种组合)还是二进制。 下表显示了用于表示这些特征的后缀

| Suffix 后缀 | Meaning 意义                  |
| ----------- | ----------------------------- |
| `_ai`       | Accent-insensitive 口音不敏感 |
| `_as`       | Accent-sensitive 口音敏感     |
| `_ci`       | Case-insensitive 不区分大小写 |
| `_cs`       | Case-sensitive 区分大小写     |
| `_bin`      | Binary 二进制                 |

​       对于不指定重音敏感性的非二进制排序规则名称，它由大小写敏感性确定。 如果排序名称不包含 ai 或 as，则名称中的 ci 意味着 ai，名称中的 cs 意味着 ai。 例如，latin1 general ci 显式地区分大小写和隐式地区分重音，latin1 general cs 显式地区分大小写和隐式地区分重音。

##### 服务器字符集

Mysql 服务器有一个服务器字符集和一个服务器排序规则。 可以在服务器启动时在命令行上设置或在选项文件中设置，并在运行时进行更改。最初，服务器字符集和排序规则取决于启动 mysqld 时使用的选项。 可以对字符集使用 -- character-set-server。 除此之外，还可以添加 -- collation-server 进行排序。 如果没有指定字符集，就等于说 -- character-set-server latin1。 如果您只指定一个字符集(例如，latin1) ，而没有指定排序规则，那么这就等于说 -- character-set-server latin1 -- collination-server latin1，因为 latin1是 latin1的默认排序规则。 因此，以下三个命令都具有相同的效果

mysqld

 mysqld --character-set-server=latin1 

mysqld --character-set-server=latin1  --collation-server=latin1_swedish_ci

##### 数据库字符集和排序规则

​     每个数据库都有一个数据库字符集和数据库排序规则。 Create DATABASE 和 ALTER DATABASE 语句具有用于指定数据库字符集和排序规则的可选子句：

```sql
CREATE DATABASE db_name
    [[DEFAULT] CHARACTER SET charset_name]
    [[DEFAULT] COLLATE collation_name]
ALTER DATABASE db_name
    [[DEFAULT] CHARACTER SET charset_name]
    [[DEFAULT] COLLATE collation_name]
```

可以使用关键字 SCHEMA 代替 DATABASE.Character SET 和 COLLATE 子句使得在同一个 MySQL 服务器上创建具有不同字符集和排序规则的数据库成为可能.

Mysql 以下列方式选择数据库字符集和数据库排序规则:

- 如果同时指定了 CHARACTER SET 字符集名称和 COLLATE 排序规则名称，则使用字符集字符集名称和排序规则名称。
- 如果在没有 COLLATE 的情况下指定 CHARACTER SET 字符集名称，则使用字符集字符集名称及其默认排序规则。 若要查看每个字符集的默认排序规则，请使用 SHOW CHARACTER SET 语句或查询 informationschemacharacter sets 表
- 如果指定的 COLLATE 排序规则名称没有使用 CHARACTER SET，则使用与排序规则名称和排序规则名称关联的字符集
- 否则(既未指定 CHARACTER SET 也未指定 COLLATE) ，将使用服务器字符集和服务器排序规则

可以根据字符集数据库和排序规则数据库系统变量的值确定默认数据库的字符集和排序规则。 每当缺省数据库发生更改时，服务器都会设置这些变量。 如果没有缺省数据库，则变量的值与相应的服务器级系统变量、字符集服务器和排序规则服务器的值相同

要查看给定数据库的默认字符集和排序规则:

```sql
SELECT DEFAULT_CHARACTER_SET_NAME, DEFAULT_COLLATION_NAME
FROM INFORMATION_SCHEMA.SCHEMATA WHERE SCHEMA_NAME = 'db_name';
```

**数据库字符集和排序影响服务器操作的这些方面:**

- 对于 CREATE TABLE 语句，如果未指定表字符集和排序规则，则数据库字符集和排序规则将用作表定义的默认值。 若要重写此选项，请提供显式的 CHARACTER SET 和 COLLATE 表选项

- 对于不包含 CHARACTER SET 子句的 loaddata 语句，服务器使用字符集数据库系统变量指示的字符集来解释文件中的信息。 若要覆盖此操作，请提供一个显式的 CHARACTER SET 子句。

- 对于存储例程(过程和函数) ，数据库字符集和在例程创建时生效的排序规则被用作字符集和字符数据参数的排序规则，声明不包括 CHARACTER SET 或 COLLATE 属性。 若要重写此命令，请显式提供 CHARACTER SET 和 COLLATE

  

##### 表的字符集和排序规则

每个表都有一个表字符集和一个表排序规则。 Create TABLE 和 ALTER TABLE 语句具有用于指定表字符集和排序规则的可选子句:

```sql
CREATE TABLE tbl_name (column_list)
    [[DEFAULT] CHARACTER SET charset_name]
    [COLLATE collation_name]]
ALTER TABLE tbl_name
    [[DEFAULT] CHARACTER SET charset_name]
    [COLLATE collation_name]
```

##### 列字符集和排序规则：

每个“字符”列(即 CHAR、 VARCHAR、 TEXT 类型或任何同义词的列)都有一个列字符集和一个列排序规则。 Create TABLE 和 ALTER TABLE 的列定义语法有用于指定列字符集和排序规则的可选子句:

```sql
col_name {CHAR | VARCHAR | TEXT} (col_length)
    [CHARACTER SET charset_name]
    [COLLATE collation_name]
 ////
    CREATE TABLE t1
(
    col1 VARCHAR(5)
      CHARACTER SET latin1
      COLLATE latin1_german1_ci
);
ALTER TABLE t1 MODIFY
    col1 VARCHAR(5)
      CHARACTER SET latin1
      COLLATE latin1_swedish_ci;
```

Mysql 以下列方式选择列字符集和排序规则:

- 如果同时指定了 CHARACTER SET 字符集名称和 COLLATE 排序规则名称，则使用字符集字符集名称和排序规则名称。

  ```sql
  CREATE TABLE t1
  (
      col1 CHAR(10) CHARACTER SET utf8 COLLATE utf8_unicode_ci
  ) CHARACTER SET latin1 COLLATE latin1_bin;
  ```

  为该列指定了字符集和排序规则，因此将使用它们。 该列具有字符集 utf8和排序规则 utf8 unicode ci。

- 如果在没有 COLLATE 的情况下指定 CHARACTER SET 字符集名称，则使用字符集字符集名称及其默认排序规则。

  ```sql
  CREATE TABLE t1
  (
      col1 CHAR(10) CHARACTER SET utf8
  ) CHARACTER SET latin1 COLLATE latin1_bin;
  ```

  为该列指定了字符集，但没有指定排序规则。 该列具有字符集 utf8和默认的 utf8排序规则，即 utf8 general ci。 若要查看每个字符集的默认排序规则，请使用 SHOW CHARACTER SET 语句或查询 informationschemacharacter sets 表。

- 如果指定的 COLLATE 排序规则名称没有使用 CHARACTER SET，则使用与排序规则名称和排序规则名称关联的字符集。

  ```sql
  CREATE TABLE t1
  (
      col1 CHAR(10) COLLATE utf8_polish_ci
  ) CHARACTER SET latin1 COLLATE latin1_bin;
  ```

  为列指定了排序规则，但没有为字符集指定。 该列具有 collation utf8 polish ci，并且字符集是与 collation 关联的字符集 utf8。

- 否则(未指定 CHARACTER SET 或 COLLATE) ，将使用表字符集和排序规则。

  CREATE TABLE t1
  (
      col1 CHAR(10)
  ) CHARACTER SET latin1 COLLATE latin1_bin;
  
  没有为列指定字符集或排序规则，因此使用表默认值。 该列具有字符集 latin1和排序规则 latin1 bin。

  

##### 与字符集有关的系统变量

###### 服务器或当前数据库的属性

character_set_system

character_set_server 和collaction_server

character_set_database 和collaction_database

###### 客户端与服务器之间通信

character_set_client

character_set_results

charcter_set_conneaction

charater_set_filesystem



##### Unicode支持

mysql 6.0之前 仅支持BMP(初级多语言方案)里面的字符是通过两种字符集方案进行解决的。

ucs2：对应着unicode ucs-2编码方案，使用两个字节表示一个字符，高位字节排列在前。这种字符集无法收录两个以上字节才能表示的字符。UCS是通用字符集的缩写

utf8:使用1-3个字节表示一个字符

mysql 6.0之后 支持BMP(初级多语言方案)里面的字符补充字符集收录了。

ucs2：对应着unicode ucs-2编码方案，使用两个字节表示一个字符，高位字节排列在前。这种字符集无法收录两个以上字节才能表示的字符。UCS是通用字符集的缩写新增utf6和utf32字符集类似于utf8,扩充了对补充字符集的支持，在utf16字符集里，Bmp字符任然暂用2个字节，补充字符暂用4个字节。在utf32所有字节暂用4个字节。

utf8:使用1-4个字节表示一个字符，以前的utf8可以用utf8mb3字符集显示。

<font color="red">mysql创建的数据表格式文件扩展名是.frm 每个数据表中只有一个相应的.frm文件。</font>

###  Mysql 各存储引擎

通过 show engenies命令进行查看支持的存储引擎。同样infomation_schema也提供了查询存储引擎的表select ENGINE from information_schema.`ENGINES`

| InnoDB             | DEFAULT | Supports transactions, row-level locking, and  foreign keys  | YES  | YES  | YES  |
| ------------------ | ------- | ------------------------------------------------------------ | ---- | ---- | ---- |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                        | NO   | NO   | NO   |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables    | NO   | NO   | NO   |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO   | NO   | NO   |
| MyISAM             | YES     | MyISAM storage engine                                        | NO   | NO   | NO   |
| CSV                | YES     | CSV storage engine                                           | NO   | NO   | NO   |
| ARCHIVE            | YES     | Archive storage engine                                       | NO   | NO   | NO   |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                           | NO   | NO   | NO   |
| FEDERATED          | NO      | Federated MySQL storage engine                               |      |      |      |

#### MyISAM

默认的存储引擎：提供了键压缩功能。他使用某种压缩算法来保持连续的，相似的字符串索引值。myISAM存储引擎为AUTO_INCREATEMENT 数据列提供了更多的功能。每个MyISAM都有一个标识。支持全文检索，但需要通过FullTEXT索引

不支持事务、也不支持外键，优势是访问速度快，对事务完整性没有 要求或者以select，insert为主的应用基本上可以用这个引擎来创建表

支持3种不同的存储格式，分别是：静态表；动态表；压缩表

静态表：表中的字段都是非变长字段，这样每个记录都是固定长度的，优点存储非常迅速，容易缓存，出现故障容易恢复；缺点是占用的空间通常比动态表多（因为存储时会按照列的宽度定义补足空格）ps：在取数据的时候，默认会把字段后面的空格去掉，如果不注意会把数据本身带的空格也会忽略。

动态表：记录不是固定长度的，这样存储的优点是占用的空间相对较少；缺点：频繁的更新、删除数据容易产生碎片，需要定期执行OPTIMIZE TABLE或者myisamchk-r命令来改善性能

压缩表：因为每个记录是被单独压缩的，所以只有非常小的访问开支

#### Merge存储引擎

提供了把多个MyISAM数据表合并为一个逻辑单元的手段。查询一个merge表相当于查询其所有的成员数据表。是一组MyISAM表的组合，这些MyISAM表必须结构完全相同，merge表本身并没有数据，对merge类型的表可以进行查询，更新，删除操作，这些操作实际上是对内部的MyISAM表进行的。

#### MEMORY存储引擎

Memory存储引擎使用存在于内存中的内容来创建表。每个memory表只实际对应一个磁盘文件，格式是.frm。memory类型的表访问非常的快，因为它的数据是放在内存中的，并且默认使用HASH索引，但是一旦服务关闭，表中的数据就会丢失掉。
MEMORY存储引擎的表可以选择使用BTREE索引或者HASH索引，两种不同类型的索引有其不同的使用范围

Hash索引优点：
Hash 索引结构的特殊性，其检索效率非常高，索引的检索可以一次定位，不像B-Tree 索引需要从根节点到枝节点，最后才能访问到页节点这样多次的IO访问，所以 Hash 索引的查询效率要远高于 B-Tree 索引。
Hash索引缺点： 那么不精确查找呢，也很明显，因为hash算法是基于等值计算的，所以对于“like”等范围查找hash索引无效，不支持；

Memory类型的存储引擎主要用于哪些内容变化不频繁的代码表，或者作为统计操作的中间结果表，便于高效地对中间结果进行分析并得到最终的统计结果，。对存储引擎为memory的表进行更新操作要谨慎，因为数据并没有实际写入到磁盘中，所以一定要对下次重新启动服务后如何获得这些修改后的数据有所考虑。

#### InnoDB存储引擎

支持提交回滚操作，确保数据在事务处理过程中万无一失。可以创建保存点的办法实现回滚。

在系统崩溃后自动恢复。

外键和引用完整性支持，包括递归删除和更新。

数据行级别锁定和多版本共存，使得InnoDB数据表在需要同时检索和更新时表现出非常好的并发性能。

在默认情况下，InnoDB会把数据表集中存储在一个共享的表空间。而不是像大多数其他引擎为不同的数据表创建不同的文件。InnoDB表空间可以由多个文件构成，还可以包含原始分区。Innodb表空间就像一个虚拟的文件系统，他存储和管理所有InnoDB数据表内容。

该存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是对比MyISAM引擎，写的处理效率会差一些，并且会占用更多的磁盘空间以保留数据和索引。
InnoDB存储引擎的特点：支持自动增长列，支持外键约束。

#### 选择合适的存储引擎

<font color="red">除非需要用到某些InnoDB不具备的特性，并且没有其他办法可以替代，否则都应该优先选择InnoDB引擎。</font>如果要用到全文索引建议考虑InnoDB加上Sphinx的组合，而不是使用支持全文索引的MyISAM.除非万不得已，否则建议不要使用多种存储引擎，否者会带来一些列复杂问题、以及一些潜在的bug和边界问题。

如果应用需要不同的存储引擎需考虑以下几个因素：

- 事务：

  如果应用需要事务支持，那么InnoDB是目前最稳定并且经过验证的选择，如果不需要事务，并且主要是insert和select操作，那么MyIsam是个不错的选择。

- 备份：

  备份的需求就会影响存储引擎的选择。如果定期的关闭服务器执行备份，那备份因素可以忽略。反之，需要热备份，那么选择InnoDB就基本的需求。

- 奔溃恢复：

  数据量比较大的时候，系统崩溃后MyISAM崩溃后发生损毁的概率比InnoDB要高的多。

- 特有的特性

  mysql也只有MyISAM支持地理空间搜索。如果一个存储引擎拥有一些关键的特性，同时却缺乏一些必要的特性，那么有时候不得不做折中的考虑，或者在架构设计上做取舍。

alter table mytable engine= InnoDB;

### Mysql数据类型

##### 数值

| 数据类型  | 字节数 | 含义          | 数据范围                                                     |
| --------- | ------ | ------------- | ------------------------------------------------------------ |
| tinyint   | 1      | 非常小的整数  | 有符号的范围是-128到127，无符号的范围是0到255                |
| smallint  | 2      | 小整数        | 有符号的范围是-32768到32767，无符号的范围是0到65535          |
| mediumint | 3      | 中等大小的int | 有符号的范围是-8388608到8388607，无符号是0到16777215         |
| int       | 4      | 标准的整数    | 有符号的范围是-2147483648到2147483647，无符号是0到4294967295 |
| bigint    | 8      | 大整数        | 有符号的范围是-9223372036854775808到9223372036854775807，无符号的范围是0到18446744073709551615 |
| decimal   |        | 定点数        | 一个未压缩(unpack)的浮点数字。DECIMAL(M,d)M字节.不能无符号。行为如同一个CHAR列：“未压缩”意味着数字作为一个字符串被存储，值的每一位使用一个字符,每9位数字需要4个字节。剩下的数字需要1到4个字节 |
| float     | 4      | 单精度浮点数  | 不能无符号。允许的值是-3.402823466E+38到-1.175494351E-38，0 和 1.175494351E-38到3.402823466E+38。M是显示宽度而D是小数的位数。没有参数的FLOAT或有<24 的一个 参数表示一个单精密浮点数字 |
| double    | 8      | 双精度浮点数  | 允许的值是-1.7976931348623157E+308到-2.2250738585072014E-308、 0和2.2250738585072014E-308到1.7976931348623157E+308 |
| bit       | M      | 位字段        | 可变取决于M，一个bit值大约为（M+7)/8个字节的存储空间         |

##### 字符串值

`mysql 的二进制数据类型 BINARY, VARBINARY, BLOB 都没有字符集的概念。`

| 数据类型   | 含义                                                         |
| ---------- | ------------------------------------------------------------ |
| char       | 固定长度的非二进制字符串M的范围是 1——255 个字符              |
| varchar    | 可变长度的非二进制字符串，M的范围是 1——65532 个字符          |
| binary     | 固定长度的二进制字符串                                       |
| varbinary  | 可变长度的二进制字符串                                       |
| tinyblob   | 非常小的blob                                                 |
| blob       | blob 最大长度为65535(2^16-1)个字符，二进制流                 |
| mediumblob | 中等大小的blob                                               |
| longblob   | 大blob，一个BLOB或TEXT列，最大长度为4294967295(2^32-1)个字符 |
| tinytext   | 非常小的非二进制字符串                                       |
| text       | 小文本，最大长度为65535(2^16-1)个字符。  不能存储图片，非二进制。 |
| mediumtext | 中等大小的非二进制字符串，一个BLOB或TEXT列，最大长度为16777215(2^24-1)个字符 |
| longtext   | 大的非二进制字符串，一个BLOB或TEXT列，最大长度为4294967295(2^32-1)个字符 |
| enum       | 枚举集合，这个值式选自与值列表'value1'、'value2', ...,或NULL。一个ENUM最多能有65535不同的值 |
| set        | 集合，能有零个或多个值的一个字符串对象，其中每一个必须从值列表'value1', 'value2', ...选出。一个SET最多能有64个成员 |

##### 日期时间值

日期时间数据类型。

| 数据类型  | 含义                              | 取值范围                                     | 字节 |
| --------- | --------------------------------- | -------------------------------------------- | ---- |
| date      | 日期值，格式YYYY-MM-DD            | ’1000-01-01‘到’9999-12-31‘                   | 3    |
| time      | 时间值，格式’hh:mm:ss‘            | ’-838:59:59‘到’838:59:59‘                    | 3    |
| datetime  | 日期加时间值。YYYY-MM-DD hh:mm:ss | ’1000-01-01 00:00:00‘到’9999-12-31 23:59:59‘ | 8    |
| timestamp | utc时间戳值，YYYY-MM-DD hh:mm:ss  | ’1970-01-01 00:00:01‘到’2038-01-19 03:14:07‘ | 4    |
| year      | 年份 ccYY或YY                     | year(4):1901到2155 year(2):1970到2069        | 1    |

##### 坐标值

set @pt=pointfromatext('point(10,20)');

空间数据类型

| 数据类型           | 含义                    |      |
| ------------------ | ----------------------- | ---- |
| geometry           | 任何类型的坐标值        |      |
| point              | 点（一对 x，y坐标值）   |      |
| linestring         | 曲线（一个或多个point） |      |
| polygon            | 多边形                  |      |
| geometrycollection | geometry值的集合        |      |
| multllinesting     | linestring值的集合      |      |
| multipoint         | point值的集合           |      |
| multipolygon       | polygon的值集合         |      |

##### 布尔值

在boolean值中，零表示“假”，任何非零，非null的值都为“真”    1==true,0==false

##### 空值NuLL

NULL是一个“没有类型”的值，他通常用来表示“没有数据”，“数据未知”，“数据缺失”，“数据不支持”

#### 如何进行挑选数值数据类型

在选择数据类型时需要考虑并挑选一个能够覆盖该范围的最小类型使用，选择较大的类型会浪费存储空间，毫无必要的是数据表变大，会降低数据的处理效率。所以要选择合适的数据类型对数据表数据。

***应该把值表示为字符数据还是二进制数据？***

如果字符数据选择非二进制字符串类型最合适；如果是二进制数据就选择一种二进制字符串类型。

***比较操作需要区分字母的大小写情况吗？***

如果是，则应该选用一种非二进制字符串类型。让存储在数据库里的字符与一种字符集和排序方式关联起来。

非二进制字符串比较和排序操作中的大小写区分情况取决于指定的排序方式。

***你想尽量少占用存储空间吗？***

如果是，选用一种可变长度的类型。

***数据列的可取值总是某几个合法值之一或他们的组合吗？***

如果是，enum或set类型往往是最好的选择，如果字符串是一个有限的集合并且你想按照某种非字母表顺序对他们进行排序，enum类型可以使用

***尾缀的空格（或零值字节）很重要吗？***

如果数据必须原样存入数据库，原样取出数据库在存储和检索过程中不增加和消除任何尾缀的空格应选用text或varchar数据列。为二进制字符串选用一个blob或varbinary数据列。

| 数据类型          | 存储时     | 检索时     | 结果                 |
| ----------------- | ---------- | ---------- | -------------------- |
| char              | 补足       | 去掉尾缀   | 检索出来的值没有尾缀 |
| binary            | 补足       | 无任何动作 | 检索出来的值没有尾缀 |
| varchar,varbinary | 无任何动作 | 无任何动作 | 尾缀没有任何变化     |
| text,blob         | 无任何动作 | 无任何动作 | 尾缀没有任何变化     |



##### char和varchar数据类型

char和varchar字符串类型用来保存非二进制字符串，因而与这一种字符集和排序方式相关联。

- char是一种固定长度的类型，而varchar是一种长度可变类型
- 从char数据类型检索出来的值的尾缀空格将被去掉。给定一个char(M)的数据列，如果某个值的长度小于M个字符，Mysql在吧存入该数据列时将空格把他补足到M个字符长度，但是在检索时追加的空格在检索时将被去掉。
- 对于vatchar(M)数据列，尾缀空格在存储和检索时都会被保留。
- char类型中的M是可选的，如果省略 他的默认是1，char(0)是合法的，如果你允许他为null值或空字符串。在数据表里 char(0) 数据列置占用非常少的空间----只占用一位。

对varchar(M)数据列而言，M在语义上的取值范围是1到65535，但他实践能够容纳的最大字符个数肯定小于65535,个字节

- 一个长varchar数据列需要2个字节存放字符串值的长度，这两个字节计算在数据行总长度之内。
- 如果使用多字节字符，数据行最大长度所容纳的字符个数将减少。
- 数据表里往往还有其他的数据列，而那些数据列将挤占varchar数据列的“生存”空间，

##### binary和varbinary数据类型

char和varchar是非二进制类型。Mysql将根据相关的字符集和排序方式把char和varchar数据列的值解释为一系列字符。比较操作的依据是各字符的先后顺序。

binary和varbinary是二进制类型，binary和varbinary数据列里的值是一串字节，不涉及任何字符集和排序方式。比较的是数据各自节的数值大小。

##### blob和text数据类型

blob类型是由tinyblob,blob,mediumblob,longblob等组成的大家庭。blob数据列存储的是二进制字符串，如果想保存的信息有可能集聚膨胀到非常大的地步，或者个数据行的长短差异很大。存放：压缩数据，加密数据，图像，声音。当blob和text值太大时，innodb会使用专门的“外部”存储区域来进行存储，此时每个值在行内需要1-4个自己存储一个指针，然后在外部存储实际的值。

text类型是由tinytext，text,mediumtext和longtext等类型组成的text家族。text类型存储的是非二进制字符串，

<font color="red">blob和text数据列的值在长度方面往往差异巨大，blob和text数据行的删除和修改容易产生大量碎片</font>

<font color="red">max_sort_length系统变量的设置情况会影响到blob和text值的比较和排序操作，每个blob或text值只有前max_sort_length个字节参加比较或排序，max_sort_length默认值1024不够大而导致的问题就应该加大这个值，如果不行加大max_allowed_packet参数的值</font>

##### enum和set数据类型

enum和set是比较特殊的字符串数据类型，他们的取值只能预先定义好的字符串。enum数据列里必须包含且只包含一个来自他值列表的成员，而set数据列里只允许包含任意多个来自成员的值。enum类型的值不允许同时出现，而set类型的值允许同时出现。

employees enum('less than 100','100-400','501-1550')

set值被存储为位值，每个字节对应8个set成员。set数据列占用的存储空间取决于他的成员个数，最多为64个。

### Mysql使用函数：

#### 文本处理函数

RTrim()函数来去除列值右边的空格。

Upper()函数将文本转换为大写。select vend_name, Upper(vend_name)  as vend_name_upcase from vendors;

| 函数        | 说明              |
| ----------- | ----------------- |
| Left()      | 返回串左边的字符  |
| Length()    | 返回串的长度      |
| Locate()    | 找出一个子串      |
| Lower()     | 将串转换为小写    |
| LTrim()     | 去除左边的空格    |
| Reight()    | 返回串右边的字符  |
| RTrim()     | 去掉串右边的空格  |
| Soundex()   | 返回串的SOUNDEX值 |
| SubString() | 返回子串的字符    |
| Upper()     | 将串转换为大写    |

匹配所有的发音类似于Y.Lie的联系名：

select cust_name,cust_contact from customers where Soundex(cust_contact) = Soundex('Y Lie');

#### 日期处理函数

| 函数                               | 说明                         |
| ---------------------------------- | ---------------------------- |
| AddDate()                          | 增加一个日期（天，周）       |
| AddTime()                          | 增加一个时间                 |
| CurDate()                          | 返回当前的日期               |
| CurTime()                          | 返回当前时间                 |
| Date()                             | 返回日期时间的日期部分       |
| DateDiff(date1,date2)              | 计算两个日期差               |
| Date_Add(date, INTERVAL expr type) | 高度灵活的计算函数           |
| Date_Format(date,format)           | 返回一个格式化的日期或时间串 |
| Day(date)                          | 返回一个日期的天数部分       |
| DayOfWeek(date)                    | 对于一个日期返回周几         |
| Hour(date)                         | 返回一个时间的小时部分       |
| Minute(date)                       | 返回一个时间的分钟部分       |
| Month('2020-03-01')                | 返回一个日期的月份           |
| Now()                              | 返回当前日期和时间           |
| Second(date)                       | 返回一个时间的秒部分         |
| Time(date)                         | 返回一个日期时间部分         |
| Year(date)                         | 返回一个日期的年份           |

#### 数值处理函数

| 函数   | 说明               |
| ------ | ------------------ |
| Abs()  | 返回一个数的绝对值 |
| Cos()  | 返回一个角度的余弦 |
| Exp()  | 返回一个数的指数值 |
| Mod()  | 返回操作的余数     |
| Pi()   | 返回圆周率         |
| Rand() | 返回一个随机数     |
| Sin()  | 返回一个角度的正弦 |
| Sqrt() | 返回一个数的平方根 |
| Tan()  | 返回一个角度的正切 |

IFNULL()

### Mysql数据表操作

#### 创建一个原本没有的数据表：

 create table if not exits语句，无需假设它需要用到的数据表是否存在。当存在一个相同的表名时用create table语句创建时就会出错。所以使用 If not exits语句就不会有这些问题了。

#### 临时数据表

如果在数据表创建语句加入temporary关键字，服务器将创建一个临时的数据表，当与服务器连接断开时会自动消失。

create temporary table tab1_name...;

这样做服务器会在客户回话结束时自动删除一个Temporary数据表，但也可以用完后显式的进行删除、一个temporary的数据表允许与一个永久性表名相同，只是在临时表期间不能够访问原来的永久表数据（原来的表被隐藏了，只能访问临时表）。无法创建两个同名的临时表。如果使用连接池则与mysql的连接状态会保持连接，这意味着创建的temporary数据表不一定会在应用程序结束时自动消失、

#### 从其他数据表或查询结果创建数据表

##### Create table ... like 语句

创建一个新数据表作为原始数据表的一份空白副本。将原来的数据表结构复制过来。包括数据列的所有属性，索引内容。不能将原始数据表以外的数据列复制。

这时新的数据表是个空白的数据表需要用insert into selecte 语句进行填充数据，

create table new_table_name like tab1_name;

insert into new_table_name select * from tab1_name;

过滤一部分数据到数据表中

create table new_table_name like tab1_name;

insert into new_table_name select * from tab1_name  where xxxx0 ;

##### create table ... select语句

可以从任意一条select语句的查询结果创建新的数据表。可以同时完成创建和填充新的数据，。

#### 复制表数据

##### 复制表数据时创建表

SELECT vale1, value2 into Table2 from Table1

SELECT INTO 语句从一个表复制数据，然后把数据插入到另一个新表中。

  要求目标表Table2不存在，因为在插入时会自动创建表Table2，并将Table1中指定字段数据复制到Table2中。示例如下：

select * into notes_note_migrate1 from notes_note where createatutc>='2019-7-1';

##### 复制表数据不创建表(新的表结构必须存在)

Insert into Table2(field1,field2,...) select value1,value2,... from Table1

   要求目标表Table2必须存在，由于目标表Table2已经存在，所以我们除了插入源表Table1的字段外，还可以插入常量

##### 提示

*MySQL 数据库不支持 SELECT ... INTO 语句，但支持* [INSERT INTO ... SELECT](https://www.runoob.com/sql/sql-insert-into-select.html) *。*

当然你可以使用以下语句来拷贝表结构及数据：

```sql
CREATE TABLE 新表
AS
SELECT * FROM 旧表 
```

#### 删除数据表

Drop table tabe1_name;// 直接删除

#### 数据表创建索引

索引，加快对数据表内容的访问速度的基本手段，尤其是涉及数据表的关联查询。

索引主要有以下几个作用

1. 即上述所说，索引能极大地减少扫描行数
2. 索引可以帮助服务器避免排序和临时表
3. 索引可以将随机 IO 变成顺序 IO

##### 索引分类

唯一索引：不允许索引项出现重复的值，对涉及一个数据列的索引不会包涵重复的值，对于多个数据列的索引（复合索引）就是这几个数据列的值的组合在数据表的范围内不能重复出现重复。
普通（非唯一）索引：允许索引值重复。

FullText索引：用来进行全文检索，只适用于MyISAM数据表

SPATIAL索引：只适用于MyISAM数据表

hash索引：memory数据表的默认索引类型。也可以改用为Btree索引代替这个默认索引。

##### 索引类型：

普通索引：仅加速查询

唯一索引：加速查询 + 列值唯一（可以有null）

主键索引：加速查询 + 列值唯一（不可以有null）+ 表中只有一个  每个数据表中只能有一个 PRIMARY key, 并且不可以为NULL

组合索引：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并

全文索引：对文本的内容进行分词，进行搜索

ps.索引合并，使用多个单列索引组合搜索

*覆盖索引，select的数据列只用从索引中就能够取得，不必读取数据行，换句话说查询列要被所建的索引覆盖*

<font color='red'>可以使用alert table或create index语给数据表加索引。mysql内部会把create index语句映射为alter table 操作。</font>

alert table tab1_name add  INDEX index_name

alert table tab1_name add UNIQUE index_name

alert table tab1_name add PRIMARY KEY (index_name)

alert table tab1_name add  FullTEXT index_name

<font color="blue">对于某个字符数据列的一个前缀编索引是col_name(n),n是代表的是索引包含列值的前n个字节（二进制字符串类型）或前n个字符（非二进制字符串类型）比如：alert table tab1_name add index_name(10),blob类型或text数据类型只能创建前缀索引。索引本身的长度等于构成索引的各个数据列的索引部分的长度总和。如果长度超出则可以使用缩短编排的前缀索引。</font>

##### 删除索引：

删除索引可以用Drop index或alert table语句来完成。

Drop index index_name on tb1_name;

alert table tab1_name drop index index_name;

同样可以使用，show create table或者show index 查询出索引。

##### 索引的优点

索引大大减少了服务器需要扫描的数据量

索引可以帮助服务器避免排序和临时表

索引将随机I/O变为顺序I/O

索引并不是最好的工具，总的来说，只有当索引帮助存储引擎快速查找到记录带来的好处大于其带来的额外工作时，索引才是有效。对于中大行的表，索引就非常有效。但对于特大型的表，建立和使用索引的代价随之增长。



#### 改变数据表的结构

##### alter table语句

alter table tab1_name action [,action] ... ;

每一个action代表一个要修改的数据表字段

alter table mytab1 Modify i mediumint unsigned;

alter table mytab1 change i  i mediumint unsigned;

change子句在改变时还可以重新命名。

alter table mytab1 change i k mediumint unsigned;

<font color="\#0000FF">改变数据列时，同时指定字符集: alter table t modify c  char(20) character set</font>

<font color="red">使用alter table重新命名一个数据表。alter table  tab_name rename new_tab_name;</font>

<font color="#33FF33">alter table语句每次只能重新命名一个数据表，而rename语句可以一次重新命名多个数据表 rename table t1 to tmp,t2 to t1, tmp to t2 </font>

#### 获取数据库元数据

##### 各种的show语句

show databases;列出所有的数据库。

show create database db_name;查看给定数据库的 create databases 语句

show tables;show tables from db_name 列出数据表

show create table tb1_name; 查看数据表的 create table 语句

show columns  from tab1_name;查看数据表了里面的列

show index from tb1_name; 查看数据列或索引信息

describle tb1_name和 explain tb1_name 语句是show columns from tb1_name语句的同义词。

show table status;show table status from db_name 查看默认数据库或给定数据库表的描述信息。

##### infomation_schema数据库里的数据

infomation_schema数据库是以sql语言标准为基础。虽然一部分是mysql特有的，优于show语句的可移植性。

infomation_schame数据库可想像为是一个虚拟的数据库是一些由不同的数据库元数据构成的视图。

show tables in infomation_schema;

输出以下结果：

CHARACTER_SETS  ：所支持的字符集
COLLATIONS ：每种字符集的排序方式
COLLATION_CHARACTER_SET_APPLICABILITY ：每种排序方式与他的字符集的映射关系信息
COLUMNS  ：数据列
COLUMN_PRIVILEGES
ENGINES ：存储引擎
EVENTS ：数据库事件
FILES    ： 关于NDB硬盘数据文件的信息。
GLOBAL_STATUS 
GLOBAL_VARIABLES ：全局变量
KEY_COLUMN_USAGE ：数据列的约束条件信息
OPTIMIZER_TRACE
PARAMETERS
PARTITIONS ：数据库分区
PLUGINS ：服务插件信息
PROCESSLIST ：服务器内执行的线程信息。
PROFILING
REFERENTIAL_CONSTRAINTS  ：外键的信息
ROUTINES ：存储例程（store routine）
SCHEMATA   ：数据库
SCHEMA_PRIVILEGES
SESSION_STATUS
SESSION_VARIABLES
STATISTICS ：关于数据表索引特性的信息
TABLES ：数据表
TABLESPACES
TABLE_CONSTRAINTS ： 数据表的约束条件
TABLE_PRIVILEGES
TRIGGERS  :触发器
USER_PRIVILEGES ：：全局的权限
VIEWS  ：视图
INNODB_LOCKS
INNODB_TRX
INNODB_SYS_DATAFILES
INNODB_FT_CONFIG
INNODB_SYS_VIRTUAL
INNODB_CMP
RDS_EVENTS_THREADS_WAITS_CURRENT
INNODB_CMP_RESET
INNODB_CMP_PER_INDEX
INNODB_CMPMEM_RESET
INNODB_FT_DELETED
INNODB_BUFFER_PAGE_LRU
INNODB_LOCK_WAITS
INNODB_TEMP_TABLE_INFO
INNODB_SYS_INDEXES
INNODB_SYS_TABLES
INNODB_SYS_FIELDS
INNODB_CMP_PER_INDEX_RESET
INNODB_BUFFER_PAGE
INNODB_FT_DEFAULT_STOPWORD
INNODB_FT_INDEX_TABLE
INNODB_FT_INDEX_CACHE
INNODB_SYS_TABLESPACES
INNODB_METRICS
INNODB_SYS_FOREIGN_COLS
INNODB_CMPMEM
INNODB_BUFFER_POOL_STATS
INNODB_SYS_COLUMNS
INNODB_SYS_FOREIGN
INNODB_SYS_TABLESTATS
RDS_PROCESSLIST
INNODB_FT_BEING_DELETED



### Mysql基本信息检索

#### select 语句的语法实例：

select select_list 

from table_list

where row_constrainst

group by grouping_columns

order by sorting_columns

having group_constaint

limint count;

#### 指定检索条件-where子句

select * from score where score>95

字符串的比较操作通常是不区分大小写的。

select last name,firstname from president where lastname='ABC';

select last name,firstname from president where lastname='abc';结果一致

#### null值

null是个特殊的值。他的含义是“无数据，”或者“未知数据”，所以能用它与“有数据”的值进行比较。

select null<0,null=0,null<>0,null>0;

select lastname from president where death is null;

#### Order By子句

order by 子句默认排序是升序序列。

select lastname from president order by lastname desc;

对于null值的数据行，如果采用的升序desc排列他们讲出现在结果的开头。如果是降序排列则他们将出现在结果的末尾。

select lastname,firstname.death from president order by If(death is null,0,1),death desc;

IF函数的作用是对其紧随其后的表达式进行求值，结果为真则是第一个值，为假则是第二个值。

select lastname,firstname.death from president order by Rand() limit 3;随机出现3条

#### 对输出列进行求值和命名

mysql允许将表达式的计算结果当做输出列的值，而不引用数据表。表达式可以简单也可以复杂。

select 17,Format(sqrt(25+13),3)生成平方根并将结果按3位小数表示

select concat(fisrtname,' ',lastname) as 'president name' from president;连接属性值并取别名。

#### 日期有关的问题

查找日期

select * from grade_event where date='2018-09-01'

select lastname,firstname.death from president where death >= '1970-01-01' and death < '1980-01-01';

将日期中的年月日分离出来 用函数 YEAR(),MONTH(),DAYOFMONTH()。

select last_name,first_name,birth from president where month(birth)=3;

select last_name,first_name,birth from president where month(birth)=3 and DayOfMonth(birth)=29;查询出birth为3-29的记录

查询与“今天”有关的日期

select last_name,first_name,birth from president where month(birth)=month(curdate()) and DayOfMonth(birth)=DayOfMonth(curdate());

Timestampdiff()函数：用于指定计算结果的单位 计算两个时间间隔。

select lastname,firstname,birth,death, timestampdiff(year,birth,death) as age from president where death is not null order by age desc limit5 这样既有可以计算出年龄。death-birth;

TO_DAYS函数将日期转换为天数。可计算出据某个特定日期还有多少时间。比如查询60天以内的。

select lastname,firstname,expriation from member where (TO_DAYS(expriation)-TO_DAYS(CURDATE()))<60;

使用TImeStampdiff()函数

select lastname,firstname,expriation from member where timestampdiff(DAY,CURDATE(),expiration)<60;

日期使用DATE_ADD() 或DATE_SUB()函数来完成。这两个函数的输入参数是一个日期值和一个时间间隔值，返回结果则是一个新日期。

Select DATA_ADD('1970-1-1',INTERVAL 10 Year)  --输出为-->1980-01-01

Select DATA_sub('1970-1-1',INTERVAL 10 Year)  --输出为-->1960-01-01

#### 模式匹配 like

like和not like 提供了包含通配符的字符串，“_” 只能匹配一个字符，百分号“%”则能匹配任何一个字符序列包括空序列在内

#### 如何设置和使用sql变量

mysql允许自定义变量。可以使用查询结果设置变量。变量命名语法：

“@变量名” 赋值语法是在select语句中使用一个”@变量名:=值“的形式的表达式。

比如:select @birth :=birth from president where last_name='jack' and first_name='asd';---->

| @birth:=birth |      |
| ------------- | ---- |
| 1765-03-15    |      |

再次使用变量

select last_name,first_name,birth from president where birth<@birth order by birth;

| last_name | first_name | birth      |
| --------- | ---------- | ---------- |
|           |            | 1732-02-22 |

sql语句中也可以用来对变量进行赋值。“=”和“:=”都可以进行对其赋值操作符

set @today=curdate();

set @oneweek_ago :=Date_sub(@today,INTERVAL 7 day);

select @today,oneweek_ago;

通过上面可以查出变量的值

#### 如何生成统计信息

###### Distinct

可以将查出来的结果重复出现的数据行清除掉。

select distinct  state from president order by state;

###### Count,Group by

count是将所有的数据行进统计出来。

select count(*) from member;

count(*)统计的结果是被选中的行数据的总数。count(列名) 统计全体非Null 值的个数。

select count(*),count(eamil),count(expiration) from meber;

| count(*) | count(eamil | count(expiration) |
| -------- | ----------- | ----------------- |
| 102      | 80          | 96                |

count和distinct联合起来统计查询结果里有多少种不同的非null值。

select count(distinct state) from president;

**通过count和Group by 子句可以用一个查询查询到某数据列里不同值分别出现的个数**

select sex,count(*) from student Group by sex;

| sex  | count(*) |
| ---- | -------- |
| F    | 14       |
| M    | 16       |

能够统计出这样的结果是Group BY子句必不可少，他可以让数据进行分类。如果不加会出现错误。

使用Group by 与count(*)结合优点如下：

- 在开始统计前，我们不必知道被统计的数据有多少不同的取值。
- 我们只需使用一个而不是好几个查询
- 因为只用一个查询就能把所有的结果查找出来，而且可以进行排序、

select state,count(*)  as count from president Group by state order by count desc;

###### Having子句与Where

having和where都是用来设定查询条件的。输出的行必须符合这些查询条件。

count()之类的汇总函数的计算结果允许在having子句中出现。

select state,count(*) as count from president Group by state having count>1 order by count desc;

带有having子句的查询特别适合用来查找某个数据列重复出现的值

###### 其他

min() 最小值

max() 最大值

sum() 总和

avg() 平均

```sql
select event_id min(score) as minimum,max(score) as maximum, max(score)-min(score)+1 as span,sum(score) as total, AVG(score) as average,Count(score) as count from score Group BY event_id;
```

###### 超级聚合值：with RollUP子句

select sex,count(*) from student Group by sex width rollup;

with rollup还可以结合其他聚合函数使用：

```sql
// 统计结果可出现一行对每一列的统计信息
select event_id min(score) as minimum,max(score) as maximum, max(score)-min(score)+1 as span,sum(score) as total, AVG(score) as average,Count(score) as count from score Group BY event_id width rollup;
// 如果group by 子句指定了多列，width rollup 会生成另外的包含高级统计值的超级聚合行。
```

### Mysql 多表高级检索信息

#### 联结join:

是将一个数据表中的信息与另一个数据表结合起来才能得到查询结果。

<font color="red">根据某个数据表中的每一行数据与另一个数据表里的每一行组合来连接操作（笛卡尔积），</font>

select t1.*,t2. *  from t1,t2 where t1.i1=t2.i2;

select t1.*, t2. * from t1 join t1.i1=t2.i2;

","（逗号）关联的操作符的效果与 inner join 相似。逗号操作符的优先级和其他联结类型不一致，有时会导致语法错误。避免使用逗号操作符。

```sql
select student_id,date,score,category 
from grade_event inner join score
on grade_event.event_id=score.event_id
where date='2019-09-23'
```

这个inner join 查询是先根据指定日期查询出grade_event行，再利用行里的event_id把score数据表里拥有同一event_id的考试分数查询出来。把学生的学号，分数，日期，考试事件的类型。from 子句从哪些数据表中匹配信息。on子句给出匹配的条件。

```sql
select student_id,date,score,category 
from grade_event inner join score inner jon student
on grade_event.event_id=score.event_id
and score.student_id=student.student_id
where date='2019-09-23'

```

这个inner join 查询是先根据指定日期查询出grade_event行，再利用行里的event_id把score数据表里拥有同一event_id的考试分数查询出来。把学生的学号，分数，日期，考试事件的类型。再通过student数据表将分数与学生进行关联起来。from 子句从哪些数据表中匹配信息。on子句给出匹配的条件。

#### left join

left join的工作情况是：给出用来匹配两个数据表里的数据行的数据列，当来自左数据列的某个数据行与右侧数据表的某个数据行匹配时，那两个数据行的内容就会被选取一个输出行；如果来自左数据表的某个数据行在右侧不能找到匹配，他也会被选取一个输出数据行，此时与他关联的是一个来自右侧数据表的“假”数据行，这个“假”数据行的所有数据列都包含null值。

left join 是将连接操作的第一张数据表（左表）的数据行生成一个输出行。来自左侧数据表中的每一个数据行在结果集里都有一个对应的数据行，不管他在右侧的数据表里有没有匹配。在结果集里，在右侧数据表里没有匹配的结果数据行有这样一个特征：来自右数据表的所有数据列都是Null值，这个特征可以让知道右数据表里缺少了哪些数据行。

如：得到每一个学生的缺勤次数》

```sql
select student.student_id,student name 
count (absence.date) as absence
from student left join absence
on student.student_id = absence.student_id
group by student.student_id
```

例：

数据表t1:

| i1   | c1   |
| ---- | ---- |
| 1    | a    |
| 2    | b    |
| 3    | c    |

数据表t2:

| i2   | c2   |
| ---- | ---- |
| 2    | c    |
| 3    | b    |
| 4    | a    |

select t1.*,t2. * from t1 inner join t2 on t1.i1 = t2.i2;

| i1   | c1   | i2   | c2   |
| ---- | ---- | ---- | ---- |
| 2    | b    | 2    | c    |
| 3    | c    | 3    | b    |

左连接操作会为t1数据表里每一行生成一个输出数据行，不管他在t2里有没有匹配。

select t1.* t2. * from t1 left join t2 on t1.i1=t2.i2;

| i1   | c1   | i2   | c2   |
| ---- | ---- | ---- | ---- |
| 1    | a    | NULL | NULL |
| 2    | b    | 2    | c    |
| 3    | c    | 3    | b    |

<font color="red">在使用左连接时需要注意一个问题：如果右数据表里的数据列没有被全部定义成Not NUll，结果集里的数据行就很可能不能反映真实情况，比如右数据表里某个数据列允许取值为NULL并被收录在结果集里，用这个数据列的null值来判断“在右数据表里没有匹配”就可能出问题。</font>

select t1.* ,t2.*  from t1 left join t2 on t1.i1=t2.i2;

select t1.* ,t2.* from t2 right join t1 on t1.i1=t2.i2;

<font color="red">1.on条件在生成临时表时使用的条件，他不管on中的条件是否为真，都会返回左边表的记录2.where 条件是在临时表生成后，再对临时表进行过滤。这时已经没有left join 的含义，条件不为真的就全部过滤掉3.left join 适合解决“哪些值是缺失的”这个问题</font>

<font color="green">在left join时在结果集里在右侧数据表里没有匹配的结果数据行时：来自右数据表的所有数据列都是Null值，这个特征可以让知道右数据表里缺少了哪些数据行。如果left join 后过滤了右表的为null的情况下，就是和inner join 结果一致了</font>

#### 用union 语句进行多表检索

如果想把多个查询的结果合并在一起并且创建一个结果集，可以使用union 语句来做这件事。

select i from t1 union select i from t2 union select from t3;

**union语句有以下特性：**

- [ ] `数据列的名字和数据类型`

  unoin 结果集里的数据列名字来自第一个select 语句里的数据列的名字，union中的第二个和再后面的select语句必需选取个数相同的数据列，但各有关数据列不必有同样的名字或数据类型（一般说，同一条union语句里的各有数据列会是相同的数据类型，但不是一项要求，如果他们一样，Mysql会进行必要的类型转换。）数据列是根据位置而不是名字进行匹配的。

  select i, c from t1 union select i,d from t3;

  select i, c from t1 union select d,i from t3;

  | i    | c          | i          | c    |
  | ---- | ---------- | ---------- | ---- |
  | 1    | red        | 1          | red  |
  | 100  | 1904-01-01 | 1904-01-01 | 100  |

- [ ] 重复行的处理。

  <font color="red">在默认情况下，union将从结果集里剔除重复的数据行。union distinct是union同义词，他们都是死只保留不重复的数据行.去除重复行</font>

  <font color="blue">如果你想保留数据的重复行，需要把每个union都改为union all.</font>

  <font color="green">如果把union （或union distinct）和union all 混杂在一起使用，每个union (或union distinct )操作将优先于他左边的 任何 union all 操作</font>

- [ ] order by 和limit 处理

  如果想把将union 结果作为一个整体进行排序，需要用括号把每一个select 语句包括起来并在最后一个 select 语句的后面加上一个order by 子句。<font color="red">注意，因为uoin 的结果集数据列的名字来自第一个select语句，所以你在order by 子句里必须引用那些名字，而不是最后一个select语句的数据列名字。(select i ，c from t1) union (select i,d from t3) order by c; `如果某个数据列有别名，则位于union 语句末尾的order by 子句必须引用这个别名。此外order by 不能引用数据表的名字。(select t.i ，t.c from t1 t) union (select i,d from t3) order by t.c; ` </font>

  若果想限制union的行数则可以使用：

  (select i ，c from t1) union (select i,d from t3) limit 3; 

  

  

#### 子查询

Mysql支持子查询。子查询就是嵌套select语句进行查询》比如从grada_event 数据表里找出对应于考试（'T'）的event_id再用他们去选取考试成绩;

select  * from  score where event_id in(select event_id from grade_event where category='T')

子查询可以返回以下不同类型的信息。

- 标量子查询将返回一个值。
- 数据列子查询将返回一个由一个或多个值构成的数据列
- 数据行子查询将返回一个由一个或多个值构成的数据行
- 数据表子查询将返回一个或多个数据行构成的数据表，数据行可以由一个或多个数据列构成。

子查询的结果可以用以下的不同的方法进行测试。

- 标量子查询的结果可以用诸如“=”或“<”之类的相对比较操作符进行求值。
- 可以用in和not in操作符把某个定值是否包含在子查询的结果集中。
- 可以用all any和some操作符吧某给定值与子查询的结果集进行比较。
- 可以用exists和not exists操作符来测试子查询的结果集是否为null.

##### 子查询与关系比较符

=，<>,>,>=,<,<=操作符用来进行相对值比较。他们可以和标量子查询配合使用：用子查询返回值来构造一个检索条件，再有主查询根据该条件进一步检索。

select * from score where event_id =(select event_id from grade_event  where date='2008-09-23' and  category='Q')

子查询的前面有一个值和一个相似比较操作符，子查询的这种用法要求他只返回一个值。有时候为了确保子查询返回一个值有必要用limit 1对子查询的结果加以限制。

select * from score where event_id = 5 and score >(select AVG(score) from score where event_id = 5);

如果子查询返回的是一个数据行，可以用一个数据行构造器把一组值（一条临时创建的记录）与子查询的结果进行比较。下面是将数据返回一个数据行，他可以告诉我们哪几位总统和john 出生在同一个城市和州。

select last_name,first_name,city,state from president where (city,state) = (select city, stage from president) where last_name ='Admis' and first_name ='john';

##### in 和 not in 子查询

如果子查询返回数据行多个，可以用in 和 not in 操作符来构造主查询的检索条件。in和not in 操作符的用途是测试一个给定的比较值有没有出现一个特定的集合里。只要主查询里的数据行与子查询返回的任何一个数据行匹配，in操作符比较结果就是 true,如果主查询里的行与子查询所返回的所有数据行都不匹配，not in 操作符的比较结果为true,

select * from student where student_id in (select student_id from absence)

select * from student where student_id not in (select student_id from absence)

in和not in 还可以将用在返回多个数据列的子查询里。

select last_name,firste_name,city, state from pressident where (city,site) in (select city ,state from president where last_name='ADDD');

##### all any 和some

其实 in 和 not in操作符和 = any和 <> all的同义词

all 和any操作符的常见用法是结合一个相对比较操作符对一个数据列子查询的结果进行测试，如果比较值小于或等于子查询所返回的每一个值<= All将为true，只要比较值小于或等于子查询所返回的任何一个值，<= any 将是true.

select last_name,first_name birth from student where birth <=all (select birth from president);

 当 all ，any 或者some操作符与“=” 比较符配合使用时，子查询可以是一个数据表子查询。此时使用一个数据行构造器来提供与子查询所返回的数据行进行比较的比较值。

select last_name,firste_name,city, state from pressident where (city,site) any (select city ,state from president where last_name='ADDD');

select last_name,firste_name,city, state from pressident where (city,site) in (select city ,state from president where last_name='ADDD');

以上两条sql语句，效果一样。

##### exists和 not exists子查询

exists和not exists操作符只测试 某个子查询是否返回了数据行，如果是，exists将是true,not exists是false.

select exists (select * from absence)

select not exists(select * from absence)

在使用exists和not exists操作符时子查询通常使用“*”作为数据列清单，这两个操作符是根据子查询是否返回了数据行判断真假，不关心起内容是什么、

##### 与主查询相关的子查询

- 与主查询无关的子查询不引用主查询的任何值。与主查询无关的子查询可以脱离主查询作为一条独立的查询命令去执行。比如说：select j from t2 where j in (select i from t1)
- 与主查询相关的子查询需要引用子查询的值，所以必须依赖主查询。因为这种关系可与主查询相关的子查询不能脱离主查询作为独立的查询。比如select j from t2 where (select i from t1 where i =j )

与主查询相关的子查询通常用 exists 和not exists子查询里。这类子查询主要用来在某个数据表里查找另一个数据表里有匹配数据行或没有匹配数据行的数据行。与主查询相关的子查询的工作情况是：把主查询传递到子查询，看到他们是否满足在子查询里面给出的条件。如果数据列中名字可能引起歧义的话建议使用数据库名字限定列

select student_id ,name from student where exists (select * from absence where absence.student_id=student.student_id);

 ##### from子句中的子查询

子查询可以用在from子句中，这种情况子查询在行为结果上就像一个数据表，from 子句里的子查询可以参加关联操作，他的值可以在where 子句中被测试，

select * from (select 1,2) as t1 inner join (select 3,4) as t2;

#### 子查询如何改为联结查询

有相当一部分的子查询可以改为连接查询。有时候连接查询要比子查询的效率更高。所以子查询改为连接查询是个更好的注意。

##### 如何改写用来选取匹配值的子查询

select * from score where event_id in (select event_id from grade_event where category='T')

 这条语句可以修改为一个联结查询：

select score.* from score inner join grade_event on score.event_id = grade_event.event_id where grade_event.category = 'T'

这类的子查询都可以更改为联结查询：

select * from table1 where colum1 in (select col2 from table2 where col2= value)

这类的查询可以改为联结查询：

select table1.* from table1 inner join table2 on table1.column1=table2.column2a where table2.column2b=value;

<font color="red">这时改为联结查询时会导致出现重复的数据行，如果防止重复在编写是使用 select  distinct 代替select</font>

##### 如果改写用来选取非匹配（缺失）值的子查询

在子查询语句中的另一个常见的用法是检索在一个数据表里有，在另一个数据表没有的值。与“哪些值没有出现”有关问题通常都可以用来left join来解决。

一般以下的子查询都可以改为连接查询：

select * from table1 where column1 not in (select column2 from table2);

可以改为以下联结查询：

select table1.* from table1 left table2 on table2.column1=table2.column2 where table2.column2 is null;

<font color="red"> 与 left join 相比，子查询具备比较直观和容易理解的优点。绝大多数人都可以毫无困难的理解为“没被包含在...里面”的含义</font>

### Mysql查询优化

查询优化是查询编译过程中的一个环节，他负责把以某种高级语言写出的数据操作语句转成一个更加详尽的过程化的操作符序列，这个序列就是所谓的[查询计划]()。

select ->查询解析器-->查询优化器--->代码生成解释器---->查询处理器----->得到查询结果

进行查询优化的4个基本技术：

- 基于开销的优化
- 启发式优化
- 语义优化
- 参数优化

mysql是一个混合使用多种优化技术的数据库系统。mysql查询优化是围绕“选取--投影--联结”策略进行的。综合了基于开销的优化器和启发式优化器的优点，生成查询计划相随较少。然后在利用基于开销的优化技术从中选出一个最短的执行路径。

#### 使用索引

索引可以加快查询的技术有很多，其中最重要的是索引，通常能够使得查询速度照成差异化。当查询速度很慢时，添加上索引后就能够迅速解决问题。

索引可以提高搜索效率的第一个原因就是我们可以得知匹配数据行在什么位置结束，从而跳过其余部分，

mysql使用索引的方式有以下几种：

- 在查询操作中把where子句所给出的条件相匹配的数据行尽快找出来，二是在关联操作中把与其他数据表的数据行相匹配的数据行尽快找出来。
- 对于使用min()或max()函数的查询，如果数据列带索引，那么它的最小值和最大值能够被迅速找到而不用通过逐行检查的方法查找
- mysql经常使用索引来迅速完成order by子句和group by子句的分类和分组凑在哦。
- 有时mysql可以通过使用索引来避免为一个查询整体读取数据行。

如果不需要某个特定的索引加快查询速度就不要创建他。

#### 挑选索引

- **尽量为用来搜索，分类或分组的数据列编制索引，不要为输出显示列编排索引。最适合有索引列的数据列是那些在where子句出现的数据列，在连接子句中给出的数据列，或者order by ,group by 子句出现的数据列。**
- **根据select关键子输出的数据列最好不要做索引。**

select

​	 col_a     <-----a candidate

from 

​	tab1 left join tb2

​	on tab1.col_b = tab12.col_c    <-- candidates

where 

​	col_d=expr          <---a candidate

- **综合考虑各数据列的维度。**

  数据列的维度等于他所能容纳的非重复值的个数。数据列的维度越高，他包含的独一无二的值就越多，重复值就越少，索引的效果就越好。

  <font color="red">查询优化程序确定出某个数值在数据表中的数据行中出现的频率超过30%时查询优化就会跳过索引。进行全表扫描。现如今的优化器更复杂，能够把其他因素也考虑过来，所以这个百分比已经不再是mysql决定进行一次扫描而不使用一个索引的唯一依据了</font>

- **对短小的值进行索引。**

  尽量选用小的数据类型，比如你的数据没有一个比25个字符更长，就不要选用char(100).比较短小的值可以从以下几个方面提高索引的处理性能。

  短小的值可以让操作更快完成，加快索引查找速度。

  短小的值可以让索引的“体积”更小，减少磁盘I/O活动

  短小的键值意味着缓存里的索引可以容纳更多的键。

- **为字符串值的前缀编排索引。**

- **充分利用最左边的前缀。**

  当创建一个n个数据列的复合索引时，复合索引 相当于n个索引，因为索引的最左边的数据列集合能够用于匹配数据行，这样的一个集合称为：最左边索引。

- **适可而止，不要建立过多的索引。**

- **让索引的类型与你打算进行比较操作的类型保持匹配。**

- **利用慢查询日志找出性能劣质的查询。**

  

#### 查询优化程序

EXPLAIN  SELECT * from domains_domain where isDeleted=0

| id   | selete_type | table          | partitions | type | possible_keys | key-len | ref  | rows  | filtered | Extra        |      |
| ---- | ----------- | -------------- | ---------- | ---- | ------------- | ------- | ---- | ----- | -------- | ------------ | ---- |
| 1    | SIMPLE      | domains_domain |            | all  |               |         |      | 75203 | 10       | using  where |      |

##### Explain

###### id

- id相同，执行顺序由上至下
- id不同，如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行
- id有相同、不同，不同的按越大越先执行，相同的按顺序执行

###### select_type

- simple：简单的select查询，查询中不包含子查询或者union
- primary：查询中若包含任何复杂的子部分，最外层查询则被标记为此
- subquery：在select或where列表中包含了子查询
- derived：在from列表中包含的子查询被标记为derived,MySQL会递归执行这些子查询

把结果放在临时表里。

- union：若第二个select出现在union之后，则标记为union；

若union包含在from子句的子查询中，外层select将被标记为：derived

- union result：从union表获取结果的select 

###### table

所操作的表

###### type

字段表明优化器可以使用索引来搜索一个特定区间的值。

性能：system > const > eq_ref > ref > range > index > all

全版：

system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > all

一般保证查询至少达到range级别，最好能达到ref

- all：full table scan 将遍历全表以找到匹配的行
- index：full index scan，index与all区别为：index类型只遍历索引树。这通常比all快，因为索引文件通常比数据文件小。（也就是说虽然all和index都是读全表，但index是从索引中读取而all是从硬盘中读）
- range：只检索给定范围的行，使用一个索引来选择行。key列显示使用了哪个索引，一般就是在你的where语句中出现了between、<、>、in等的查询。这种范围扫描，索引扫描比全表扫描要好，因为它只需要开始于索引的某一点，而结束于另一点，不用扫描全部索引。
- ref：非唯一性索引扫描，返回匹配某个单独值的所有行，本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，它可能会找到多个符合条件的行，所以他应该属于查找和扫描的混合体
- eq_ref：唯一索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描。
- const：表示通过索引一次就找到了，const用于比较primary key或者unique索引。因为只匹配一行数据，所以很快。如将逐渐置于where列表中，MySQL就能将该查询转换为一个常量
- system：表只有一行记录(等于系统表)，这是const类型的特例，平时不会出现，可以忽略不计
- null

###### possible_keys

显示可能应用在这张表中的索引，一个或多个。查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询实际使用。

###### key

实际使用的索引。如果为NULL，则没有使用索引。查询中若使用了覆盖索引，则该索引仅出现在key列表中。

###### key_len

表示索引中使用的字节数，可通过该列计算查询中使用的索引长度。在不损失精确性的情况下，长度越短越好。

key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的。

###### ref

显示索引的哪一列被使用了，如果可能的话，是一个常数。哪些列或常量被用于查找索引列上的值。

###### rows

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数。

###### extra

包含不适合在其他列中显示但十分重要的额外信息

- using filesort：说明mysql会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取。MySQL中无法利用索引完成的排序操作称为“文件排序”。
- using temporary：使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表。常见于排序order by和分组查询group by。
- using index： 表示相应的select操作中使用了覆盖索引，避免访问了表的数据行，效率不错。如果同时出现using where，表明索引被用来执行索引键值的查找；如果没有同时出现using where，表明索引用来读取数据而非执行查找动作。
- using where：表示使用了where过滤
- using join buffer：使用了连接缓存
- impossible where：where字句的值总是false，不能用来获取任何元祖
- select tables optimized away：在没有groupby子句的情况下，基于索引优化MIN/MAX操作或者对于MyISAM存储引擎优化count(*)操作，不必等到执行阶段再进行计算，查询执行计划生成阶段即完成优化。
- distinct：优化distinct操作，在找到第一匹配的元组后即停止找同样值的动作。

##### 查询优化器工作原理

查询优化器主要的目的是只要对可能就要使用索引，并且要使用条件最严格的索引来尽可能多，尽可能快地排除那些不符合索引条件的数据行。

###### 对数据表进行分析

这将生成关于索引值分情况的统计数据，他们可以帮助优化器对索引的使用效果做出更准确的评估。如果数据表经常更新就应该对其不时地进行分析；如果某数据填充好就不在发生变化，只需在加载后对其做一次分析就够了。

###### 使用Explain语句来验证优化器操作。

expain语句会告诉对定的查询有没有使用索引

###### 向优化器提供提示或在必要时屏蔽之

在连接时可以对数据表名字后面利用 force index ,use index或ignore index限定词告诉服务器想使用哪些索引。

###### 尽量使用数据类型相同的数据列进行比较

对带有索引的数据列进行比较时，如果特闷数据类型相同，查询性能就会高一些，如果查询类型不同，查询性能就会低一些。

###### 使带索引的数据列在比较表达式中单独出现

如果你在函数调用中使用数据列，或者将数据列作为算术运算表达式中很复杂的项目的一部分，mysql将不能使用索引。因为它必须要对每个数据列计算出表达式的值。

<font color="red">这个索引不能使用：select * from mytab where Year(data_col) <1990; </font>这时比较的时必须计算出数据列的值。完成整个计算结果数据列data_col的索引就不能使用，会进行全表扫描，必须使用一个准确的日期表示方法 where data_col<'1990-01-01'

<font color="red">这个索引不能使用：where TO_DAYS(date_col) - TO_DAYS(curDate()) < cutoff  因为数据列的每行都要检索才能计算出TO_DAYS(date_col)的值</font>

<font color="red">这个索引不能使用：where TO_DAYS(date_col) <cutoff +TO_DAYS(curDate()) 因为数据列的date_col仍然会出现在  函数调用中</font>

<font color="green">这个索引能使用：where date_col < DATE_ADD(currdate(), INTERVAL cutoff DAY) 这样是让日期值与数据列比较不再转换成天数，可以使用索引</font>

###### 不要在like 模式的开始位置使用通配符

where col_name like '%string%' 语句是正确的，但不要出于习惯将符号“%”放置字符串两侧、

匹配“MAC姓氏开头的”macGre mascD

where col_name like 'string%' 时优化程序看到了模式是字符开始部分会使用索引行，这样好像是就是 where las_name >='mac' and last_name <'mad'  

<font color="red">这种优化不能使用在Regexp操作表达式中，regexp表达式不能被优化</font>

###### 利用优化器的长处

Mysql支持联结查询和子查询，联结查询优化效果比对子查询的优化效果更好。

###### 试验各种查询的变化的格式，而且多次运行他们

当试验一个查询的变化2格式时每一种方式都要多运行几次。

###### 避免过多的使用Mysql的自动类型转换功能

mysql能够实现自动类型转换，但是如果能避免这些转换，

select * from mytb1 where num_col= 4;

select * from mytb1 where num_col='4' ; 包含数据转换转换，如果数据列num_col有索引，那么含有类型转换的比较有可能阻止索引得到试用、

#### 提高查询效率而挑选数据类型

##### 尽量使用数值操作，少使用字符串操作

数值运算比字符运算快的多。数值之间的比较只需要一个操作就可以完成，而字符串之间的比较一般需要进行多次字节与字节或字符与字符的比较才能完成，而且字符串比越长，比较次数就越多。

##### 如果小类型够用，就不要选用大类型

小类型比大类型的处理速度更快，字符串处理为时与他们得长度呈正比关系。选用小的数据类型可以使得整个数据表更小，从小减少磁盘读写开销，对于数据列有索引的，使用较短的数据值还将进一步提升。短索引值处理速度比长索引值的处理更快。

##### 如果能选择数据行的存储格式，就应该尽量选用最合适的存储引擎格式

##### 尽量把数据列声明为not null

数据列声明not null 时，对他的处理可以更快的完成。查询期间不再会检查数据列值是不是null了。

##### 考虑使用enum数据列

##### 利用Procedure analyse()语句

select * from tab1_name procedure analyse()

##### 对易产生碎片的数据表进行整理

##### 把数据压缩到blob或text数据列里

##### 使用人造索引

根据数据表里的其他数据列计算出一个散列值并将它来另外保存一个数据列然后通过搜索散列值去找，这里只适合精确匹配查询。Md5(),SHA1() CRC32()函数生成散列值

##### 尽量避免对很大的blob或text值进行检索

当不需要查询出大的字段时建议不要查出。

##### 把blob或text数据列剥离到单独的数据表中



#### 调度和锁定问题

mysql的默认调度策略综述如下：

- 写入比读取有更高的优先权
- 对数据表的写操作必须按照“写”请求先来后到的顺序一个一个地进行。
- 对同一个数据表进行读操作可以同时进行。

mysql提供了一些语句修饰符来改变他的调度策略。一是在delete,insert,load data, replace和update语句使用low_priority关键字，二是在select和insert语句使用high_priority关键字，三是在insert和replace语句中使用delayed关键字。

low_priority和high_priority限定符只对支持数据表锁定功能的存储引擎。有效果，delayed限定符在myISAM,MEMEORY,ARCHIVE和BLACKHOLE存储引擎上都可以使用。

##### 改变语句优先级

low_priority关键字影响delete,insertmload_data，replace和update语句的调度。

写等待：当一个数据表正在读取时来了一个写的操作，则写操作就会等待读操作完成之后。默认的调度策略是写入者比读取者优先

对于high_priority关键字的select查询，情况也是类似。允许select查询操作插入正在等待的写入操作之前。即使通常情况下写入操作有较高的优先。另外一个高优先的select将先于普通的select语句执行。

##### 使用延迟插入

delayed修饰符用在insert和replace语句中，当delayed请求到达数据表时，服务器将数据行排序并且迅速地返回到客户程序的状态，从而使客户程序可以在数据行被插入之前进行。

##### 使用并发插入

并发插入应注意以下问题。

- 不要在insert语句中使用low_priority修饰符，这会时insert语句总是锁定读取者，从而导致并发插入不能完成。
- 那些需要显式锁定数据表但又想允许并发插入的读取者应该使用lock tables ... read local 而不是lock tables ...read。关键字local会使你得到一个允许并发插入的锁。因为它只用于在数据表中已经存在的数据行并不锁定正在被添加到数据表的末尾的新行，
- 应该给load data嘉善concurrent限定符，让给数据表上的select语句在该数据表正在加载数据的同时仍可以执行。

### MYSQL事务

事务（Transaction），一般是指要做的或所做的事情。在计算机术语中是指访问并可能更新数据库中各种数据项的一个程序执行单元(unit)。事务通常由高级数据库操纵语言或编程语言（如SQL，C++或Java）书写的用户程序的执行所引起，并用形如begin transaction和end transaction语句（或函数调用）来界定。事务由事务开始(begin transaction)和事务结束(end transaction)之间执行的全体操作组成。

#### 事务应该具有4个属性：

原子性、一致性、隔离性、持久性。这四个属性通常称为ACID特性。

- 原子性（atomicity）。一个事务是一个不可分割的工作单位，事务中包括的操作要么都做，要么都不做。
- 一致性（consistency）。事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。
- 隔离性（isolation）。一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
- 持久性（durability）。持久性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。将数据库设计成单线程的数据库，可以防止所有线程安全问题，但是实际上是行不通的；

多个线程开启各自事务操作数据库中数据时，数据库系统要负责隔离操作，以保证各个线程在获取数据时的准确性。
如果是两个线程并发修改一定会干扰，这时就需要用锁的机制防止并发修改；
如果两个线程是查询，没有线程安全问题；
事务的使用：
事务是指逻辑上的一组操作，这组操作要么同时完成要么同时失败，数据库会管理事务，方式是一条语句独占一个事务
或者自己用以下的语句来操作：
Connection.setAutoCommit(false); //  相当于start transaction
Connection.rollback();  rollback
Connection.commit();  commit

要先把事务打开然后再进行事务的提交，只有提交后才能结束本次的操作；

#### 四大隔离级别：

安全性级别：

Serializable：可避免脏读、不可重复读、虚读情况的发生。（串行化）
Repeatable read：可避免脏读、不可重复读情况的发生。（可重复读）不可以避免虚读
Read committed：可避免脏读情况发生（读已提交）
Read uncommitted：最低级别，以上情况均无法保证。(读未提交)
效率性：由后往前；
真正使用数据的时候，根据自己使用的数据库的需求，综合分析针对安全性和效率的要求来选择一个隔离级别；
<font color="red">mysql默认是Repeatable read：可避免脏读、不可重复读情况的发生。</font>

oracle默认是read committed隔离级别；
其中如果不写括号里的是默认是session指的是当前客户端的；

set [global/session]  transaction isolation level 设置事务隔离级别
select @@tx_isolation 查询当前事务隔离级别

```
start transaction;

insert into t set name='aaa';

commit;
```



### Mysql锁表查询：

select * from information_schema.INNODB_TRX  #查看当前运行的所有事务
select * from information_schema.INNODB_LOCKS # 当前出现的锁
SELECT * from information_schema.INNODB_LOCK_WAITS # 锁等待的对应的关系
show processlist # 查看进程列表
kill 1362 # 杀死进程Id
desc information_schema.innodb_locks; # 查看锁的信息
SHOW VARIABLES LIKE 'innodb_lock_wait_timeout' # 查看锁等待的参数



### MYSQL變量--检查变更存在的一些影响

在某些情况下某个变量设置的值设置的太大或者太小，会导致改变量的值被服务器丢弃，如果怀疑变更对服务器是否照成影响使用“show [session|gloabal] variables like '变量名称';” 或者使用查询：“show variable_value from infomation_schema.[session|global]_variables where variable_name='变量名称'”

#### 与服务器相关的变量

限制与max_*变量：

如限制结果集的大小：set group_concat_max_len=100

查看字符集：

show variables like '%char%';

#### 与操作系统处理有关lower_case*参数

lower_case_filesystem 与 lower_case_table_names选项跟字符集选项的作用相似，这些最后不要修改

#### 初始sql

init_file:指向包含在服务器启动时应该执行的sql语句的一个文件

init_connect:包含每个客户端连接时需要执行的一个sql字符串

init_slave包含当一个服务器作为从服务器启动其sql线程时需要执行的一个sql字符串、。

open_files_limit:限制打开文件句柄的数量，限制越高打开的表文件与临时表越多，处理的并发连接数越多。

log_warnings:当此选项非零(打开)时就会在服务器的错误日志文件写入警告信息。如果设置为2，此选项告诉服务器记录连接错误。使用同步复制时，在主服务器上开启此选项非常重要，能够确定服务器IO线程何时失去连接。当设置为1时输出自己的诊断信息。

#### 复制选项

主从服务器选项关系：

binlog-* 与replicate- * 过滤器。通过binglog-do-*,replicate-do- * binlog-ignore- *与replicate-ignore- * 选项在复制过程中又能能力顾虑对象。

二进制日志格式：

binlog_format变量允许你复制的格式：STATEMENT,ROW或MixED

binlog_direct_non_transactional_updates指定何时非事务表更新写入二进制文件

### InnoDB选项

#### innodb_autoinc_lock_mode

innodb_file_per_table

innodb_table_locks 定义InnoDB是如何处理locktables语句发出的表锁请求。

innodb_lock_wait_timeout 等待行锁直到放弃的秒数。

innodb_rollback_on_timeout 当查询因锁等待错误而中断时，只有最后一条语句回滚了，整个事务没有终止。

innodb_use_navite_aio指定是否使用linux下的原生的AIO接口，或者是自己实现“模拟AIO”

innodb_locks_unsafe_for_binlog定义InnoDB如何使用间隙锁来搜索和扫描索引，默认0间隙锁开启，设为1时禁用间隙锁。

#### 超时

connect_timeout 设置使用mysql服务器和客服端交换授权数据包

interactive_timeout交互式客户端在断开连接之前等待多长时间。

wait_timeout在断开连接前等待任何客户端中活动的时间。如果客户端是交互式的并且interactive_timeout的值不同于wait_timeout则以interactive_timeout为准

net_read_timeout从客服端写入mysql服务器等待应答时间

net_write_timeout从客服端读取mysql服务器等待应答时间