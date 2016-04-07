### 使用对象
一个对象就是一系列属性的集合,一个属性包含一个名字和一个值.一个属性的值可以是函数,这种情况下属性也被称为方法.

### 枚举一个对象的所有属性
从 ECMAScript5 开始,有三种原生的方法用于列出或枚举对象的属性 :
+ `for...in`循环 可访问一个对象及其原型链中所有可枚举的属性
+ `Object.keys(o)` 翻译一个数组,返回对象 o 包含的所有属性的名称.
+ `Object.getOwnPropertyNames(o)` 返回一个数组,包含对象 o 所有拥有的属性.

获取一个对象的所有属性
```
function listAllProperties(o){     
	var objectToInspect;     
	var result = [];
	for(objectToInspect = o; objectToInspect !== null; objectToInspect = Object.getPrototypeOf(objectToInspect)){  
		result = result.concat(Object.getOwnPropertyNames(objectToInspect));  
	}
	return result;
}
```

### 创建新对象
#### 使用对象初始化器
```
var myHonda = {color: "red", wheels: 4, engine: {cylinders: 4, size: 2.2}};
```

#### 使用构造函数
作为另一种方式,通过两步来创建对象:
1. 创建一个构造函数.首字母大写是普遍的作坊.
2. 通过 new 创建对象实例.
```
// 创建 构造函数
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
// 创建 mycar 对象:
var mycar = new Car("Eagle", "Talon TSi", 1993);
```

#### 使用Object.create方法
[ Object.create method](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)


#### 继承
所有 JavaScript 对象继承至少一个对象.被继承的对象被称为原型,并且继承的属性可能通过构造函数的 prototype 对象找到.

#### 为对象类型定义属性
可以偶蹄类刚刚 prototype 属性为之前定义的对象类型增加属性.这为该类型的所有对象,而不是静静一个对象增加了一个属性.下面的代码为所有的类型为 car 的对象增加了 color 属性,然后为对象 car1 的 color 属性赋值:
```
Car.prototype.color = null;
car1.color = "black";
```

比较对象
在 JavaScript 中 objects 是一个引用类型.
```
// fruit object reference variable
var fruit = {name: "apple"};

// fruitbear object reference variable
var fruitbear = {name: "apple"};

fruit == fruitbear // return false

fruit === fruitbear // return false 这尤其要注意
```
