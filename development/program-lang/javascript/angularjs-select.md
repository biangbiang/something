AngularJS select详细用法
========================

select 是 AngularJS 预设的一组directive。下面是其官网api doc给出的用法：[AngularJS:select](http://docs.angularjs.org/api/ng.directive:select)

大意是，select中的ngOption可以采用和ngRepeat指令类似的循环结构，其data source可以是array或者是object。针对两种data source又有衍生的好几种用法。但是官网的例子实在是太少了。

下面是针对几个不太容易理解的用法的例子。

先上controller

    <!-- lang: js -->
    function selectCtrl($scope) {
        $scope.selected = '';

        $scope.model = [{
            id: 10001,
            mainCategory: '男',
            productName: '水洗T恤',
            productColor: '白'
        }, {
            id: 10002,
            mainCategory: '女',
            productName: '圆领短袖',
            productColor: '黑'
        }, {
            id: 10003,
            mainCategory: '女',
            productName: '短袖短袖',
            productColor: '黃'
        }];
    }

### 实例一：基本下拉效果

usage: label for value in array


    <select ng-model="selected" ng-options="m.productName for m in model">
        <option value="">-- 请选择 --</option>
    </select>

效果:

![](http://biang.io/biangpic/blog/1517526dd33e0db82551af16245322ae.png)

说明

1.usage中的 value 也就是 ng-options 中的 m，而 m 是数组model的一个元素，它是一个变量

2.usage中的 label 也就是 ng-options 中的m.productName， 其实就是一个 AngularJS Expression

### 实例二：自定义下拉显示名称

usage: label for value in array


    <select ng-model="selected" ng-options="(m.productColor + ' - ' + m.productName) for m in model">
        <option value="">-- 请选择 --</option>
    </select>

效果

![](http://biang.io/biangpic/blog/eca944e5f7b22a442a2540f5e3172bb7.png)

说明

1.可以看到，usage 中的 label 可以根据需求拼接出不同的字符串

### 实例三： 让选项分组

usage: label group by group for value in array


    <select ng-model="selected" ng-options="(m.productColor + ' - ' + m.productName) group by m.mainCategory for m in model">
        <option value="">-- 请选择 --</option>
    </select>

效果

![](http://biang.io/biangpic/blog/869dbe766f4b7acd7bfb9347599044b2.png)

说明

1.这里使用了group by，通过$scope.model中的mainCategory字段进行分组

### 实例四：自定义ngModel的值

usage: select as label for value in array

    <select ng-model="selected" ng-options="m.id as m.productName for m in model">
        <option value="">-- 请选择 --</option>
    </select>

效果

![](http://biang.io/biangpic/blog/b5d96037c388dac7f4d65c8642856a98.png)

说明

1. 这种用法也是select指令最复杂的一种。从效果中可以看出，usage中select的作用就是重新定义ng-model的值。在这里，ng-model等于m.id，当ng-model发生改变的时候，得到的是m.id的值

### 参考

1.http://docs.angularjs.org/api/ng.directive:select

2.http://blog.miniasp.com/post/2013/05/12/AngularJS-ng-module-select-ngOptions-usage-samples.aspx
