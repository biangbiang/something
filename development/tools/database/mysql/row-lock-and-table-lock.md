mysql的行锁与表锁
=================

mysql的行锁与表锁。(select* .... FOR UPDATE)

### MySQL

MySQL中使用select for update的必须针对InnoDb，并且是在一个事务中，才能起作用。

select的条件不一样，采用的是行级锁还是表级锁也不一样。

---

由於 InnoDB 預設是 Row-Level Lock，所以只有「明確」的指定主鍵，MySQL 才會執行 Row lock (只鎖住被選取的資料例) ，否則 MySQL 將會執行 Table Lock (將整個資料表單給鎖住)。

舉個例子:

假設有個表單 products ，裡面有 id 跟 name 二個欄位，id 是主鍵。

例1: (明確指定主鍵，並且有此筆資料，row lock)

    SELECT * FROM products WHERE id='3' FOR UPDATE;

例2: (明確指定主鍵，若查無此筆資料，無 lock)

    SELECT * FROM products WHERE id='-1' FOR UPDATE;

例2: (無主鍵，table lock)

    SELECT * FROM products WHERE name='Mouse' FOR UPDATE;

例3: (主鍵不明確，table lock)

    SELECT * FROM products WHERE id<>'3' FOR UPDATE;

例4: (主鍵不明確，table lock)

    SELECT * FROM products WHERE id LIKE '3' FOR UPDATE;

註1:

    FOR UPDATE 僅適用於 InnoDB，且必須在交易區塊(BEGIN/COMMIT)中才能生效。

註2:

要測試鎖定的狀況，可以利用 MySQL 的 Command Mode ，開二個視窗來做測試。
