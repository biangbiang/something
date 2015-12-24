php标准注释
===========

### 文件头部模板

    /** 
    *这是一个什么文件 
    * 
    *此文件程序用来做什么的（详细说明，可选。）。 
    * @author      xxx<xxx@xxx.com> 
    * @version     $Id$ 
    * @since        1.0 
    */  

### 函数头部注释

    /** 
    * some_func  
    * 函数的含义说明 
    * 
    * @access public 
    * @param mixed $arg1 参数一的说明 
    * @param mixed $arg2 参数二的说明 
    * @param mixed $mixed 这是一个混合类型 
    * @since 1.0 
    * @return array 
    */  
    public function thisIsFunction($string, $integer, $mixed) {return array();}  

### 类的注释

    /** 
    * 类的介绍 
    * 
    * 类的详细介绍（可选。）。 
    * @author         xxx<xxx@xxx.com> 
    * @since          1.0 
    */  
    class Test   
    {  
    }  

### 程序代码注释

1. 注释的原则是将问题解释清楚，并不是越多越好。

2. 若干语句作为一个逻辑代码块，这个块的注释可以使用/* */方式。

3. 具体到某一个语句的注释，可以使用行尾注释：//。

 
        /* 生成配置文件、数据文件。*/  
          
        $this->setConfig();  
        $this->createConfigFile();  //创建配置文件  
        $this->clearCache();         // 清除缓存文件  
        $this->createDataFiles();   // 生成数据文件  
        $this->prepareProxys();  
        $this->restart();  
