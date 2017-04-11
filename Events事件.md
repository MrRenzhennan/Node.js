# Events事件
大多数 Node.js 核心 API 都是采用惯用的异步事件驱动架构，其中某些类型的对象（称为触发器）会周期性地触发命名事件来调用函数对象（监听器）。  
  
例如，一个 net.Server 对象会在每次有新连接时触发一个事件；一个 fs.ReadStream 会在文件被打开时触发一个事件；一个 stream 会在数据可读时触发事件。  
  
所有能触发事件的对象都是 EventEmitter 类的实例。这些对象开放了一个 eventEmitter.on() 函数，允许将一个或多个函数附加到会被对象触发的命名事件上。   
事件名称通常是`驼峰式`的字符串，但也可以使用任何有效的 JavaScript 属性名。
    
Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。  
列如： fs.readFile 和 http.createServer 的回 调函数都是通过 EventEmitter 来实现的。下面我们用一个简单的例子说明 EventEmitter 的用法:
```js
var EventEmitter = require('events').EventEmitter;
var event = new EventEmitter();

event.on('some_event', function(){
    console.log('some event occured.');
});

setTimeout(function(){
    event.emit('some_event');
}, 1000);
```
## 给监听器传入参数与`this`
`eventEmitter.emit() `方法允许将任意参数传给监听器函数。   
当一个普通的监听器函数被 EventEmitter 调用时，标准的 `this` 关键词会被设置指向监听器所附加的 EventEmitter。
```js
//给监听器传入参数与this
const EventEmitter = require("events");

class MyEmitter extends EventEmitter {};
const myEmitter = new MyEmitter
myEmitter.on("event", function(a, b) {
    console.log(a, b);
    console.log(this);
    //标准的this关键词会被设置指向监听器所附加的 EventEmitter
});
//触发事件时，向回调函数传入参数
myEmitter.emit("event", "a", "b");
/*打印结果
a b
MyEmitter {
  domain: null,
  _events: { event: [Function] },
  _eventsCount: 1,
  _maxListeners: undefined }
*/
```
也可以使用 ES6 的箭头函数作为监听器。但是这样 this 关键词就不再指向 EventEmitter 实例：
```js
const EventEmitter = require("events");
class MyEmitter1 extends EventEmitter {};
const myEmitter1 = new MyEmitter1;
myEmitter1.on("event1", (a, b) => {
    console.log("ES6语法", a, b);
    console.log("ES6语法:", this);
});
myEmitter1.emit("event1", "a", "b")
/*打印结果
ES6语法 a b
ES6语法: {}
*/
```
## eventEmitter.on()
## eventEmitter.emit()
## setImmediate()与process.nextTick()
EventListener 会按照监听器注册的顺序同步地调用所有监听器。   
所以需要确保事件的正确排序且避免竞争条件或逻辑错误。   
监听器函数可以使用 setImmediate() 或 process.nextTick() 方法切换到异步操作模式：
```js
const EventEmitter = require("events");

class MyEmitter extends EventEmitter {};

const myEmitter = new MyEmitter;
myEmitter.on("event", (a, b) => {
    setImmediate(() => {
        console.log("异步操作", a, b);
        console.log("异步操作", this);
    });
    process.nextTick(() => {
        console.log("异步", a, b);
        console.log("异步", this);
    });
    console.log("同步操作", a, b);
    console.log("同步操作", this);
});
myEmitter.emit("event", "a", "b")
/*打印结果
同步操作 a b
同步操作 {}
异步 a b
异步 {}
异步操作 a b
异步操作 {}
*/
```
## eventEmitter.once()
使用 eventEmitter.once() 方法时可以注册一个对于特定事件最多被调用一次的监听器。 当事件被触发时，监听器会被注销，然后再调用。
```js
const EventEmitter = require("events");
class MyEmitter extends EventEmitter {};
const myEmitter = new MyEmitter;
myEmitter.once("event", function(a) {
    console.log(a);
});
myEmitter.emit("event", "a"); //会触发事件
myEmitter.emit("event", "a"); //上面事件已经被触发了，这里不会在被触发
myEmitter.emit("event", "a");
myEmitter.emit("event", "a");
myEmitter.emit("event", "a");
myEmitter.emit("event", "a");
myEmitter.emit("event", "a");
/*
a
*/
```

## 事件错误`error`
## removeListener()
## removeAllListeners()
## setMaxListeners()
## listeners()
##
