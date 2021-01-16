```js
function readOnlyProxy(o) {
  function readonly() {
    throw new TypeError("Read Only!");
  }
  
  return new Proxy(o, {
    set: readonly,
    defineProperty: readonly,
    deleteProperty: readonly,
    setPrototypeOf: readonly,
  });
}

let o = { x: 1, y: 2 };    // Normal writable object
let p = readOnlyProxy(o);  // Readonly version of it
p.x                        // => 1: reading properties works
p.x = 2;                   // !TypeError: can't change properties
delete p.y;                // !TypeError: can't delete properties
p.z = 3;                   // !TypeError: can't add properties
p.__proto__ = {};          // !TypeError: can't change the prototype
```

Source: 14.7 –– JavaScript: Definitive Guide

The `Proxy` class allows us to create proxies for objects that may or may not alter how fundamental 
operations on those objects. The `Proxy()` constructor takes two arguments, a `target` object and a `handlers` object.
For any given fundamental operation, like `get()` and `defineProperty()`, the proxy will first check to see if 
they are defined in the `handlers` object. If so, then the matching handler will be invoked. Otherwise, the 
operation will be delegated to the `target` object using the `Reflect` API. 

In the simple code snippet above, the author has created a `readOnlyProxy` that wraps the original object with the 
opeartions that alter the object forbidden and contained in the passed in handlers object.

As David, the author, has pointed out, we can use this proxy to make sure that objects passed to functions that are 
not supposed to modify the objects indeed leave the objects untouched.
