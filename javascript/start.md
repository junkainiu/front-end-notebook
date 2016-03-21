# JS基础
## 类型

JS内置类型

* `null`
* `undefined`
* `boolean`
* `number`
* `string`
* `object`

typeof 可以判断JS数据类型

```js
typeof undefined     === "undefined"; // true
typeof true          === "boolean";   // true
typeof 42            === "number";    // true
typeof "42"          === "string";    // true
typeof { life: 42 }  === "object";    // true
```

null比较特殊

```js
typeof null     === "object"; //true
(!a && typeof a === "object"); // true 可以用来判断一个变量是否为null
```

function的类型是'function'

```js
typeof function a(){return 1} === 'function'; //true
```

数组的类型是object

```js
typeof [1,2,3] === 'object'; //true
```

typeof 也可以判断变量的类型

```js
var a = 20;
typeof a === 'number'; //true
a = true;
typeof a === 'boolean'; //true
```

typeof 自身返回string

```js
typeof typeof 100 === 'string'; //true
```

typeof 未赋值变量返回undefined

```js
var a;
typeof a; //"undefined"
```

## JS中的value与reference

### 数组(Array)
不同于Java 在JS中定义数组不需要事先定义长度 数组中的value可以是number, string, object, 甚至另一个array

```js
var a = [1, 'a', '2' ,[1, 2], {'a':1}, function a(){return 1}];
a.length; //6
a[0] === 1; //true
a[3][1] === 1; //true
var b = [];
b.length; //0
b[0] = 1;
b[1] = 'a';
b[2] = 'b';
b.length; // 3
```
