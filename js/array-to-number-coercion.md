## Implicit array to number casting (coercion)
In some places javascript does implicit conversion to number. For example, it does that for every parameter passed to `Math.max` or `Math.min` methods. (example of v8 engine doing this in version [4.3.21](https://github.com/v8/v8/blob/4.3.21/src/math.js#L89))

This is what I found out how some array values are casted to number in javascript.

```js
+[]; // 0
+[1]; // 1
+[Infinity]; // Infinity
+[null]; // 0
+[undefined]; // 0
+[1, 2]; // NaN
```

### Why?
Array is an object. [ToNumber](http://es5.github.io/#x9.3) operation defines that object has [ToPrimitive](http://es5.github.io/#x9.1) called upon. This operation calls `[[DefaultValue]]` with hint `Number`. And `[[DefaultValue]]` method with hint `Number` calls `valueOf` or `toString`, whichever returns primitive value.

Steps to convert Array to Number would be:

 1. Call `ToNumber(input argument)` [[1](http://es5.github.io/#x9.3)]
 1. Call `ToPrimitive(input argument, hint Number)`, since argument type is `Object` [[2](http://es5.github.io/#x9.1)]
 1. Call `[[DefaultValue]](hint Number)` on `input argument`, since `input argument`'s type is `Object` [[3](http://es5.github.io/#x8.12.8)]
 1. Call `input argument.toString`, sice hint is `Number` and `input argument.valueOf` doesn't return primitive type
 1. Call `ToNumber(value of the last method)`
 
To understand behavior in full, we need to know that [`Array.prototype.toString`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toString) behaves same as `Array.prototype.join`.
So, here is what happens with all above examples:

```js
+[]; // [].toString() -> "" -> 0
+[1]; // [1].toString() -> "1" -> 1
+[Infinity]; // [Infinity].toString() -> "Infinity" -> Infinity
+[null]; // [null].toString() -> "" -> 0
+[undefined]; // [undefined].toString() -> "" -> 0
+[1, 2]; // [1, 2].toString -> "1,2" -> NaN
```
