# JavaScript 笔记


## 概述

JavaScript可以实时更改HTML内容（标签 | 属性 | 属性值 | 样式）。
改变页面的外观或结构，甚至与数据库通信。

**示例：开关灯**

```html
<div id="lamp" style="width:100px; height:100px; border-radius:50%; background:silver; cursor:pointer"></div>
<script>
	lamp.onclick = turn
	function turn(){
		lamp.style.backgroundColor = lamp.style.backgroundColor == "silver"?"orange":"silver"
	}
</script>
```



小技巧：可以直接使用id来操作DOM（没有官方标准，但所有浏览器都支持）。

但要注意：如果id值被改过（例如: id+=5），类型改变。就不能再直接操作DOM了。
此时，需要用getElementById(id)来重新获取DOM对象。
可以自己封装一个安全的简写方法：

```js
function $(id){
	return typeof id === "string" ? document.getElementById(id) : id
}
```



附：获取DOM的几种方法：

```js
document.getElementById("id")				// [object HTMLXXXElement]
document.getElementsByClassName("blue")		// [object HTMLCollection]
document.getElementsByTagName("input")		// [object HTMLCollection]
document.getElementsByName("cat")			// [object NodeList]
document.querySelector("#.ele")				// [object HTMLXXXElement]
document.querySelectorAll("#.ele")			// [object NodeList]
```

---

## 数据类型

| 类型      | 说明   | 示例与说明               |
| --------- | ------ | ------------------------ |
| Boolean   | 布尔值 | true/false               |
| Number    | 数值   | 12; Nan/Infinity也是数值 |
| String    | 字符串 | "Welcome"                |
| Null      | 空引用 | null                     |
| Undefined | 未定义 | undefined                |
| Symbol    | 唯一值 | ES6引入                  |
| Array     | 数组   | [val1,val2,val3]         |
| Object    | 对象   | {key1:val1,key2:val2}    |
| Function  | 函数   | function fnName(){}      |


说明：

- 数组和对象可以嵌套，值可以是另一个数组或对象
- 字符串被视为特殊的数组，可以用数组的访问方法去访问，例如str.length
- 尽量使用null，undefined仅做判断函数传参有用

一些特殊值，对应的数据类型如下：

| 特殊值    | 数据类型  | 补充说明   |
| --------- | --------- | ---------- |
| ""        | String    |            |
| NaN       | Number    | 不是数的数 |
| Infinity  | Number    | 无穷大     |
| undefined | Undefined |            |
| null      | Object    |            |

```js
var i = 2/0;		// 值是infinity			类型是Number
var x = '';			// 值是‘’				类型是String
var y = null;		// 值是null				类型是Object
var z;				// 值是undefined		类型是Undefined

undefined和null的值相同，类型不同
null == undefined	// true
null === undefined	// false
NaN与任何元素都不同，甚至与自己都不同。唯一能判断NaN的方法是isNaN函数
NaN == NaN	// false
NaN === NaN	// false
isNaN(NaN)	// true
```



### 类型转换

**隐式转换：** 让系统自动将数据类型转换

- ++ -- > == < 都可以让字符串变为数字
- +号两边，只要有一个是字符串，结果为字符串类型；
- 其他的算数运算符，`- * / ` 结果为数字类型


技巧：

- +号作为正号解析，可以转换成数字型 【 +str 】
- 任何数据和字符串相加结果都是字符串

**显式转换：** 自定义转换数据类型

- `Number( )` ：转换为数字。如果转换内容有非数字，转换结果为NaN。等于与技巧 +
- `parseInt( )` ：只保留整数部分
- `parseFloat( )` ：只保留数字部分
- `String( )` ：转换为字符串。
- `toString( )` ：转换为字符串。



### 字符串

'XX' "XX" `XX`(ES6标准) 
可用 \ 转义来显示引号 'I\'m OK!'

#### 反引号
ES6新标准，支持多行文本，简化变量输出，便利。
`这是个
多行
字符串`


输出简化语法：

```js
// 原有写法
var msg = '你好, ' + name + ', 你今年' + age + '岁了!';
// 简化写法
var msg = `你好, ${name}, 你今年${age}岁了!`;
```

#### 字符串操作

字符串可以视为特殊的数组，能使用数组的方法来操作

- `str.length`	字符串的字符数量
- `str[ num ]`	获取某个位置的字符
  字符串是不可变的，对字符串的某个索引赋值，不会出错，但是也无效果

```js
var s = 'Hello, world!';
s.length; // 13

var s = 'Hello, world!';
s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined

var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```

- `str.toUpperCase()`		全转为大写字母（得到新字符串，不影响原有串）
- `str.toLowerCase()`		全转为小写字母（得到新字符串，不影响原有串）

```js
var s = 'Hello';
s.toUpperCase();	// 返回'HELLO'
s.toLowerCase();	// 返回'hello'
s;					// 仍是'Hello'
```

- `str.indexOf('xxx')`	搜索指定字符串出现的位置

```js
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```

- `str.substring(x,x)`	返回指定索引区间的子串

