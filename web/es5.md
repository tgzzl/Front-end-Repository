### 简史
- ECMA (European Computer Manufacturers Association，欧洲计算机制造商协会) 
- ISO/IEC (International Organization for Standardization and International Electrotechnical Commission，国标标准化组织和国际电工委员会)
- JavaScript 组成结构
```
graph LR
JavaScript-->ECMAScript
JavaScript-->DOM文档对象模型
JavaScript-->BOM浏览器对象模型
```

### DOM 级别

- DOM0
  
  未形成标准的试验性质的初级阶段的DOM，称为dom0，并不是标准。

- DOM1
  
  主要定义了HTML和XML文档的底层结构。

- DOM2
  
  原来DOM的基础上又扩充了鼠标、视图、事件、范围和遍历等细分模块，而且通过对象接口增加了对CSS的支持。

- DOM3
  
  DOM加载和保存模块、DOM验证模块、DOM核心的扩展


### 正则表达式
![image](https://img.w3cschool.cn/attachments/image/20160809/1470710362109508.gif)

### test()
返回一个布尔值，表示当前模式是否能匹配参数字符串。如果正则模式是一个空字符串，则匹配所有字符串。

带g修饰符时：每一次开始搜索的位置都是上一次匹配的后一个位置。可以通过正则对象的lastIndex属性指定开始搜索的位置。

### exec()
匹配，就返回一个数组，成员是每一个匹配成功的子字符串，否则返回null。

如果正则表示式包含圆括号（即含有“组匹配”），返回一个数组。第一个成员是整个匹配的结果，第二个成员是圆括号匹配的结果。

带g修饰符时：正则对象的lastIndex属性不仅可读，还可写。一旦手动设置了lastIndex的值，就会从指定位置开始匹配。

### 组匹配
使用组匹配时，不宜同时使用g修饰符，否则match方法不会捕获分组的内容。在正则表达式内部，可以用\n引用括号匹配的内容，n是从1开始的自然数，表示对应顺序的括号。

```JavaScript
/(.)b(.)\1b\2/.test("abcabc")  // true
// 括号还可以嵌套
/y((..)\2)\1/.test('yabababab') // true
// 匹配网页标签
/<([^>]+)>[^<]*<\/\1>/
```

### 非捕获组 (?:x)
(?:x)称为非捕获组（Non-capturing group），表示不返回该组匹配的内容，即匹配的结果中不计入这个括号。

```JavaScript
'abc'.match(/(?:.)b(.)/); // ["abc", "c"]
```

### 先行断言 x(?=y)
x(?=y)称为先行断言（Positive look-ahead），x只有在y前面才匹配，y不会被计入返回结果。

```JavaScript
'abc'.match(/(?:.)b(.)/); // ["abc", "c"]
```

### 先行否定断言 x(?!y)
x(?!y)称为先行否定断言（Negative look-ahead），x只有不在y前面才匹配，y不会被计入返回结果。

```JavaScript
'abd'.match(/b(?!c)/); //['b']
```

### String.prototype.match()
匹配成功返回一个数组，匹配失败返回null。设置正则表达式的lastIndex属性，对match方法无效，匹配总是从字符串的第一个字符开始。

带g修饰符时：则该方法与正则对象的exec方法行为不同，会一次性返回所有匹配成功的结果。

### String.prototype.search()
返回第一个满足条件的匹配结果在整个字符串中的位置。如果没有任何匹配，则返回-1。该方法会忽略g修饰符。

### String.prototype.replace()
replace方法的第二个参数可以使用美元符号$，用来指代所替换的内容。

replace方法的第二个参数还可以是一个函数，将每一个匹配内容替换为函数返回值。

- $& 指代匹配的子字符串。
- $` 指代匹配结果前面的文本。
- $' 指代匹配结果后面的文本。
- $n 指代匹配成功的第n组内容，n是从1开始的自然数。
- $$ 指代美元符号$。


### 基本概念
- 数值转换

    Number()可以用于任何数据类型；parseInt()和 parseFloat()，则专门用于把字符串转换成数值。
    
    不指定基数意味着让 parseInt()决定如何解析输入的字符串，因此为了避免错误的解析，建议无论在什么情况下都明确指定基数。

- 全等和不全等
    
    全等操作符由 3 个等于号(===)表示，它只在两个操作数未经转换就相等的情况下返回 true。除了在比较之前不转换操作数之外，全等和不全等操作符与相等和不相等操作符没有什么区别。

- 数组操作
    
    slice：返回起始和结束位置之间的项— —但不包括结束位置的项。不会影响原始数组。
    
    splice：删除、插入、替换数组。返回一个数组，该数组中包含从原始数组中删除的项。

- 字符串操作
    
    slice：返回起始和结束位置之间的项— —但不包括结束位置的项。
    
    substring：返回起始和结束位置之间的项— —但不包括结束位置的项。
        
    substr：返回起始位置开始的指定长度的项。

- 其他
    
    encodeURI()不会对本身属于 URI 的特殊字符进行编码，例如冒号、正斜杠、 问号和井字号;而 encodeURIComponent()则会对它发现的任何非标准字符进行编码。
    

### 函数

- this 总是返回一个对象，简单说，就是返回属性或方法“当前”所在的对象。
- 函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。
- 函数的length属性与实际传入的参数个数无关，只反映函数预期传入的参数个数。

#### 1. this

按照优先顺序来总结一下从函数调用的调用点来判定 this 的规则了。按照这个顺序来问问题，然后在第一个规则适用的地方停下。

1. 函数是通过 new 被调用的吗（==new 绑定==）？如果是，this 就是新构建的对象。==var bar = new foo()==

2. 函数是通过 call 或 apply 被调用（==明确绑定==），甚至是隐藏在 bind 硬绑定 之中吗？如果是，this 就是那个被明确指定的对象。==var bar = foo.call( obj2 )==

3. 函数是通过环境对象（也称为拥有者或容器对象）被调用的吗（==隐含绑定==）？如果是，this 就是那个环境对象。==var bar = obj1.foo()==

4. 否则，使用默认的 this（==默认绑定==）。如果在 strict mode 下，就是 undefined，否则是 global 对象。==var bar = foo()==

以上，就是理解对于普通的函数调用来说的 this 绑定规则 所需的全部。


#### 2. 作用域


#### 3. arguments
arguments 很像数组，但它是一个对象。数组专有的方法（比如slice和forEach），不能在arguments对象上直接使用。

非严格模式下，arguments 对象为其内部属性以及函数形式参数创建 getter 和 setter 方法。

因此，改变形参的值会影响到 arguments 对象的值，反之亦然。

```javascript
function foo(a, b, c) {
    arguments[0] = 2;
    a; // 2

    b = 4;
    arguments[1]; // 4

    var d = c;
    d = 9;
    c; // 3
}
foo(1, 2, 3);
```
在严格模式下， getters 和 setters 不会被创建。

```
function f(a) {
  "use strict";
  a = 42;
  return [a, arguments[0]];
}
var pair = f(17);
assert(pair[0] === 42);
assert(pair[1] === 17);
```

#### 4. ==三剑客：call、applay、bind==

- call(thisObj[, arg1[, arg2[, ...]]])
  
    方法的参数，应该是一个对象。如果参数为空、null和undefined，则默认传入全局对象。
  
    如果call方法的参数是一个原始值，那么这个原始值会自动转成对应的包装对象，然后传入call方法。

- apply(thisObj[, arg1[, arg2[, ...]]])

  方法的作用与call方法类似，也是改变this指向，然后再调用该函数。唯一的区别就是，它接收一个数组作为函数执行时的参数。
  
- bind([thisObj[,arg1[, arg2[, [,.argN]]]]])

  如果方法的第一个参数是null或undefined，等于将this绑定到全局对象，函数运行时this指向顶层对象（在浏览器中为window）。
  
  arg1 … argN可传可不传。如果不传，可以在调用的时候再传。如果传了，调用的时候则可以不传，调用的时候如果你还是传了，则不生效

  call和 apply直接执行函数，而 bind需要再一次调用。

### 闭包
- 闭包的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在。
- 闭包的另一个用处，是封装对象的私有属性和私有方法。


### 原型 & 原型链

![image](https://dn-mhke0kuv.qbox.me/fed5ccbaec8b54c133a3.png?imageView2/1/w/1304/h/734/q/85/interlace/1)

- prototype 是函数(function) 的一个属性, 它指向函数的原型.
- __proto__ 是对象的内部属性, 它指向构造器的原型, 对象依赖它进行原型链查询，instanceof 也是依赖它来判断是否继承关系.

prototype 只有函数才有, 其他(非函数)对象不具有该属性. 而 __proto__ 是对象的内部属性, 任何对象都拥有该属性.

### 创建对象

- 工厂模式
    
    没有解决对象识别的问题

- 构造函数模式
    
    每个方法都要在每个实例上重新创建一遍 

- 原型模式
    
    每个函数都有一个 prototype(原型)属性。原型中所有属性是被很多实例共享的。
    
- 组合使用构造函数模式和原型模式

    构造函数模式用于定义实 例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本， 但同时又共享着对方法的引用，最大限度地节省了内存。另外，这种混成模式还支持向构造函数传递参 数;可谓是集两种模式之长。
 
- 动态原型模式
    
    通过 检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

### 继承
- 原型链继承

![image](http://tgzzl.oss-cn-shenzhen.aliyuncs.com/WX20170909-120519@2x.png)

缺点：1、子对象实例共享原型引用类型属性。2、在创建子类型的实例时，不能向超类型的构造函数中传递参数。

```javascript
function SuperType(){
  this.colors = ["red", "blue", "green"];
}
function SubType(){
}
//继承了 SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green,black"
```

- 借用构造函数

缺点：无法复用函数

```javascript
function SuperType(){
  this.colors = ["red", "blue", "green"];
}
function SubType(){
  //继承了 SuperType
  SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);    //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors);    //"red,blue,green"
```

- 组合继承（伪经典继承）

将原型链和借用构造函数的技术组合到一块。最常用的继承模式。

缺点：会调用两次超类型构造函数

```javascript
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
}
function SubType(name, age){
  //继承属性
  SuperType.call(this, name);
  this.age = age;
}
//继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType; 
SubType.prototype.sayAge = function(){
  alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors);//"red,blue,green,black"
