>1.**对象**适合于收集和管理数据，容易形成树型结构。
Javascript包括一个原型链特性，允许对象继承另一对象的属性。正确的使用它能减少对象的初始化时间和内存消耗。

>2.**函数**它们是javascript的基础模块单元，用于代码复用、信息隐藏和组合调用。函数用于指定**对象的行为**。
一般来说，编程就是将一组需求分解成一组函数和数据结构的技能。

>3.**模块**我们可以使用函数和闭包来构造模块。模块是一个提供接口却隐藏实现状态和实现的函数或对象。

  

1.自定义类型--构造函数模式(伪类模式)
---------------
>在基于类的系统中，对象是这样定义的：使用类来描述它是什么样的。假如建筑是基于类的系统，则建筑师会先画出房子的蓝图，然后房子都按照该蓝图来建造。

在使用自定义类型模式实现继承的时候,我们只需要将参数传递给构造函数，然后将参数挂载在实例对象上。其他关于实例对象的方法都不用传递参数，因为通过 **实例对象**调用的方法内部的this都可以访问到该参数。**挂载在实例this对象上的变量称为实例变量。**

###组合--继承
```
function Person (name, age, job) {
    // 实例变量
    this.name = name;
    this.age = age;
    this.job = job;
}
Person.prototype.sayName = function () {
    alert(this.name);
}

var person1 = new Person('Nicholas', 29, 'Software Engineer');
var person2 = new Person('Greg', 27, 'Doctor');
```

```
function SuperType (name) {
    this.name = name;
    this.colors = ['red','blue', 'green'];
}

SuperType.prototype.sayName = function () {
    console.log(this.name);
}

function SubType (name, age) {
    // 继承属性
    SuperType.call(this,name);
    this.age = age;
}

// 继承方法
SubType.prototype = new SuperType();

SubType.prototype.sayAge = function () {
    console.log(this.age)
}

var instance1 = new SubType('Nicholas', 29);
instance1.colors.push('black')
console.log(instance1.colors);
instance1.sayName();
instance1.sayAge();

var instance2 = new SubType('Greg', 27)
console.log(instance2.colors);
instance2.sayName();
instance2.sayAge();
```
在继承属性和继承方法上，我们一共调用了两次超类构造函数，当通过new调用超类构造函数创建子类构造函数的原型时，有一个问题，子类构造函数的原型对象现在便是超类构造函数的实例，因此也会有在超类构造函数为实例对象this添加的属性，只是值为undefined而已，也就是说通过new调用超类构造器函数来更改子类改造器的原型时，那么在子类构造器的原型上便会有多余的属性。这便造成了浪费。而我们需要的其实只是，子类构造器的原型能够继承超类构造器原型的方法而已。因此我们需要的，1.创建一个子类构造器原型对象。2.此子类构造器原型继承自超类构造器的原型。3.因为我们在1中改写了子类构造器的原型对象，也就是重新创建了原型对象，因此我们需要在新创建的原型对象上添加constructor属性并将其赋值为子类构造器函数。
将上面的代码改写一些，如下所示。

**关于constructor属性:**只在构造器函数的原型上才有的属性并指向该构造器，改写了的原型对象默认是没有constructor属性的。

###寄生组合式--继承
```
function inheritPrototype (subType,superType) {
    var prototype = Object.creat(superType.prototype);
    prototype.constructor = subType;
    subType.prototype = prototype;
};

function SuperType (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}
SuperType.prototype.sayName = function () {
    console.log(this.name);
}
function SubType(name, age) {
    //继承属性
    SuperType.call(this,name);
    this.age = age;
}
//继承方法
inheritPrototype(SubType,SuperType);
SubType.prototype.sayAge = function () {
    console.log(this.age);
}

var instance = new SubType();
```

通过隐藏那些所谓的prototype操作细节，现在看起来没那么怪异了。但是否真的有所发现:
**没有私有环境，所有属性都是公开的。无法访问父类的方法。难以调试**

2.原型
----------------
>在一个纯粹的原型模式中，我们会摈弃类，转而专注对象。基于原型的继承相比基于类的继承在概念上更简单:一个新对象可以继承一个旧对象的属性。你通过构造有用的对象开始，接着可以构造更多和那个对象类似的对象。这就可以完全避免把一个应用拆解成一系列嵌套抽象类的分类过程
在基于原型的系统中,我们创建的对象，看起来要像我们想要的所有这种类型的对象那样，然后告诉javascript引擎，我们想要更多像这样的对象。如果建筑是基于原型的，建筑师会先建一所房子，然后将房子都建成像这种模样的。

方法`Object.creat()`作为`new`操作符的替代方案，使用它来创建javascript对象时，能增添一种更像是基于原型的感觉。

```
function myMammal = {
    name : 'Herb the Mammal',
    get_name : function () {
        return this.name;
    },
    says : function () {
        return this.saying || '';
    }
}

var myCat = Object.create(myMammal);
myCat.name = 'Henrietta';
myCat.saying = 'meow';
myCat.purr = function (n) {
    var i, s = '';
    for (i = 0;i < n; i += 1) {
        if(s) {
            s += '-'
        }
        s += 'r';
    }
    return s;
}

myCat.get_name = function () {
    return this.says + ' ' + this.name + this.says;
}

```

