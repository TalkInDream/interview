# git的使用
1、宏任务和微任务(https://www.cnblogs.com/wangziye/p/9566454.html)：

宏任务：script(整体代码)、setTimeout、setInterval、I/O、UI交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)

微任务：Promise.then、Object.observe、MutationObserver、process.nextTick(Node.js 环境)
==========================
2、原型和原型链

![](https://www.showdoc.com.cn/server/api/attachment/visitFile?sign=42d11cfa21a4a45b114f7cc19cc3bedd)

`
function Person () {};
var p = new Person();
`

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

3、继承
```
function Animal(name) {
    // 属性
    this.name = name || 'Animal';
    // 实例方法
    this.sleep = function () {
        console.log(this.name + '正在睡觉！');
    }
    //实例引用属性
    this.features = [];
}

```

①原型链继承: 
```
function Cat(name) {

}
Cat.prototype = new Animal();
```

|优点|缺点|
|:----                                                          |:-------    |
|**·** 继承关系简单：实例是子类的实例，也是父类的实例 ；</br>**·** 父类新增加的原型方法/属性，子类都能访问 ；</br> **·** 实现方法简单；      | **·**要想为子类新增属性和方法，比须在`new Animal()`之后，不能放到构造函数中；</br>**·**无法实现多继承；</br>**·** 来自原型对象的引用属性是素有实例共享的。如features,  name等不受影响</br>**·**创建子类实例的时候，无法像父类构造函数传参   | 

②构造继承
```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
```
|优点|缺点|
|:----                                                          |:-------    |
|**·** 解决了1中，子类实例共享父类引用属性的问题</br>**·** 创建子类实例时，可以向父类传递参数；</br> **·** 可以实现多继承（call多个父类对象）；      | **·**实例并不是父类的实例，只是子类的实例`console.log(cat instanceof Animal); // false`；</br>**·**只能继承父类的实例属性和方法，不能继承原型属性/方法；</br>**·** 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能</br>  | 

③实例继承：为父类实例添加新特性，作为子类返回
```
function Cat(name){
    var instance = new Animal();
    instance.name = name || 'Tom';
    return instance;
}

var cat = new Cat();
```
|优点|缺点|
|:----                                                          |:-------    |
|**·** 不限制调用方式，不管是new 子类()还是子类(),返回的对象具有相同的效果</br>| **·**实例是父类的实例，不是子类的实例；</br>**·**不支持多继承 |

④拷贝继承
```
function Cat(name){
    var animal = new Animal();
    for(var p in animal){
       Cat.prototype[p] = animal[p];
    }
    Cat.prototype.name = name || 'Tom';
}

var cat = new Cat();
```
|优点|缺点|
|:----                                                          |:-------    |
|**·** 支持多继承| **·**效率较低，内存占用高；</br>**·**无法获取不可枚举的方法属性 |

⑤组合继承：调用父类构造，继承父类属性并保留传参的优点，然后嗲用父类实例，实现函数复用
```
function Cat(name){
    Animal.call(this);
    this.name = name || 'Tom';
}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

var cat = new Cat();
```
|优点|缺点|
|:----                                                          |:-------    |
|**·** 弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法；<br/>**·**既是子类的实例，也是父类的实例；<br/>**·**不存在引用属性共享问题；<br/>**·**可传参；<br/>**·**函数可复用；| **·**调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）？？？？仅仅多消耗了一点内存； |


⑥寄生组合继承：通过寄生方式，砍掉父类的实例属性，在调用2次父类的构造的时候，就不会初始化2次实例属性/方法，避免组合继承的缺点
```
function Cat(name){
    Animal.call(this);
    this.name = name || 'Tom';
}
(function(){
    // 创建一个没有实例方法的类
    var Super = function(){};
    Super.prototype = Animal.prototype;
    //将实例作为子类的原型
    Cat.prototype = new Super();
    Cat.prototype.constructor = Cat;
})();

var cat = new Cat();
```
