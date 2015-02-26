perl智能匹配操作符~~
====================

## 介绍

智能匹配操作符，英文叫做smart matching operator，写法是连续的两个波浪线~~，为什么说它是智能的呢？因为它能够根据操作符两侧的操作数进行智能匹配，具体智能到什么程度呢？听我一一道来！

## 具体应用

### 案例一，判断某个元素是否在给定的数组中

这算是一个比较常见的问题，如果不用智能匹配操作符的话，我想多数人会这样写程序

    my $value = 3 ;
    my @array = (1, 2, 3, 4, 5) ;

    sub test{
        for(@array){
            if ($value == $_){
                print "$value was found!\n" ;
                return ;
            }
        }
        print "$value was not found!\n" ;
    }

但是，有了智能匹配操作符，程序就简单多了，如下

    sub test{
        if(@array ~~ $value){
            print "$value was found!\n" ;
        }
        else{
            print "$value was not found!\n" ;
        }
    }

### 案例二，判断两个数组所有元素是否相同

通常的做法是，依次比较两个数组对应位置的元素，如果有不相等的元素，立即返回0，如果都相等，则返回1，程序应该是下面的样子。

    sub test{
        for my $i (0 .. $#array1){
            if($array1[$i] != $array2[$i]){
                return 0 ;
            }
        }
        return 1 ;
    }

有了智能匹配操作符，可以像下面这样写啦，太简单了！

    sub test{
        if(@array1 ~~ @array2){
            return 1 ;
        }
        else{
            return 0 ;
        }
    }
    
### 案例三，正则表达式匹配

~~可以完全代替=~进行匹配，而且比=~更强大，比如要判断数组中是否有满足匹配的元素，直接可以这样写

    my @array = ("abcd", "xyz", "123", 456) ;
    print "found match!\n" if @array ~~ /xyz/ ;

没有必要再逐个元素进行匹配了。

### 案例四，普通比较

~~还能代替普通的比较操作符，我们知道，在perl中，数字比相等较用==，字符串相等比较用eq，有了~~，就不必考虑类型问题了，它会根据待比较的数选择合适的操作符进行比较的

    print "number equal\n" if 1 ~~ 2 ;
    print "string equal\n" if 'abc' ~~ 'abc' ;

### smart matching in details

智能匹配到底能做多少事？这里有个详细的列表。

[http://perldoc.perl.org/perlsyn.html#Switch-statements](http://perldoc.perl.org/perlsyn.html#Switch-statements) 在网页上搜索smart matching in detail即可。


