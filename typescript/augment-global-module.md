## Augment global module

Similarly as one can augment any module (eg: [redux](./redux-augment-store-property.md)), it is possible to augment global module.

### Why?

I defined an interface to be a collection, where keys are numbers, like this:

```ts
interface ValueById {
    [key: number]: number;
}
```

But when I tried to call `Object.values` on an object of this type I got unexpected return type.

```ts
const valuesById: ValueById = {};
// ...
Object.values(valuesById); // type is any[]
```

When I looked into definition, I found out that `ObjectConstructor` in `es2017.object.d.ts` only has generic definition for a case where key is string, and then defaults to any.

```ts
// es2017.object.d.ts
interface ObjectConstructor {
    values<T>(o: { [s: string]: T } | ArrayLike<T>): T[];
    values(o: {}): any[];
}
```

There is a [discussion on typescript's github](https://github.com/Microsoft/TypeScript/issues/26010) about why is this. [Root cause](https://github.com/microsoft/TypeScript/issues/21089) is that `Object.values` on enum doesn't work because of this.

But, since I don't use `Object.values` on enums, I decided to update global definition.

### How?

```ts
// in types.ts
declare global {
    interface ObjectConstructor {
        values<T>(o: { [s: number]: T }): T[];
    }
}
```
