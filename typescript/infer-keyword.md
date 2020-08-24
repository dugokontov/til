## infer keyword
[`infer`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#type-inference-in-conditional-types) is a keyword in typescript introduced to help with generics. It's main purpose is to introduce another abstract/generic type that will be used in current definition. Without it compiler can't know if that type should represent type that is already created, or some internal type what will not float out of definition.

### How to use it?
```ts
// lets define type FlattenIfArray that uses generic type T.
// FlattenIfArray that will check if provided type T is an array, and if
// so, it will return type of the array's element. Otherwise it will return 
// type T
type FlattenIfArray<T> = T extends (infer U)[] ? U : T; // U is internal abstract type used only inside this definition

type Type1 = FlattenIfArray<string[]>; // Type1 = string
type Type2 = FlattenIfArray<Promise<string>>; // Type2 = Promise<string>
```

Without infer, typescript will display error, telling that type U is not defined:

```ts
type FlattenIfArray<T> = T extends U[] ? U : T; // Cannot find name 'U'.(2304)
```

Just to put it in perspective, you sometimes want to use existing types, and not internal abstract types. For example:

```ts
type FunctionPropertyNames<T> = {
    // here we want typescript to check if Function type exist and report error
    // if we made typo
    [K in keyof T]: T[K] extends Function ? K : never;
}[keyof T];
```
