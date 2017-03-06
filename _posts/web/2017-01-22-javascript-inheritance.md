---
layout: page
title: Javascript对象与继承（二）
subheadline: "面向对象(Object Oriented)程序设计之继承"
teaser: "本编主要总结记录Javascript继承机制，由于Javascript特有的原型链(Prototype Chain)模式和对象概念，与其他语言中的继承，类，实例等概念有较大不同，故做本篇加强理解。"
categories:
    - web
tags:
    - Javascript
---

### 原型链
Javascript中描述了原型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。而原型链的本质便是让原型对象等于另一个类型的实例，如此递进，就构成了实例与原型的链条。  

实现原型链有一种基本模式，代码大致如下。

{% highlight javascript %}
function SuperType(){
    this.property = true;
}
        
SuperType.prototype.getSuperValue = function(){
    return this.property;
};
        
function SubType(){
    this.subproperty = false;
}
        
//inherit from SuperType
SubType.prototype = new SuperType();
        
SubType.prototype.getSubValue = function (){
    return this.subproperty;
};
        
var instance = new SubType();
alert(instance.getSuperValue());   //true

alert(instance instanceof Object);      //true
alert(instance instanceof SuperType);   //true
alert(instance instanceof SubType);     //true

alert(Object.prototype.isPrototypeOf(instance));    //true
alert(SuperType.prototype.isPrototypeOf(instance)); //true
alert(SubType.prototype.isPrototypeOf(instance));   //true
{% endhighlight %}

以上代码定义了两个类型：SuperType 和 SubType。每个类型分别由一个属性和方法。它们的主要区别是 SubType 继承了 SuperType，而继承是通过创建 SuperType 的实例，并将该实例赋给 SuperType.prototype 实现的。实现的本质是重写原型对象，代之以一个新类型的实例。换句话说，原来存在于 SuperType 的实例中的所有属性和方法，现在存在于 SubType.prototype 中了。这个例子中的实例以及构造函数和原型之间的关系如下图所示。

![inheritance02](/images/inheritance02.png)

还有一点需要注意，即在通过原型链实现继承时，不能适用对象字面量创建原型方法。因为这样做就会重写原型链，如下面例子所示。

{% highlight javascript %}
//inherit from SuperType
SubType.prototype = new SuperType();
        
//try to add new methods – this nullifies the previous line
SubType.prototype = {
    getSubValue : function (){
        return this.subproperty;
    },
        
    someOtherMethod : function (){
        return false;
    }
};
        
var instance = new SubType();
alert(instance.getSuperValue());   //error!
{% endhighlight %}

以上代码展示了刚刚把 SuperType 的实例赋值给原型，紧接着又将原型替换成一个对象字面量而导致的问题。由于现在的原型包含的是一个 Object 的实例，而非 SuperType 的实例，因此我们设想中的原型链已经被切断————SubType 和 SuperType 之间已经没有关系了。

#### 原型链的问题
其中，最主要的问题来自包含引用类型的原型。包含引用类型的原型属性会被所有实例共享；而这也正是为什么要在构造函数中，而不是在原型对象中定义属性的原因。在通过原型来实现继承时，原型实际上会变成另一个类型的实例。于是，原先的实例属性也就顺理成章地成为了现在的原型属性了。下列代码可以用来说明这个问题。

{% highlight javascript %}
function SuperType(){
    this.colors = ["red", "blue", "green"];
}

function SubType(){            
}
        
//inherit from SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);    //"red,blue,green,black"
        
var instance2 = new SubType();
alert(instance2.colors);    //"red,blue,green,black"
{% endhighlight %}

原型链的第二个问题是：在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。有鉴于此，在加上由于原型包含引用类型值所带来的问题，实践中很少会单独使用原型链。

#### __借用构造函数__

这种技术的基本思想相当简单，即在子类型构造函数的内部调用超类型构造函数。如下所示：

{% highlight javascript %}
function SuperType(){
    this.colors = ["red", "blue", "green"];
}

function SubType(){  
    //inherit from SuperType
    SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);    //"red,blue,green,black"
        
var instance2 = new SubType();
alert(instance2.colors);    //"red,blue,green"
{% endhighlight %}

通过使用 call() 方法（或 apply() 方法），我们实际上是在新创建的 SubType 实例的环境下调用了 SuperType 构造函数。这样一来，就会在新 SubType 对象上执行 SuperType() 函数中定义的所有对象初始化代码。结果，SubType 的每个实例就都会具有自己的 colors 属性的副本了。

#### 借用构造函数的问题

如果仅仅是借用构造函数，那么也将无法避免构造函数模式存在的问题————方法都在构造函数中定义，因此函数复用就无从谈起了。而且，在超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式。考虑到这些问题，借用构造函数也很少单独使用。

#### __组合继承__  

组合继承(Combination Inheritance)，有时候也叫做伪经典继承，指的是将原型链和借用构造函数的技术组合到一块，从而发挥二者之长的一种继承模式。如下面的例子。

{% highlight javascript %}
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

SubType.prototype = new SuperType();
        
SubType.prototype.sayAge = function(){
    alert(this.age);
};
        
var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors);  //"red,blue,green,black"
instance1.sayName();      //"Nicholas";
instance1.sayAge();       //29
        
       
var instance2 = new SubType("Greg", 27);
alert(instance2.colors);  //"red,blue,green"
instance2.sayName();      //"Greg";
instance2.sayAge();       //27
{% endhighlight %}

组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为 Javascript 中最常用的继承模式。而且，instanceof 和 isPrototypeOf() 也能够用于识别基于组合继承创建的对象。

#### __寄生组合式继承__ 

组合继承最大的问题是无论什么情况，都会调用两次超类型构造函数：一次是在创建子类型的时候，另一次是在子类型构造函数内部。如下面组合继承的例子。

{% highlight javascript %}
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
        }
        
SuperType.prototype.sayName = function(){
    alert(this.name);
};

function SubType(name, age){  
    SuperType.call(this, name);         //第二次调用 SuperType()
            
    this.age = age;
}

SubType.prototype = new SuperType();   //第一次调用 SuperType()
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function() {
	alert(this.age);
};
{% endhighlight %}

在第一次调用 SuperType 构造函数时，SuperType.prototype 会得到两个属性：name 和 colors；它们都是 SuperType 的实例属性，只不过现在位于 SubType 的原型中。当调用 SubType 构造函数时，又会调用一次 SuperType 构造函数，这一次又在新对象上创建了实例属性 name 和 colors。于是，这两个属性就屏蔽了原型中的两个同名属性。为了解决这个问题，提出了寄生式继承。

所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。寄生组合式继承的基本模式如下所示。

{% highlight javascript %}
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}

function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype);   //create object
    prototype.constructor = subType;               //augment object
    subType.prototype = prototype;                 //assign object
}                            

inheritPrototype(SubType, SuperType);
        
SubType.prototype.sayAge = function(){
    alert(this.age);
};
        
var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors);  //"red,blue,green,black"
instance1.sayName();      //"Nicholas";
instance1.sayAge();       //29
             
var instance2 = new SubType("Greg", 27);
alert(instance2.colors);  //"red,blue,green"
instance2.sayName();      //"Greg";
instance2.sayAge();       //27
{% endhighlight %}

示例中的 inheritPrototype() 函数实现了寄生组合式继承的最简单形式。这个函数接受两个参数：子类型构造函数和超类型构造函数。在函数内部，第一步是创建超类型的一个副本。第二部是为创建的副本添加 constructor 属性，从而弥补因重写原型而失去的默认的 constructor 属性。最后一步，将创建的对象赋值给子类型的原型。

