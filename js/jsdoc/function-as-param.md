## Define function received as params

```js
/** @param {(name: string) => string[] } callback */
const action = (callback) => {
    // ...
    const ary = callback();
    ary.map();
};
```
