Yes — in JavaScript, all numbers are floating-point numbers by default.
There is only one number type in JavaScript: the 64-bit double-precision floating-point number (IEEE 754 standard).

```js
console.log(0.1 + 0.2); // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3); // false ★ very famous gotcha

console.log(9999999999999999); // 10000000000000000   ← lost precision
console.log(9007199254740992 + 1); // 9007199254740992   ← can't count up anymore

console.log(1 + 1 === 2); // true   (safe up to 2⁵³)
console.log(9007199254740993); // 9007199254740992   ← rounded
```
