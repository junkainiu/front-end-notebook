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

function的类型是`function`

```js
typeof function a(){return 1} === 'function'; //true
```

数组的类型是`object`

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

typeof 自身返回`string`

```js
typeof typeof 100 === 'string'; //true
```

typeof 未赋值变量返回`undefined`

```js
var a;
typeof a; //"undefined"
```

## JS中的value与reference

### 数组(Array)
不同于Java 在JS中定义数组不需要事先定义长度 数组中的value可以是`number`, `string`, `object`, 甚至另一个`array`

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

#### 类数组
有时候JS有些数据结构看上去很象数组, 但其实是只是`array`-like, 他们并不是数组, 也没有我们常用的一些数组方法, 例如(`push(...)`, `shift(...)`, `forEach(...)`), 基本上他们只有`length(...)`可以用, 我们DOM操作返回的列表和函数内部的`arguments`变量都是这样的类数组, 如果想要使用数组的方法, 我们必须手动添加, 通常使用`Array.slice(...)`方法, 利用`slice`方法在不传参数的时候返回一个数组拷贝的原理来将类数组转换为数组, 例如:

```js
function toArray(){
    var arr = Array.prototype.slice.call(arguments);
    arr.push(3);
    console.log(arr);
};
toArray(1, 2); // [1, 2, 3]
```

### 字符串(String)
字符串和数组(`Array`)某些特性有些相似, 例如他们都有`length`属性, 都有`indexOf`, `concat`方法, 
