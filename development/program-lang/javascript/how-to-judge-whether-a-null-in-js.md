# JS中如何判断null

以下是不正确的方法：
 
    var exp = null;
    if (exp == null)
    {
        alert("is null");
    }

exp 为 undefined 时，也会得到与 null 相同的结果，虽然 null 和 undefined 不一样。

注意：要同时判断 null 和 undefined 时可使用本法。
 
    var exp = null;
    if (!exp)
    {
        alert("is null");
    }

如果 exp 为 undefined，或数字零，或 false，也会得到与 null 相同的结果，虽然 null 和二者不一样。

注意：要同时判断 null、undefined、数字零、false 时可使用本法。
 
    var exp = null;
    if (typeof exp == "null")
    {
        alert("is null");
    }

为了向下兼容，exp 为 null 时，typeof null 总返回 object，所以不能这样判断。
 
    var exp = null;
    if (isNull(exp))
    {
        alert("is null");
    }

VBScript 中有 IsNull 这个函数，但 JavaScript 中没有。
 
---
 
以下是正确的方法：
 
    var exp = null;
    if (!exp && typeof exp != "undefined" && exp != 0)
    {
        alert("is null");
    }

typeof exp != "undefined" 排除了 undefined；

exp != 0 排除了数字零和 false。
 
更简单的正确的方法：
 
    var exp = null;
    if (exp === null)
    {
        alert("is null");
    }
 
---
 
尽管如此，我们在 DOM 应用中，一般只需要用 (!exp) 来判断就可以了，因为 DOM 应用中，可能返回 null，可能返回 undefined，如果具体判断 null 还是 undefined 会使程序过于复杂。
