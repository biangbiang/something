# ECMAScript5 Object的新属性方法

虽然说现在并不是所有的浏览器都已经支持ECMAScript5的新特性，但相比于ECMAScript4而言ECMAScript5被广大浏览器厂商广泛接受，目前主流的浏览器中只有低版本的IE不支持，其它都或多或少的支持了ECMAScript5的新特性，其中重中之重自然是一切对象的基类型——Object

### Object.create(prototype[,descriptors])

这个方法用于创建一个对象，并把其prototype属性赋值为第一个参数，同时可以设置多个descriptors，关于decriptor下一个方法就会介绍这里先不说。只需要这样就可以创建一个原型链干净对象了

    var o = Object.create({
                "say": function () {
                    alert(this.name);
                },
                "name":"Byron"
            });

![](http://biangbiangpic.b0.upaiyun.com/blog/0f4849f82d19e347e3aa3ad6a5cd9391.png)

### Object.defineProperty(O,Prop,descriptor) / Object.defineProperties(O,descriptors)

想明白这两个函数必须明白descriptor是什么，在之前的JavaScript中对象字段是对象属性，是一个键值对，而在ECMAScript5中引入property，property有几个特征

1. value：值，默认是undefined

2. writable：是否是只读property，默认是false,有点像C#中的const

3. enumerable：是否可以被枚举(for in)，默认false

4. configurable：是否可以被删除，默认false

同样可以像C#、Java一样些get/set，不过这两个不能和value、writable同时使用

5.get:返回property的值得方法，默认是undefined

6.set：为property设置值的方法，默认是undefined

    Object.defineProperty(o,'age', {
                value: 24,
                writable: true,
                enumerable: true,
                configurable: true
            });

            Object.defineProperty(o, 'sex', {
                value: 'male',
                writable: false,
                enumerable: false,
                configurable: false
            });

            console.log(o.age); //24
            o.age = 25;

            for (var obj in o) {
                console.log(obj + ' : ' + o[obj]);
                /*
                age : 25  //没有把sex ： male 遍历出来
                say : function () {
                    alert(this.name);
                } 
                name : Byron 
                */
            }
            delete o.age;
            console.log(o.age);//undefined 

            console.log(o.sex); //male
            //o.sex = 'female'; //Cannot assign to read only property 'sex' of #<Object> 
            delete o.age; 
            console.log(o.sex); //male ,并没有被删除

也可以使用defineProperties方法同时定义多个property，

    Object.defineProperties(o, {
                'age': {
                    value: 24,
                    writable: true,
                    enumerable: true,
                    configurable: true
                },
                'sex': {
                    value: 'male',
                    writable: false,
                    enumerable: false,
                    configurable: false
                }
            });

### Object.getOwnPropertyDescriptor(O,property)

这个方法用于获取defineProperty方法设置的property 特性

    var props = Object.getOwnPropertyDescriptor(o, 'age');
            console.log(props); //Object {value: 24, writable: true, enumerable: true, configurable: true}

### Object.getOwnPropertyNames

获取所有的属性名，不包括prototy中的属性，返回一个数组

    console.log(Object.getOwnPropertyNames(o)); //["age", "sex"]

例子中可以看到prototype中的name属性没有获取到

### Object.keys()

和getOwnPropertyNames方法类似，但是获取所有的可枚举的属性，返回一个数组

    console.log(Object.keys(o)); //["age"]

上面例子可以看出不可枚举的sex都没有获取的到

### Object.preventExtensions(O) / Object.isExtensible

方法用于锁住对象属性，使其不能够拓展，也就是不能增加新的属性，但是属性的值仍然可以更改，也可以把属性删除，Object.isExtensible用于判断对象是否可以被拓展

    console.log(Object.isExtensible(o)); //true
            o.lastName = 'Sun';
            console.log(o.lastName); //Sun ,此时对象可以拓展

            Object.preventExtensions(o);
            console.log(Object.isExtensible(o)); //false

            o.lastName = "ByronSun";
            console.log(o.lastName); //ByronSun，属性值仍然可以修改

            //delete o.lastName;
            console.log(o.lastName); //undefined仍可删除属性

             o.firstname = 'Byron'; //Can't add property firstname, object is not extensible 不能够添加属性
             
### Object.seal(O) / Object.isSealed

方法用于把对象密封，也就是让对象既不可以拓展也不可以删除属性（把每个属性的configurable设为false）,单数属性值仍然可以修改，Object.isSealed由于判断对象是否被密封

    Object.seal(o);
            o.age = 25; //仍然可以修改
            delete o.age; //Cannot delete property 'age' of #<Object>

### Object.freeze(O) / Object.isFrozen

终极神器，完全冻结对象，在seal的基础上，属性值也不可以修改（每个属性的wirtable也被设为false）

    Object.freeze(o);
            o.age = 25; //Cannot assign to read only property 'age' of #<Object>

### 最后

上面的代码都是在Chrome 29下一严格模式（’use strict’）运行的，而且提到的方法都是Object的静态函数，也就是在使用的时候应该是Object.xxx(x)，而不能以对象实例来调用。总体来说ES5添加的这些方法为javaScript面向对象设计提供了进一步的可配置性，用起来感觉很不错。
