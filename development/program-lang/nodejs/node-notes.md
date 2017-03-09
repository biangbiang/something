nodejs学习笔记
==============

### 执行退出

    process.kill()

### How can I get the full object in Node.js's console.log(), rather than '[Object]'?

##### Q
When debugging using console.log(), how can I get the full object?

    const myObject = {
       "a":"a",
       "b":{
          "c":"c",
          "d":{
             "e":"e",
             "f":{
                "g":"g",
                "h":{
                   "i":"i"
                }
             }
          }
       }
    };    
    console.log(myObject);

Outputs:

    { a: 'a', b: { c: 'c', d: { e: 'e', f: [Object] } } }

But I want to also see the content of property f.

##### A1

You need to use util.inspect():

    const util = require('util')

    console.log(util.inspect(myObject, {showHidden: false, depth: null}))

    // alternative shortcut
    console.log(util.inspect(myObject, false, null))

Outputs

    { a: 'a',  b: { c: 'c', d: { e: 'e', f: { g: 'g', h: { i: 'i' } } } } }

See util.inspect() docs.

##### A2

You can use JSON.stringify, and get some nice indentation as well as perhaps easier to remember syntax.

    console.log(JSON.stringify(myObject, null, 4));
    {
        "a": "a",
        "b": {
            "c": "c",
            "d": {
                "e": "e",
                "f": {
                    "g": "g",
                    "h": {
                        "i": "i"
                    }
                }
            }
        }
    }

The third argument sets the indentation level, so you can adjust that as desired.

More detail here if needed:

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify
