---
title: JS继承方式
date: 2018-03-29 12:16
tags: web前端, js
categories: notes
---
原型链和继承基本上面试都会问到，总结记录一下

##### 构造函数、原型和实例之间的关系

每个构造函数都有个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

##### 原型继承

总的来说就一句话，父类的实例赋值给子类的prototype。比如：

```js
    function SuperType () { // 父类
       this.name = ['zhangsan', 'lisi']
    }

    function SubType () { // 子类
      this.age = 12
    }

    SubType.prototype = new SuperType() // 将父类的实例赋值给子类的prototype

    const sub = new SubType() // 实例化子类
    sub.name // ['zhangsan', 'lisi']
    sub.age // 12
```

  之前一直带着对prototype的畏惧，继承都是靠记忆，后来想想，其实只要记住构造函数、原型和实例之间的关系就好：父类的实例包含指向父类原型的指针，将父类的示例赋值给子类的原型，子类的原型自然就包含指向父类的指针，因此也就实现了继承。

顺带提一句，所有函数的默认原型都是Object的实例，因此默认原型都会包含一个内部指针，指向Object.prototype。

原型链虽然强大，但也存在一些问题，即包含引用类型值的问题。原型上的属性会被所有实例共享，如果父类有引用类型的属性，就会出现这种问题：
```js
// 继续上一个例子，假如SubType使用原型继承了SuperType

const sub1 = new SubType() // 实例1
sub1.name.push('wangwu') // 给name添加一个元素，因为name是引用类型，因此所有实例共享同一个引用
sub1.name // ['zhangsan', 'lisi', 'wangwu']

const sub2 = new SubType() // 实例2
sub2.name // ['zhangsan', 'lisi', 'wangwu']，实例2与实例1共享了同一个name
```
总的来说就是：原型链方式可以实现所有属性方法共享，但无法做到属性、方法独享

因此出现了构造函数继承方式

##### 经典继承（借用构造函数）
基本思想是在子类型构造函数的内部调用超类型构造函数。比如：
```js
    function SuperType () { // 父类
       this.name = ['zhangsan', 'lisi']
    }

    function SubType () { // 子类
      this.age = 12
      SuperType.call(this) // 通过call()方法执行父类构造函数实现继承
    }


    const sub1 = new SubType() // 实例1
    sub1.name.push('wangwu') // 给name添加一个元素
    sub1.name // ['zhangsan', 'lisi', 'wangwu']

    const sub2 = new SubType() // 实例2
    sub2.name // ['zhangsan', 'lisi']，实例2的nema没有被改变

```

每当新创建一个SubType实例时，都会执行SuperType.call(this)，此时的this是当前实例的上下文，因此，SuperType的属性就会被初始化到SubType的实例上，这就实现了继承。

借用构造函数除能独享属性、方法外还能在子类构造函数中传递参数，但代码无法复用。总的来说就是能实现属性、方法独享，但无法做到属性、方法共享

因此出现了一种继承方式，将两者结合起来，名叫组合式继承

##### 组合式继承

之前说的两种继承各自有优缺点，组合起来自是要取其有点，即需要独享的属性方法就用构造函数继承，需要共享的就用原型链继承。例如：

```js
    function SuperType (name) { // 父类
       this.name = name
       this.colors = ['red', 'blue', 'green']
    }

    SuperType.prototype.sayName = function() {
        console.log(this.name)
    }

    function SubType (name, age) { // 子类
      SuperType.call(this, name) // 继承属性
      this.age = age
    }

    SubType.prototype = new SuperType() // 继承方法

    SubType.prototype.sayAge = function () {
      console.log(this.age)
    }

    const sub1 = new SubType('Nicholas', 29)
    sub1.colors.push('black')
    sub1.colors // ['red', 'blue', 'green', 'black']
    sub1.sayName() // 'Nicholas'
    sub1.sayAge() // 29

    const sub2 = new SubType('Greg', 27)
    sub2.name // ['red', 'blue', 'green']
    sub2.sayName() // 'Greg'
    sub2.sayAge() // 27
```
这是继承时最常用的继承方式。

其他的后续跟上





