angularjs学习
============

AngularJS 是一个 JavaScript 框架。它是一个以 JavaScript 编写的库。

AngularJS 是以一个 JavaScript 文件形式发布的，可通过 script 标签添加到网页中：

    <script src="angular.js"></script>

AngularJS官方文档: [https://docs.angularjs.org/guide](https://docs.angularjs.org/guide)

---

### 什么是 AngularJS？

"AngularJS 是专门为应用程序设计的 HTML。"

AngularJS 使得开发现代的单一页面应用程序（SPAs：Single Page Applications）变得更加容易。

* AngularJS 把应用程序数据绑定到 HTML 元素。
* AngularJS 可以克隆和重复 HTML 元素。
* AngularJS 可以隐藏和显示 HTML 元素。
* AngularJS 可以在 HTML 元素"背后"添加代码。
* AngularJS 支持输入验证。

---

### AngularJS 表达式

AngularJS 表达式写在双大括号内：{{ expression }}。

AngularJS 表达式把数据绑定到 HTML，这与 ng-bind 指令有异曲同工之妙。

AngularJS 将在表达式书写的位置"输出"数据。

AngularJS 表达式 很像 JavaScript 表达式：它们可以包含文字、运算符和变量。

实例 {{ 5 + 5 }} 或 {{ firstName + " " + lastName }}

### AngularJS 指令

AngularJS 指令是扩展的 HTML 属性，带有前缀 ng-。

* ng-app	定义应用程序的根元素。
* ng-bind	绑定 HTML 元素到应用程序数据。
* ng-click	定义元素被单击时的行为。
* ng-controller	为应用程序定义控制器对象。
* ng-disabled	绑定应用程序数据到 HTML 的 disabled 属性。
* ng-init	为应用程序定义初始值。
* ng-model	绑定应用程序数据到 HTML 元素。
* ng-repeat	为控制器中的每个数据定义一个模板。
* ng-show	显示或隐藏 HTML 元素。

### AngularJS 控制器

AngularJS 应用程序被控制器控制。

ng-controller 指令定义了应用程序控制器。

控制器是 JavaScript 对象，由标准的 JavaScript 对象的构造函数 创建。

控制器的 $scope 是控制器所指向的应用程序 HTML 元素。

### AngularJS 过滤器

过滤器可以使用一个管道字符（|）添加到表达式和指令中。

AngularJS 过滤器可用于转换数据：

* currency	格式化数字为货币格式。
* filter	从数组项中选择一个子集。
* lowercase	格式化字符串为小写。
* orderBy	根据某个表达式排列数组。
* uppercase	格式化字符串为大写。

angular小实例：

    <!DOCTYPE html>
    <html>
        <head>
        </head>
        <body>
        <div ng-app="biang" ng-controller="biangControl"> 

            <ul>
              <li ng-repeat="x in names">
                {{ x.Name + ', ' + x.Country }}
              </li>
            </ul>

            
            <table>
              <tr ng-repeat="x in names">
                <td>{{ x.Name }}</td>
                <td>{{ x.Country }}</td>
              </tr>
            </table>

        </div>
        <script src="bower_components/angular/angular.min.js"></script>
        <script>
        var app = angular.module("biang", []);
        app.controller("biangControl", function($scope, $http) {
            $http.get("json.html").success(function(response) {
                $scope.names = response;
            });
        });
        </script>


        </body>
    </html>
