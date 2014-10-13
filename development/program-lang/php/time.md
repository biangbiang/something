php时间计算
===========

    strtotime可以任意加减年、月、日
    
    $endtime='2009-02-28 16:29:18';
    
    $endtime = date('Y-m-d H:i:s', strtotime($endtime.'+15day +1 hour -10minute'));
    echo $endtime;
    
    当前日期：2008-07-10
    echo date("Y-m-d",strtotime("+3 day"));
    
    // 输出：2008-07-13
    
    echo date("Y-m-d",strtotime("+3 month"));
    
    // 输出：2008-10-10
    
    echo date("Y-m-d",strtotime("+3 year"));

---

	获取当前时间戳
	echo time()

	获得当前时间
	echo date('Y-m-d H:i:s',time());
