## How to annotate object getter

Object literal getter in javascript is a method that can be accessed as a property. It is defined like this:

```js
const numberWrapper = {
    elements: [1, 3, 5],
    get hasEvenNumber() {
        return this.elements.some((number) => number % 2 === 0);
    },
};

console.log(numberWrapper.hasEvenNumber); // outputs false
```

I was puzzled as to how to define type for object `numberWrapper` in typescript. I couldn't find any reference to getter on object literals in typescript. With the help of this [stackoverflow answer](https://stackoverflow.com/a/43390660/521373), I figured out how to do it:

```ts
interface NumberWrapper {
    elements: number[];
    readonly hasEvenNumber: boolean;
}
```

This doesn't force property `hasEvenNumber` to be getter, only that it can't be reassigned.

```ts
const otherObj: NumberWrapper = {
    elements: [],
    hasEvenNumber: false,
}; // this is OK

otherObj.elements.push(2);
otherObj.hasEvenNumber = true; // TS error: Cannot assign to 'hasEvenNumber' because it is a read-only property.
```