instance1.sayName();//"Nicholas";
instance1.sayAge();//29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors);//"red,blue,green"
instance2.sayName();//"Greg";
instance2.sayAge();//27
```
- 寄生式继承

```javascript
function createAnother(original){ 
  var clone = object(original); //通过调用函数创建一个新对象
  //以某种方式来增强这个对象
  clone.sayHi = function(){
    alert("hi");
  };
  return clone;//返回这个对象
}
```
- 寄生组合式继承

集寄生式继承和组合继承的优点与一身，是实现基于类型继承的最有效方式。

```javascript
function inheritPrototype(subType, superType){
  var prototype = Object.create(superType.prototype);//创建对象
  prototype.constructor = subType;//增强对象
  subType.prototype = prototype//指定对象
}

function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
};
function SubType(name, age){
  SuperType.call(this, name);
  this.age = age;
}

inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function(){
  alert(this.age);
}
```

### 浏览器 & 渲染引擎
不同的浏览器有不同的渲染引擎。

- Firefox：Gecko引擎
- Safari：WebKit引擎
- Chrome：Blink引擎
- IE: Trident引擎
- Edge: EdgeHTML引擎

渲染引擎处理网页，通常分成四个阶段。

- 解析代码：HTML代码解析为DOM，CSS代码解析为CSSOM（CSS Object Model）
- 对象合成：将DOM和CSSOM合成一棵渲染树（render tree）
- 布局：计算出渲染树的布局（layout）
- 绘制：将渲染树绘制到屏幕

以上四步并非严格按顺序执行，往往第一步还没完成，第二步和第三步就已经开始了。所以，会看到这种情况：网页的HTML代码还没下载完，但浏览器已经显示出内容了。

早期，浏览器内部对JavaScript的处理过程如下：

- 读取代码，进行词法分析（Lexical analysis），将代码分解成词元（token）。
- 对词元进行语法分析（parsing），将代码整理成“语法树”（syntax tree）。
- 使用“翻译器”（translator），将代码转为字节码（bytecode）。
- 使用“字节码解释器”（bytecode interpreter），将字节码转为机器码。

为了提高运行速度，现代浏览器改为采用“即时编译”（Just In Time compiler，缩写JIT），即字节码只在运行时编译，用到哪一行就编译哪一行，并且把编译结果缓存（inline cache）。

### 同源政策

目前，如果非同源，共有三种行为受到限制。

- Cookie、LocalStorage 和 IndexedDB 无法读取。
- DOM 无法获得。
- AJAX 请求无效（可以发送，但浏览器会拒绝接受响应）。

 Cookie 和 iframe 窗口可以通过设置 document.domain 来规避同源政策。

### 前端跨域的几种方法
- JSONP
  
  script 标签是不受同源策略的限制的，它可以载入任意地方的 JavaScript 文件，而并不要求同源。

- document.domain（适用于frame框架）

  http://wenku.baidu.com/ 和 http://zhidao.baidu.com/ 的相同点
  1.   协议、端口、二极域名都一样，com为一级域名，baidu.com为二级域名
  2.   设置 document.domain = 'baidu.com';

- window.name（适用于frame框架）

  在一个页面里打开另一个不同源的页面，window.name的值会保持不变。
  
  与 document.domain 方法相比，放宽了域名后缀要相同的限制，可以从任意页面获取 string 类型的数据。

- [HTML5] postMessage
  
  在 HTML5 中， window 对象增加了一个非常有用的方法：
```
windowObj.postMessage(message, targetOrigin);
windowObj: 接受消息的 Window 对象。
message: 在最新的浏览器中可以是对象。
targetOrigin: 目标的源，* 表示任意。
```
  这个方法非常强大，无视协议，端口，域名的不同。
- location.hash
  
  location.hash会直接暴露在URL里，并且在一些浏览器里会产生历史记录，数据安全性不高也影响用户体验。

  有些浏览器不支持onhashchange事件，需要轮询来获知URL的变化。


### 服务端跨域的几种方法
- 反向代理服务器
- CORS，Cross-Origin Resource Sharing 是 W3C 推出的一种跨站资源获取的机制，不兼容 IE6、7
- WebSocket，是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。 


### 杂项
- date格式化：yyyy-mm-dd

toISOString() => "2017-09-15T04:29:53.967Z", date.toISOString().split('T')[0]
- var声明的全局变量是不可配置的
- 如果一个对象表达式不需要传入参数给构造函数的话，圆括号可以省略，比如 new Date
- 


 












