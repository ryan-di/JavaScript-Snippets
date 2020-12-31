```js
function addPrivateProperty(o, name, predicate) {
  let value;
   o[`get${name}`] = function() { return value; };
   o[`set${name}`] = function(v) {
    if (predicate && predicate(v)) {
      throw new TypeError(`set{name}: invalid value ${v}`);
    } else {
      value = v;
    }
  }
}

let o = {};
addPrivateProperty(o, "Name", x => typeof x === "string");
o.setName("Frank");       // set the property value
o.getName();              // => "Frank"
o.setName(0);             // TypeError
```

Source: JavaScript: Definitive Guide, Chapter 8

Remark:
1. property value is only manipulated by the getter and setter methods and is not stored in the object `o`
2. the property is only stored as a local variable in this `addPrivateProperty` function
3. the getter and setter are defined locally in the same function, which means they have access to this local variable
4. together, the value is private to the two accessor methods, but it cannot be modified except through the setter method, which enforces `predicate`.

Related Concepts:
1. JavaScript uses lexical scoping: functions are executed using the variable scope that was in effect when they were defined, not the variable scope in effect when they are invoked
2. In order to implement lexical scoping, the internal state of JavaScript function object must include not only the code of the function but also a reference to the scope in which the function definition appears
3. **The combination of a function object and a scope (a set of variable bindings) in which the function's variables are resolved is called a *closure*.**
