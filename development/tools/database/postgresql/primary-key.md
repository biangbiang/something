PostgreSQL主键
==============

## Postgresql主键自增

在postgres中，

主键约束只是唯一约束和非空约束的组合。所以，下面两个表定义是等价的：

    CREATE TABLE products (
        product_no integer UNIQUE NOT NULL,
        name text,
        price numeric
    );
    CREATE TABLE products (
        product_no integer PRIMARY KEY,
        name text,
        price numeric
    );
 
主键也可以约束多于一个字段；其语法类似于唯一约束：

    CREATE TABLE example (
        a integer,
        b integer,
        c integer,
        PRIMARY KEY (a, c)
    );

主键表示一个或多个字段的组合可以用于唯一标识表中的数据行。这是定义一个主键的直接结果。请注意：一个唯一约束（ unique constraint ）实际上并不能提供一个唯一标识，因为它不排除 NULL 。

一个表最多可以有一个主键(但是它可以有多个唯一和非空约束)。关系型数据库理论告诉我们，每个表都必须有一个主键。PostgreSQL 并不强制这个规则，但我们最好还是遵循它。

值得注意的是，SQLite中，主键是自动 增长的

在MySQLI中，需要加一个`auto_increment` 标志

在Postgres中，有一个专门的类型 叫做serial的，来表示自动增加

为每一行生成一个”序列号（serial number）“。在 PostgreSQL 里，通常是用类似下面这样的方法生成的：

    CREATE TABLE products (
        product_no integer DEFAULT nextval('products_product_no_seq'),
        ...
    );

这里的 nextval() 从一个序列对象(sequence object)提供后继的数值。这种做法非常普遍，以至于我们有一个专门的缩写用于此目的：

    CREATE TABLE products (
        product_no SERIAL,
        ...
    );

其实， 自动增加 字段是 default字段 的一种特殊情况

可以在创建表之后，再次创建sequence

    CREATE SEQUENCE event_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

    alter table event alter column id set default nextval('event_id_seq');

或者修改字段类型

    ALTER TABLE event ALTER COLUMN id TYPE serial;

添加主键

    ALTER TABLE charge ADD PRIMARY KEY (id);

---

## Postgresql 创建主键并设置自动递增的三种方法 

Postgresql 有以下三种方法设置主键递增的方式，下面来看下相同点和不同点。

### 方法一
    create table test_a 
    (
      id serial,
      name character varying(128),
      constraint pk_test_a_id primary key( id)
    ); 

    NOTICE:  CREATE TABLE will create implicit sequence "test_a_id_seq" for serial column "test_a.id"
    NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "pk_test_a_id" for table "test_a"
    CREATE TABLE


### 方法二

    create table test_b
    (
      id serial PRIMARY KEY,
      name character varying(128)
    ); 

    NOTICE:  CREATE TABLE will create implicit sequence "test_b_id_seq" for serial column "test_b.id"
    NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "test_b_pkey" for table "test_b"
    CREATE TABLE


### 方法三

    create table test_c 
    (
      id integer PRIMARY KEY,
      name character varying(128)
    );  
    NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "test_c_pkey" for table "test_c"
    CREATE TABLE


    CREATE SEQUENCE test_c_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1;
        
    alter table test_c alter column id set default nextval('test_c_id_seq');


很明显从上面可以看出，方法一和方法二只是写法不同，实质上主键都通过使用 serial 类型来实现的，使用serial类型，PG会自动创建一个序列给主键用，当插入表数据时如果不指定ID，则ID会默认使用序列的NEXT值。    
    
方法三是先创建一张表，再创建一个序列，然后将表主键ID的默认值设置成这个序列的NEXT值。这种写法似乎更符合人们的思维习惯，也便于管理，如果系统遇到sequence 性能问题时，便于调整 sequence 属性；

### 比较三个表的表结构

    skytf=> \d test_a
                                     Table "skytf.test_a"
     Column |          Type          |                      Modifiers                      
    --------+------------------------+-----------------------------------------------------
     id     | integer                | not null default nextval('test_a_id_seq'::regclass)
     name   | character varying(128) | 
    Indexes:
        "pk_test_a_id" PRIMARY KEY, btree (id)
        
    
    skytf=> \d test_b
                                     Table "skytf.test_b"
     Column |          Type          |                      Modifiers                      
    --------+------------------------+-----------------------------------------------------
     id     | integer                | not null default nextval('test_b_id_seq'::regclass)
     name   | character varying(128) | 
    Indexes:
        "test_b_pkey" PRIMARY KEY, btree (id)
            
        
    skytf=> \d test_c
                                     Table "skytf.test_c"
     Column |          Type          |                      Modifiers                      
    --------+------------------------+-----------------------------------------------------
     id     | integer                | not null default nextval('test_c_id_seq'::regclass)
     name   | character varying(128) | 
    Indexes:
        "test_c_pkey" PRIMARY KEY, btree (id)
    
从上面可以看出，三个表表结构一模一样， 三种方法如果要寻找差别，可能仅有以下一点，

当 drop 表时，方法一和方法二会自动地将序列也 drop 掉, 而方法三不会。