```js
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```





### 数组

数组(Array)是多个变量集合，可以包含任意数据类型，通过索引来访问数组元素。

用length可以取得数组的长度，如果给length赋新值会导致数组大小变化

```js
var arr = [1, 2, 3];
arr.length; 		// 3
arr.length = 6;		// arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;		// arr变为[1, 2]
```

对索引赋新值，会把对应的元素修改为新的值

```js
var arr = ['A', 'B', 'C'];
arr[1] = 99;	// arr现在变为['A', 99, 'C']
```

通过索引赋值时，索引超过了范围，同样会引起Array大小的变化：

```js
var arr = [1, 2, 3];
arr[5] = 'x';	// arr变为[1, 2, 3, undefined, undefined, 'x']
```

**不建议直接修改数组大小，访问索引时要确保不会越界。**



#### 数组的操作

- `arr.length`			数组的元素数量

```js
var arr = [1, 2, 3.14, 'Hello', null, true];
arr.length; 		// 6
```

- `arr.indexOf(30)`		搜索指定元素的位置

```js
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10);	// 元素10的索引为0
arr.indexOf(20);	// 元素20的索引为1
arr.indexOf(30);	// 元素30没有找到，返回-1
arr.indexOf('30');	// 元素'30'的索引为2
```

- `arr.slice()`			截取数组的部分元素，返回新的数组

```js
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```

​	如果不给slice()传递任何参数，它就会从头到尾截取所有元素。利用这一点，可以很容易地复制数组。

```js
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy == arr;	// false
aCopy === arr;	// false
```

- `arr.push(A,B)`		数组末尾添加元素
- `arr.pop()`				数组末尾删除元素

```js
var arr = [1, 2];
arr.push('A', 'B'); // 返回Array新的长度: 4
arr; // [1, 2, 'A', 'B']

arr.pop(); // pop()返回'B'
arr; // [1, 2, 'A']

arr.pop(); arr.pop(); arr.pop(); // 连续pop 3次
arr; // []
arr.pop(); // 空数组继续pop不会报错，而是返回undefined
arr; // []
```

- `arr.unshift(A,B)`	数组头部添加元素
- `arr.shift()`			数组头部删除元素

```js
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
arr.shift(); // 'A'
arr; // ['B', 1, 2]
arr.shift(); arr.shift(); arr.shift(); // 连续shift 3次
arr; // []
arr.shift(); // 空数组继续shift不会报错，而是返回undefined
arr; // []
```

- `arr.sort()`		给数组元素排序(无参数则表示默认排序)

```js
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

- `arr.reverse()`		反转数组

```js
var arr = [1, 2, 3];
arr.reverse(); 
arr; // [3, 2, 1]
```

- `arr.splice()`		可以从指定的索引开始删除N个元素，再从该位置处添加N个元素

```js
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

- `arr.concat()`	数组组合，不会改变原来的数组

```js
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]);
added; // ['A', 'B', 'C', 1, 2, 3]
arr; // ['A', 'B', 'C']

// 可以接收任意个元素或数组
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```

- `arr.join()`	把数组每个元素都用指定字符连接起来，然后返回连接后的字符串
  如果`Array`的元素不是字符串，将自动转换为字符串后再连接。

```js
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```



### 对象

对象是由 键:值 组成的无序集合。其中键是字符串类型，值可以是任意数据类型。
`var car = {type:"Fiat", model:"500", color:"white"}`

访问对象的值：对象名.键名 或 对象名['键名']
`car.type` or `car['type']`

访问对象方法：对象名.方法名()
`car.fullName()`

```js
var person = {
	firstName:"Li",
	lastName:"Si",
	age:20,
	sex:true,
	fullName: function() {return this.firstName + " " + this.lastName;}
}
```

如果对象名包含特殊字符，就用‘’包起来：`'middle-school':'No.1 Middle School'`

对象是动态类型，可以自由地添加或删除属性：

```js
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```

可以用 `in`来检查是否拥有某个属性：

```js
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
'name' in xiaoming; // true
'grade' in xiaoming; // false
```

由于存在属性继承的可能，用`in`判断不一定准确

```js
'toString' in xiaoming; // true
```

因为`toString`定义在`object`对象中，而所有对象最终都会在原型链上指向`object`
所以`xiaoming`也拥有`toString`属性。

要判断一个属性是否是`xiaoming`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：

```js
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```







---





### MAP

ES6新增的数据类型，[键,值] 组成的集合

- 基本结构是二维数组
- 与对象不同，键可以是数字或其他数据类型
- 特点：具有极快的查找速度

```js
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```

初始化`Map`需要一个二维数组，或者直接初始化一个空`Map`：
```js
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```



### SET

`Set`和`Map`类似，也是一组key的集合，但不存储value。
由于key不能重复，所以在`Set`中，没有重复的key。

要创建一个`Set`，需要提供一个`Array`作为输入，或者直接创建一个空`Set`：

```js
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3

// 重复元素在Set中自动被过滤：
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
```

`add(key)`：添加元素
`delete(key)`：删除元素

