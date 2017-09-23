### let、const

- #### 暂时性死区

在代码块内，使用 let、const  命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

- #### 块级作用域与函数声明

ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数。

ES6浏览器有自己的行为方式：

1. 允许在块级作用域内声明函数。
2. 函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
3. 同时，函数声明还会提升到所在的块级作用域的头部。


### String

includes(), startsWith(), endsWith()


### Object

- 属性的简洁表示法

ES6 允许在对象之中，直接写变量。这时，属性名为变量名, 属性值为变量的值。

- Object.is()

```javascript
//ES5：
+0 === -0 //true
NaN === NaN // false
//ES6
Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

- 属性的遍历


for...in | Object.keys | Object.getOwnPropertyNames | Object.getOwnPropertySymbols | Reflect.ownKeys
---|---|---|---|---
自身的和继承的可枚举属性 | 自身的可枚举属性 | 自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性） | 自身的所有 Symbol 属性 | 自身的所有属性



### Array

Array.from()、Array.of()、find()、findIndex()、includes()


### function

默认参数、rest参数、尾调用优化

### Prmise

解疑答惑


```javascript
new Promise((resolve, reject) => {
  console.log('promise test start.')
  resolve('resolve')
  reject('reject')
}).then(res => {
  console.log('resolve callback,', res)
}).catch(e => {
  console.log('catch,', e)
})
// promise test start.
// resolve callback, resolve
------------------------------------

new Promise((resolve, reject) => {
  console.log('promise test start.')
  throw 'reject'
  resolve('resolve')
}).then(res => {
  console.log('resolve callback,', res)
}).catch(e => {
  console.log('catch,', e)
})
// promise test start.
// catch, reject

//1. 
throw e

//2.
try {
  throw e
} catch(e) {
  reject(e);
}

//3.
reject(e)

上面三种写法是等价的
------------------------------------

new Promise((resolve, reject) => {
  console.log('promise test start.')
  resolve('resolve')
  reject('reject')
}).then(res => {
  console.log('resolve callback,', res)
}).catch(e => {
  console.log('catch,', e)
})
// promise test start.
// resolve callback, resolve
------------------------------------

new Promise((resolve, reject) => {
  console.log('promise test start.')
  resolve('resolve')
}).then(res => {
  console.log('resolve callback,', res)
  throw 'resolve callback error.'
}).catch(e => {
  console.log('catch,', e)
})
// promise test start.
// resolve callback, resolve
// catch, resolve callback error.
------------------------------------

new Promise((resolve, reject) => {
  console.log('promise test start.')
  resolve('resolve')
}).then(res => {
  console.log('resolve callback,', res)
  //return 'test';
  //throw 'resolve callback error.'
}).then(res => {
  console.log('2 resolve callback,', res)
}).catch(e => {
  console.log('catch,', e)
})
// promise test start.
// resolve callback, resolve
// 2 resolve callback, undefined
------------------------------------

new Promise((resolve, reject) => {
  console.log('promise test start.')
  throw 'reject'
  resolve('resolve')
}).then(res => {
  console.log('resolve callback,', res)
  throw 'resolve callback error.'
}, e => {
  console.log('catch reject callback,', e)
  throw 'reject error'
}).then(res => {
  console.log('2 resolve callback,', res)
  throw '2 resolve callback error.'
}).catch(e => {
  console.log('catch,', e)
})
// promise test start.
// catch reject callback, reject
// catch, reject error
------------------------------------
        
