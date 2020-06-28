# JS
#### js数据类型
##### 基本数据类型(传值):
字符转(String), 数字(Number), 布尔(Boolean), 空(Null), 未定义(Undefined), Symbol
##### 引用数据类型(传址):
对象(Object), 数组(Array), 函数(Function)

#### 如何判断数据类型
##### typeof
```
console.log(
	typeof 123, //"number"
	typeof 'dsfsf', //"string"
	typeof false, //"boolean"
	typeof function(){console.log('aaa');}, //"function"
	typeof undefined, //"undefined"
	
	typeof [1,2,3], //"object"
	typeof {a:1,b:2,c:3}, //"object"
	typeof null, //"object"
	typeof new Date(), //"object"
	typeof /^[a-zA-Z]{5,20}$/, //"object"
	typeof new Error() //"object"
);
```
无法判断 Array, Object, Null, Date, RegExp, Error
##### instanceof
判断此构造函数的原型是否在给定对象的原型链上
```
console.log(
	123 instanceof Number, //false
	'asdasd' instanceof String, //false
	false instanceof Boolean, //false
	undefined instanceof Object, //false
	null instanceof Object, //false

	[1,2,3] instanceof Array, //true
	{a:1,b:2,c:3} instanceof Object, //true
	function(){console.log('aaa');} instanceof Function, //true
	new Date() instanceof Date, //true
	/^[a-zA-Z]$/ instanceof RegExp, //true
	new Error() instanceof Error //true
)
```
无法判断 Number, String, Boolean, Null, Undefined
按照一下写法可判断 Number, String, Boolean
```
var num = new Number(123);
var str = new String('asd');
var boo = new Boolean(false);
console.log(
	num instanceof Number, //true
	str instanceof String, //true
	boo instanceof Boolean //true
)
```
##### constructor
是 prototype 对象上的属性, 指向构造函数, null 和 undefined 没有 constructor 属性
```
console.log(
	123.constructor == Number, //报错
	Number(123).constructor == Number, //true
	'asdasd'.constructor == String, //true
	false.constructor == Boolean, //true
	[1,2,3].constructor == Array, //true
	{a:1,b:2,c:3}.constructor == Object, //true
	function(){console.log('aaa');}.constructor == Function, //true
	new Date().constructor == Date, //true
	/^[a-zA-Z]$/.constructor == RegExp, //true
	new Error().constructor == Error //true
)
```
##### toString()
需要以 Function.prototype.call() 或 Function.prototype.apply() 的形式来调用
```
var tS = Object.prototype.toString;

tS.call(123); //"[object Number]"
tS.call('asd'); //"[object String]"
tS.call(true); //"[object Boolean]"
tS.call([1, 2]); //"[object Array]"
tS.call({name:'a', age:111}); //"[object Object]"
tS.call(function(){ console.log('123'); }); //"[object Function]"
tS.call(undefined); //"[object Undefined]"
tS.call(null); //"[object Null]"
tS.call(new Date()); //"[object Date]"
tS.call(/^[a-zA-Z]$/); //"[object RegExp]"
tS.call(new Error()); //"[object Error]"
```
##### 封装一个判断数据类型的准确方法
```
function getType(obj) {
	return Object.prototype.toString.call(obj).replace(/^\[object (\w+)\]$/, '$1');
}
```
#### 深拷贝与浅拷贝
假设 B 复制了 A, 当修改 B 时, A 是否跟着变化, 若变化, 则为浅拷贝, 若互相独立不随着另一方变化, 则为深拷贝
1. 对于基本数据类型, name 与 value 都存在栈内存中, 拷贝时会新开辟一个内存空间, 所以互相独立, 不过这并不算时深拷贝, 因为深拷贝只针对较为复杂的引用数据类型
```
var a = 1;
var b = a;
b = 2;
console.log(a); //1
```
2. 对于引用数据类型, name 存在栈内存中, value 存在堆内存中, 栈内存中存的是一个指向对内存中值的地址, 所以进行一般的 b=a 的拷贝时, 只是将 a 在栈内存中存储的地址给了 b, 指向同一个堆内存中的值, 改变两者中的一个, 另一个也会受影响, 所以为**浅拷贝**, 拷贝时, 若可以在堆内存中开辟一块新的内存来存储b的值, 那么 a 与 b 就不会互相影响, 也就完成了**深拷贝**;
##### 实现浅拷贝
1. for...in 循环拷贝第一层
```
function simpleClone(obj) {
	let objClone = Array.isArray(obj) ? [] : {};
	for (let key in obj) {
		objClone[key] = obj[key];
	}
	return objClone;
}

let obj1 = {
	a: 1,
	b: 2,
	c: {
		c1: 3
	}
}

let obj2 = simpleClone(obj1);
obj2.a = 8;
obj2.c.c1 = 9;
console.log(
	obj1.a, // 1
	obj1.c.c1, // 9
	obj2.a, // 8
	obj2.c.c1 // 9
)
```
2. Object.assign() 实现一层拷贝
```
let obj1 = {
	a: 1,
	b: {
		b1: 2
	}
}

let obj2 = Object.assign({}, obj1);
obj2.a = 8;
obj2.b.b1 = 9;
console.log(
	obj1.a, // 1
	obj1.b.b1, // 9
	obj2.a, // 8
	obj2.b.b1 // 9
)
```
##### 实现深拷贝
1. 递归方法拷贝所有层级
```
function deepClone(obj) {
	let objClone = Array.isArray(obj) ? [] : {};

	if(obj && typeof obj === 'object') {
		for(let key in obj) {
			if(obj[key] && typeof obj[key] === 'object') {
				objClone[key] = deepClone(obj[key]);
			} else {
				objClone[key] = obj[key];
			}
		}
	}

	return objClone;
}
```
2. 通过 json 对象实现深拷贝
```
function deepClone(obj) {
	const _obj = JSON.stringify(obj);
	return JSON.parse(_obj);
}
```

* 如果obj里面有时间对象，则JSON.stringify后再JSON.parse的结果，时间将只是字符串的形式。而不是时间对象；
* 如果obj里有RegExp、Error对象，则序列化的结果将只得到空对象；
* 如果obj里有函数，undefined，则序列化的结果会把函数或 undefined丢失；
* 如果obj里有NaN、Infinity和-Infinity，则序列化的结果会变成null
* JSON.stringify()只能序列化对象的可枚举的自有属性，例如 如果obj中的对象是有构造函数生成的， 则使用
* JSON.parse(JSON.stringify(obj))深拷贝后，会丢弃对象的constructor
* 如果对象中存在循环引用的情况也无法正确实现深拷贝；

3. jQuery 深拷贝

```
let array = [1,2,3,4];
let newArray = $.extend(true,[],array); // true为深拷贝，false为浅拷贝
```