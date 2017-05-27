# MYSQL escape用法

在sql like语句中，比如

    select * from user where username like '%nihao%'
    select * from user where username like '_nihao',

其中%做为通配符通配多个，\_作为通配符通配一个

如果要真的去查询username中含有 `% _` 的，需要使他们不再作为通配符

将`% _` 在like中转义，拿\_为例，

转义前：`select * from user where username like '_nihao'`,

转义后：`select * from user where username like '/_nihao' escape '/'`,意思就是说/之后的\_不作为通配符