```

总结：
- 无论 then 的回调函数返回什么值，then 函数总是会返回一个新的 Promise 对象。
- catch 会捕获 then 调用链里的 error（包括 resolve、 reject 回调函数里的 error ）。

[promisejs 源码](https://www.promisejs.org/implementing/#then)
```javascript
function MyPromise (fn) {
  var PENDING = 0
  var FULFILLED = 1
  var REJECTED = 2

  // store state which can be PENDING, FULFILLED or REJECTED
  var state = PENDING

  // store value once FULFILLED or REJECTED
  var value = null

  // store sucess & failure handlers
  var handlers = []

  var name = Math.random().toString(36).substr(2)
  this._name = name

  this.done = function (onFulfilled, onRejected) {
    // ensure we are always asynchronous
    setTimeout(function () {
      handle({
        onFulfilled: onFulfilled,
        onRejected: onRejected
      })
    }, 0)
    console.log(name, 'done', value)
  }

  this.then = function (onFulfilled, onRejected) {
    var self = this
    console.log(self._name, 'then')
    return new MyPromise(function (resolve, reject) {
      return self.done(function (result) {
        if (typeof onFulfilled === 'function') {
          try {
            return resolve(onFulfilled(result))
          } catch (ex) {
            return reject(ex)
          }
        } else {
          return resolve(result)
        }
      }, function (error) {
        if (typeof onRejected === 'function') {
          try {
            return resolve(onRejected(error))
          } catch (ex) {
            return reject(ex)
          }
        } else {
          return reject(error)
        }
      })
    })
  }

  function getThen (value) {
    var t = typeof value
    if (value && (t === 'object' || t === 'function')) {
      var then = value.then
      if (typeof then === 'function') {
        return then
      }
    }
    return null
  }

  function fulfill (result) {
    state = FULFILLED
    value = result
    handlers.forEach(handle)
    handlers = null
    console.log(name, 'fulfill', result)
  }

  function reject (error) {
    state = REJECTED
    value = error
    handlers.forEach(handle)
    handlers = null
    console.log(name, 'reject', error)
  }

  function resolve (result) {
    try {
      var then = getThen(result)
      console.log(name, 'resolve getThen', result, then)
      if (then) {
        console.log(name, 'doResolve again')
        doResolve(then.bind(result), resolve, reject)
        return
      }
      fulfill(result)
    } catch (e) {
      reject(e)
    }
  }

  function handle (handler) {
    if (state === PENDING) {
      console.log(name, 'handle', state)
      handlers.push(handler)
    } else {
      if (state === FULFILLED && typeof handler.onFulfilled === 'function') {
        console.log(name, 'handle onFulfilled', state)
        handler.onFulfilled(value)
      }
      if (state === REJECTED && typeof handler.onRejected === 'function') {
        console.log(name, 'handle onRejected', state)
        handler.onRejected(value)
      }
    }
  }

  function doResolve (fn, onFulfilled, onRejected) {
    var done = false
    try {
      console.log(name, 'doResolve', value, done)
      fn(function (value) {
        console.log(name, 'doResolve onFulfilled', value, done)
        if (done) return
        done = true
        console.log(name, 'doResolve onFulfilled', value)
        onFulfilled(value)
      }, function (reason) {
        console.log(name, 'doResolve onRejected', reason, done)
        if (done) return
        done = true
        console.log(name, 'doResolve onRejected', reason)
        onRejected(reason)
      })
    } catch (ex) {
      if (done) return
      done = true
      onRejected(ex)
    }
  }

  doResolve(fn, resolve, reject)
}
```

### Generator

- Generator 的 throw 方法被捕获以后，会附带执行下一条yield表达式。也就是说，会附带执行一次next方法。
- Generator 函数体外抛出的错误，可以在函数体内捕获；反过来，Generator 函数体内抛出的错误，也可以被函数体外的 catch 捕获。
- Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。如果此后还调用next方法，将返回一个value属性等于undefined、done属性等于true的对象，即 JavaScript 引擎认为这个 Generator 已经运行结束了。
- 如果 Generator 函数内部有try...finally代码块，那么return方法会推迟到finally代码块执行完再执行。
- Thunkify、co

### async

await命令后面的 Promise 对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到。

如果async函数内部没有捕获错误，只要任何一个await语句后面的 Promise 变为reject，那么整个async函数都会中断执行。

### class

__proto__并不是语言本身的特性，这是各大厂商具体实现时添加的私有属性，虽然目前很多现代浏览器的JS引擎中都提供了这个私有属性，但依旧不建议在生产中使用该属性，避免对环境产生依赖。生产环境中，我们可以使用 Object.getPrototypeOf 方法来获取实例对象的原型，然后再来为原型添加方法/属性。

ES6 明确规定，Class 内部只有静态方法，没有静态属性。