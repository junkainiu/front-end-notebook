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
有时候JS有些数据结构看上去很象数组, 但其实是只是`array`-like, 他们并不是数组, 也没有我们常用的一些数组方法, 例如(`push(...)`, `shift(...)`, `forEach(...)`), 基本上他们只有`length`可以用, 我们DOM操作返回的列表和函数内部的`arguments`变量都是这样的类数组, 如果想要使用数组的方法, 我们必须手动添加, 通常使用`Array.slice(...)`方法, 利用`slice`方法在不传参数的时候返回一个数组拷贝的原理来将类数组转换为数组, 例如:

```js
function toArray(){
    var arr = Array.prototype.slice.call(arguments);
    arr.push(3);
    console.log(arr);
};
toArray(1, 2); // [1, 2, 3]
```

### 字符串(String)
字符串和数组(`Array`)某些特性有些相似, 例如他们都有`length`属性, 都有`indexOf`, `concat`等方法。也都可以通过[index]来访问成员, 而他们最重要的区别是`String`是不可变的(immutable), `Array`是可变的(mutable), 大多数用于修改`String`内容的方法都会返回一个新的`String`, 而很多作用于`Array`的方法会直接修改这个数组自身， 例如:

```js
var a = 'leo';
var b = ['l', 'e', 'o'];
a[0] = 'l';
b[1] = 'e';
c = a.toUpperCase();
c = 'LEO';
a = 'leo';
b.push('g')
b = ['l', 'e', 'o', 'g'];
```

同样的, 我们可以通过一些hack手段来使得`String`获得一些`Array`上才有的方法

```js
a.join;	//undefined
a.map;	//undefiend
var c = Array.prototype.join.call(a, '-');
var d = Array.prototype.map.call(a, function(v){
	return v.toUpperCase() + '.'
	}).join('')
c;	//'l-e-e'
d;	//'L.E.O'
```

然而并不是所有的方法都可以通过这种手段获得, 记住`string`是不可变的, 所以他只能获得不改变数组的方法, 如果该方法需要改变数组本身, 那么`String`就无法获得, 例如`reverse`

```js
a.reverse;	//undefined
b.reverse();	//['g', 'o', 'e', 'l'] 
b;	//['g', 'o', 'e', 'l']
c = Array.prototype.reverse.call(a); //报错
```

碰到这种问题的时候我们一般使用另一种hack手段, 将他转换为`Array`再转换回`String`, 比较不优雅, 但是很多情况下很管用

```js
c = a.split('').reverse().join();
c;	//'oel'
```

###数字(numbers)
js中的number并没与java那样区分类型的写法, 例如int, double, float等, 他只有1种类型`number`。

####语法
小数点最后的零对于js没有意义

```js
var a = 12.0;
var b = 12.30;
a;	//12
b;	//12.3
```

js可以用`toFixed(..)`与`toPrecision`来控制数字精度, 注意他们返回的都是`string`

```js
var a = 23.45;
a.toFixed(0);	//'23'
a.toFixed(1);	//'23.5'
a.toFixed(2);	//'23.45'
a.toFixed(3);	//'23.450'
a.toFixed(4);	//'23.4500'

a.toPrecision(1);	//'2e+1'
a.toPrecision(2);	//'23'
a.toPrecision(3);	//'23.5'
a.toPrecision(4);	//'23.45'
a.toPrecision(5);	//'23.450'
a.toPrecision(6);	//'23.4500'
```

我们也可以不必将数字保存在变量中, 可以直接用数字字面量调用这些方法, 但是要小心其中的陷阱, 由于`.`可以被看作是数字的一部分所以`23.toFixed(1)`是会报语法错误的, 通常有以下几种变通的写法

```js
23..toFixed(3);		//23.000
(23).toFixed(3);	//23.000
23.45.toFixed(3);	//23.450
```
能够理解为什么这三种写法都对吗
