# git的使用
1、宏任务和微任务(https://www.cnblogs.com/wangziye/p/9566454.html)：

宏任务：script(整体代码)、setTimeout、setInterval、I/O、UI交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)

微任务：Promise.then、Object.observe、MutationObserver、process.nextTick(Node.js 环境)

2、原型和原型链

![](https://www.showdoc.com.cn/server/api/attachment/visitFile?sign=42d11cfa21a4a45b114f7cc19cc3bedd)

#include  <stdio.h>`
function Person () {};
var p = new Person();
==========================
a.每个构造函数都有一个prototype属性。 Person.prototype

b.每个对象（null除外）都具有一个__proto__的属性。该属性---->原型 。 p.__proto__ === Person.prototype;

c.每个原型都有constructor构造函数，原型的构造函数指向自己   Person.prototype.constructor === Person

d.Person.prototype.__proto__ === Object.prototype

*一般情况下，将公共属性定义到构造函数内，公共方法放到原型（prototype）上。

3、new在执行中做的4件事：

①在内存中创建一个新的空对象

②将this指向该对象

③执行构造函数中的代码。给这个新对象添加属性和方法

④返回这个新对象（所以构造函数中不需要return）