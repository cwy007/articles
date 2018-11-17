#### 源代码
```js
this.addProduct = function(amount, product) {
  products.push(...Array(amount).fill(product));
};
```
#### 解释
```js
Array(amount).fill(object)

Array(3)                    // => [null, null, null]
Array(3).fill({ a: 1})      // => [{ a: 1 }, { a: 1 }, { a: 1 }]
```


#### command line
```js
> amount = 3
3
> product = { name: 'p  name', price: 10}
{ name: 'p  name', price: 10 }
> a = Array(amount)
[ <3 empty items> ]
> amount
3
> a.fill(product)
[ { name: 'p  name', price: 10 },
  { name: 'p  name', price: 10 },
  { name: 'p  name', price: 10 } ]
> a
[ { name: 'p  name', price: 10 },
  { name: 'p  name', price: 10 },
  { name: 'p  name', price: 10 } ]

```
## 原文链接
https://medium.freecodecamp.org/an-introduction-to-object-oriented-programming-in-javascript-8900124e316a?fbclid=IwAR0KTJOgQOSpqTT6Nw2I_-IxPeJw3o-SIbnDCb9HhBAgGkNp9OIEfoydTmo