这是一种"差异化继承"。通过定制一个新的对象，我们指明它与所基于的基本对象的区别。
有时候，它对某些数据结构继承于其他数据结构的情形非常有用。
 


3.函数化--工厂模式
--------------
>在伪类模式里，构造器函数Cat不得不重复构造器Mammal已经完成的工作。在函数化模式中那不再需要了，因为构造器Cat将会调用构造器Mammal，让Mammal去做对象创建中的大部分工作，所有Cat只关注自身的差异即可。
函数化模式有很大的灵活性。它相比伪类模式不仅带来的工作更少，还让我们得到更好的封装和信息隐藏，以及访问父类方法的能力。
如果我们用函数化得样式去创建对象，并且该对象的所有方法都不用this或that,那么该对象就是持久性的。一个持久性的对象就是一个简单功能函数的集合。

**私有变量:任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数外部访问这些变量。**

**闭包**
>闭包是阻止垃圾回收器将变量从内存中移除的方法，使的在创建变量的执行环境的外面能够访问到该变量。
请记住:闭包由函数创建。每次调用函数会创建一个唯一的执行环境对象。函数执行完后，执行对象就会被丢弃，除非调用者引用了它。当然，如果函数返回的是数字，就不能引用函数的执行环境对象。但是如果函数返回的是一个更复杂的结构，像是**函数、对象或者数组**，将返回值保存到一个变量上，就创建了一个对执行环境的引用。

```
Function.prototype.method = function (name,func) {
    this.prototype[name] = func;
    return this; 
}
// 工厂mammal函数
var mammal = function (spec) {
    var that = {};

    that.get_name = function () {
        return spec.name;
    }
    that.says = function (spec) {
        return spec.saying || '';
    } 

    return that;
}

// 工厂cat函数(基于mammal的函数)
var cat = function (spec) {
    spec.saying = spec.saying || 'meow';
    var that = mammal(spec);
    that.purr = function (n) {
        var i, s = '';
        for (i = 0; i < n; i += 1) {
            if(s) {
                s += '-';
            }
            s += 'r';
        }
    }
    that.get_name = function () {
        return that.says() + ' ' + spec.name + ' ' + that.says();
    }
    return that;
}

// 创建myCat对象
var myCat = cat({name: 'Henrietta'});

Object.method('superior',function (name) {
    var that = this,
        method = that[name];
    return function () {
        return method.apply(that, arguments)
    }
})

// 工厂coolcat函数(基于cat函数)
var coolcat = function (spec) {
    var that = cat(spec),
        super_get_name = that.superior('get_name');
    that.get_name = function (n) {
        return 'like ' + super_get_name() + ' baby';
    }
    return that;
}

var myCoolCat = coolcat({name : 'Bix'});

var name = myCoolCat.get_name();

```

函数化模块模式有很大的灵活性。它相比构造函数模式不仅带来的工作更少，还让我们得到更好的封装休息和隐藏，以及访问父类方法的能力。如果对象的所有状态都是私有的，那么该对象就成为一个"防伪(tamper-proof)"对象。该对象的属性是可以被替换或者删除，当该对象的完整性不会受到损坏。我们用函数式的样式创建一个对象，并且该对象的所有方法都不使用this或者that，那么该对象就是持久性对象。一个持久性对象，就是一个简单的函数功能的集合。
一个持久性的对象不会被入侵。访问一个持久性的对象时，除非有方法授权，否则攻击者不会访问对象的内部状态。

###模块模式

前面的模式是用于 **自定义类型**创建私有变量和特权方法的。而道格拉斯所说的模块模式则是为 **单例**创建私有变量和特权方法。所谓单例指的就是只有一个实例的对象。(就是用对象字面量表示法创建的对象)

```
var singleton = function () {
    // 私有变量和函数
    var privateVariable = 10;

    function privateFunction () {
        return false;
    }
    //特权/公有方法和属性
    return {
        publicProvperty: true;

        publicMethod: function () {
            privateVariable++;
            return privateFunction();
        }
    }
}
```

从本质上讲，这个对象字面量定义的是单例的公共接口。**这种模式在需要对单例进行某些初始化，同时又需要维护其私有变量时非常有用**。简言之，如果必须创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有数据的方法。

###增强的模块模式
>这种增强的模块模式适合那些单例必须是某种类型的实例，同时还必须添加某些属性和方法对其加以增强的例子。

```
var singleton = function () {
    // 私有变量和函数
    var privateVariable = 10;

    function privateFunction () {
        return false
    }
    // 创建对象
    var object = new CustomType();

    // 添加特权/公有属性和方法
    object.publicProperty = true;
    object.publicMethod = function () {
        privateVariable++;
        return privateFunction();
    }

    return object;
}()
```











