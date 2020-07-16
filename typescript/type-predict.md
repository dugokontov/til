## Type predicate functions

### Why?
You can use [type predicate functions](https://www.typescriptlang.org/docs/handbook/advanced-types.html#using-type-predicates) to filter out `null` and/or `undefined` values and have typescript guess correct type.

### How?
For example if you have some `map` function that could return `undefined` for some values, and you wanted to return only not `undefined` values, code would look like this.

```ts
const map: { [key: string]: number | undefined } = {
    a: 1,
    b: 2,
    // c is missing
    d: 3,
};
const results = ['a', 'b', 'c', 'd']
    .map((keys) => map[keys])
    .filter((value) => value !== undefined);
```

Even though we have a filter function that explicitly filters out `undefined` values, typescript still thinks that resulting type for results is `(number | undefined)[]`.

To cope with this we can replace arrow function passed to the filter with the function whose return type is "type predicate". This function has to return boolean, even though we use `functionParameter as type` syntax to specify return type.

```ts
const map: { [key: string]: number | undefined } = {
    a: 1,
    b: 2,
    // c is missing
    d: 3,
};
const isNumber = (value: number | undefined): value is number =>
    value !== undefined;
const results = ['a', 'b', 'c', 'd']
    .map((keys) => map[keys])
    .filter(isNumber);
```
Now typescript thinks `results` is of the type `number[]`;

### How does this work?

When we use this "type predicate" function in `if` case, it is easy to understand how typescript can predicate type.

```ts
let value: number | undefined;
if (!isNumber(value)) {
    // typescript assumes in this block that value is undefined
}
```

But, how does this work with filter? It works because typescript has two definitions for filter method on array object:

```ts
interface Array<T> {
    // other array methods
    filter<S extends T>(callbackfn: (value: T, index: number, array: T[]) => value is S, thisArg?: any): S[];
    filter(callbackfn: (value: T, index: number, array: T[]) => unknown, thisArg?: any): T[];
}
```

First one expects "type predicate function", and instead of `T[]`, returns `S[]` type.
