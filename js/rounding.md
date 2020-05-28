## Use `Intl.NumberFormat.prototype.format` instead of `Number.prototype.toFixed`
Rounding to some digital point is hard, and not so much related to javascript as it is to [floating point notation](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html).

Nevertheless, it looks like [`Intl.NumberFormat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat) does better job at rounding numbers in javascript than [`.toFixed`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)

```js
const formatter = [0, 1, 2, 3, 4, 5].map(
    (decimals) =>
        new Intl.NumberFormat('en-US', {
            minimumFractionDigits: decimals,
            maximumFractionDigits: decimals,
        }),
);

console.log(formatter[2].format(17.525)); // 17.53
console.log(formatter[2].format(5.525)); // 5.53
console.log(formatter[2].format(1.005)); // 1.01
console.log(formatter[2].format(8.635)); // 8.64
console.log(formatter[2].format(8.575)); // 8.58
console.log(formatter[2].format(35.855)); // 35.86
console.log(formatter[2].format(859.385)); // 589.39
console.log(formatter[2].format(859.3844)); // 589.38
console.log(formatter[2].format(.004)); // 0.00
console.log(formatter[2].format(0.0000001)); // 0.00

// keep in mind that this will not be formatted as expected, as the value that
// you pass is actually 0.07499999999998863. 
console.log(formatter[2].format(239.575 - 239.5)); // 0.07
console.log(formatter[2].format(0.07499999999998863)); // 0.07
```

Other option is to use `toLocaleString`.
```js
1.005.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2}) // 1.01
```
