mootools里选择器$,$$,$E,$ES等的区别
===================================

区别就是:

* $和$$都是1个参数，

* $适用于ID，或者ID代表的对象

* $$适用于CSS选择器

 

$E和$ES，有2个参数，第二个参数是可选参数代表(filter，即某个ID范围里的元素)

    $E('input[type=text]','myform');//ID为myform里的第一个文本框
    $ES('input[type=text]','myform');//ID为myform里的所有text文本框

---

## Understanding Mootools Selectors $, $$, $E and $ES

Tagged $, $$, dollar, Javascript, Mootools

While browsing the Mootools forums I came across an excellent post by Fløe Rasmus. He explains to a mootools newbie how to use the selector functions $ and $$. In this article I’ll explain how $, $$, $E and $ES work, and as a bonus I’ll list all the dollar functions in Mootools v1.11.

### The $ (dollar) function

Most people think this is just a shortcut for document.getElementById(), but actually it’s not. The $() takes one argument. This argument is called mixed in php terms, because it can be a string, or a dom element. Whenever the argument is a string s, it returns the element with id s with all Element methods applied. When the argument is a dom element, it just applies all Element methods to the element, and returns it. Here’s how it works:

    var w3cEl = document.getElementById('myDiv'); // w3cEl => W3C dom element
    var mooEl = $('myDiv'); // mooEl => Mootools Element
    var mooEl2 = $(w3cRef); // mooEl2 => Mootools Element
    var mooEl3 = $(mooEl2); // mooEl3 => mooEl2

So the $ function applies the Element methods to it’s argument. When passing a Mootools Element to $ it ‘ll be detected and no methods are applied, because the argument already has those methods (see example above mooEl3).

### The $$ (double dollar) function

The double dollar function is more like document.getElementsByTagName() on steroids, both will return an array with multiple elements.

    var w3cArr = document.getElementsByTagName('div'); // w3cArr => Array of W3C dom elements
    var mooArr = $$('div'); // mooArr => Array of Mootools Elements

But $$ can do more. It’ll accept one or more css selectors (or elements) and return an array of elements matching those selectors:

    $$('a.external'); // => Array of links with class 'external'
    $$('a[href=#]'); // => Array of links with href="#"
    $$('form input[disabled]'); // Array of input elements inside a form with a disabled attribute
    $$('form input[disabled=disabled]'); // Array of disabled input elements inside a form (valid XHTML)
    $$('div[class^=foo]'); // => Array of divs with classname starting with 'foo'
    $$('[class$=bar]'); // => Array of elements with classname ending with 'bar'
    $$('*[class$=bar]'); // => Returns the same as the previous selector

Want to know more about the css3 selectors? Read the W3C css3 selector specification. Keep in mind the Mootools $$ function doesn’t support all selectors.

### The $E function

This function is very much like the $ function. The first argument of $E is a css selector string and it returns the first found element found with the selector. The second element is the filter (id string or DOM element). See it like a scope, when the filter is passed, the selector is executed inside the filter element. So instead of passing an id to the dollar function, you can pass a css selector to $E.

    $E('div#foo'); // => Same as $('foo')
    $E('ul#bar li'); // => The first list item of the unordered list with id 'bar'
    $E('form input[type=checkbox]'); // => First checkbox of the first form
    $E('input[type=checkbox]', 'myForm'); // => First checkbox of the element with id 'myForm'

### The $ES (Element.getElements) function

The $ES function is very much the same as the $$ and the $E functions. The first argument is the css selector, and the second argument is the filter. $ES returns an array of elements found with the css selector, which is executed inside the filter element. It’s pretty straightforward:

    $ES('a.external'); // => Same as $$('a.external')
    $ES('a.external', document); // => Same as $ES('a.external')
    $ES('input', 'myForm'); // => Array of input elements within the element with id 'myForm'
    $ES('input[type=text]', 'myForm'); // => Array of textbox elements within the element with id 'myForm'

### Bonus: other $ functions

There are some more dollar function that come with Mootools:

* $chk: Returns true if the passed in value/object exists or is 0, otherwise returns false.
* $clear: Clears a timeout or an Interval.
* $defined: Returns true if the passed in value/object is defined, that means is not null or undefined.
* $extend: Copies all the properties from the second passed object to the first passed Object.
* $merge: Merges a number of objects recursively without referencing them or their sub-objects.
* $native: Will add a .extend method to the objects passed as a parameter, but the property passed in will be copied to the object’s prototype only if non previously existent.
* $pick: Returns the first object if defined, otherwise returns the second.
* $random: Returns a random integer number between the two passed in values.
* $time: Returns the current timestamp.
* $type: Returns the type of object that matches the element passed in.
* $A: Returns a copy of the passed Array.
* $each: Use to iterate through iterables that are not regular arrays, such as builtin getElementsByTagName calls, arguments of a function, or an object.
* $H: Shortcut to create a Hash from an Object.
* $RGB: Shortcut to create a new color, based on red, green, blue values.
* $HSV: Shortcut to create a new color, based on hue, saturation, brightness values.