```js
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}

var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```





## 函数

```js
// 第一种定义方式
function name(x,y) {ruturn x*y}
// 第二种定义方式
var name = function (x,y){ruturn x*y};	// JavaScript的函数也是一个对象
// 两种定义完全等价，注意第二种方式按照完整语法需要在函数体末尾加一个;，表示赋值语句结束。
```

访问函数时,只用名称没有括号，将返回函数定义，而不是函数结果：

```js
function name(x,y) {return x*y}
name;		// ruturn function name(x,y) {return x*y}
name(6,7);	// ruturn 42
```

函数体内部的语句在执行时，一旦执行到`return`时，函数就执行完毕，并将结果返回。
如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`undefined`

Javascipt函数允许传入任意个参数而不影响调用
`arguments` ：只能函数中使用，通过 `arguments[i]` 得到所有参数，常用于判断传入参数的个数

```js
// foo(a[, b], c)
// 接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
    // ...
}
```

ES6新增了 `rest` 参数，传入的参数先绑定`a`、`b`，多余的参数以数组形式交给变量`rest`

```js
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```



### 作用域

**变量作用域：** 只在定义的函数内部有效（包括内层函数）
**变量提升：** JavaScript的函数定义，会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部。

**全局作用域：** 不在函数内定义的变量就具有全局作用域，全局作用域的变量实际上被绑定到`window`的一个属性
**名字空间：** 全局变量会绑定到`window`上，不同的JS文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。
减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中

```js
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function(){ return 'foo' }
```

**局部作用域：** 也叫块级作用域，仅在 {代码段} 有效
ES6新增`let`与const定义，可以定义块级作用域的变量和常量

```js
function foo() {
    for (var i=0; i<100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
}
```

```js
function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

**解构赋值：** ES6新特性

```js
// 之前的写法
var array = ['hello', 'JavaScript', 'ES6'];
var x = array[0];
var y = array[1];
var z = array[2];
// 解构赋值的写法
var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
// 如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致：
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
x; // 'hello'
y; // 'JavaScript'
z; // 'ES6'
// 解构赋值还可以忽略某些元素：
let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
z; // 'ES6'

// 解构赋值还可以用于对象，取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性：
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;
// name, age, passport分别被赋值为对应属性:

// 对嵌套的对象属性进行赋值，只要保证对应的层次是一致的
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
name; // '小明'
city; // 'Beijing'
zip; // undefined, 因为属性名是zipcode而不是zip
// 注意: address不是变量，而是为了让city和zip获得嵌套的address对象的属性:
address; // Uncaught ReferenceError: address is not defined
```

使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为`undefined。
如果要使用的变量名和属性名不一致，可以用下面的语法

```js
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};

// 把passport属性赋值给变量id:
let {name, passport:id} = person;
name; // '小明'
id; // 'G-12345678'
// 注意: passport不是变量，而是为了让变量id获得passport属性:
passport; // Uncaught ReferenceError: passport is not defined
```

解构赋值还可以使用默认值，这样就避免了不存在的属性返回`undefined`的问题：

```js
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678'
};

// 如果person对象没有single属性，默认赋值为true:
var {name, single=true} = person;
name; // '小明'
single; // true
```

有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：

```js
// 声明变量:
var x, y;
// 解构赋值:
{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
```

这是因为JavaScript引擎把`{`开头的语句当作了块处理，于是`=`不再合法。解决方法是用小括号括起来：

```js
({x, y} = { name: '小明', x: 100, y: 200});
```

解构的使用场景

解构赋值在很多时候可以大大简化代码。例如，交换两个变量`x`和`y`的值，可以这么写，不再需要临时变量：

```js
var x=1, y=2;
[x, y] = [y, x]
```

快速获取当前页面的域名和路径：

```js
var {hostname:domain, pathname:path} = location;
```

如果一个函数接收一个对象作为参数，那么，可以使用解构直接把对象的属性绑定到变量中。例如，下面的函数可以快速创建一个`Date`对象：

```js
function buildDate({year, month, day, hour=0, minute=0, second=0}) {
    return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
}
```



---

## 方法

### setInterval 定时器

语法：
`setInterval(code, milliseconds);`
`setInterval(function, milliseconds, param1, param2, ...)`
以指定的时间间隔（以毫秒为单位）调用函数或计算表达式
setInterval()方法会持续行为，直到调用clearInterval()或关闭窗口。
setInterval()返回的ID值，用作clearInterval()方法的参数。

> 调用fn()和调用fn有什么区别呢？
> 调用fn是将函数本身代码段传给setInterval，每隔100毫秒执行一次
> 调用fn()的话，函数直接就运行了，所以相当于把fn()运行后的值传给了setInterval
> 函数fn()又没有返回值，那setInterval每100毫秒执行一次“空”，所以啥都没干

## 注意：

* 将脚本放在<body>元素的底部可以提高显示速度，因为脚本编译会降低显示速度。



## 疑问：

* 同值的数字字串完全相等（=== 为true）；同值的数组对象完全不相等（==为false）

