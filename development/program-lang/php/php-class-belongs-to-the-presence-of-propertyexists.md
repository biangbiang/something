# PHP 判断类属于是否存在 property_exists

你可能会说有isset 来看一下isset做不到的 .property\_exists 能做到的

### Demo1

    class demo {
      public $property = TRUE;
    }
     
    $demo = new demo();
     
    var_dump(isset($demo->property));  //true
    var_dump(property_exists($demo, 'property')); //true

### Demo2

    class demo {
      private $property = TRUE;
    }
     
    $demo = new demo();
     
    var_dump(isset($demo->property));  //false
    var_dump(property_exists($demo, 'property')); //true
