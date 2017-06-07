# mysql当中update select结果集

在oracle,及sql server中，我们可是使用以下update语句对表进行更新：

    update a set a.xx= (select yy from b) where a.id = b.id ;

但是在mysql中，不能直接使用set select的结果，必须使用inner join：

    update a inner join (select yy from b) c on a.id =b.id  set a.xx = c.yy
