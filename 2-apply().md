```js
/*
Replace the method named `m` of the object `o` with a version
that logs messages before and after invoking the original method.
*/
function trace(o, m) {
  let original = o[m];
  o[m] = function(...args) {
    console.log(new Date(), "Entering...:", m);
    let result = original.apply(this, args);
    console.log(new Date(), "Exiting:", m);
    return result;
  }
}

trace(o, m);
o.m();
```

Source: JavaScript: Definitive Guide

Functions in JavaScript are a specialized kind of JS objects. They have properties like `length`, 
`name`, `prototype` and methods like `call()`, `apply()`, `bind()`, and `toString()`.

For `apply()`, the first argument is the object on which the function is to be invoked; this argument 
is the invocation context and becomes the value of the `this` keyword within the body of the function.

The second argument to `apply()` is an array of arguments. 
