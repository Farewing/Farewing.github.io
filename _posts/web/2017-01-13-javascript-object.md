---
layout: page
title: Javascript对象与继承（一）
subheadline: "面向对象(Object Oriented)程序设计之创建对象"
teaser: "本编主要总结记录Javascript对象创建机制，由于Javascript特有的原型链(Prototype Chain)模式和对象概念，与其他语言中的继承，类，实例等概念有较大不同，故做本篇加强理解。"
categories:
    - web
tags:
    - Javascript
---

### 创建对象

#### 工厂模式
工厂模式是软件工程领域一种广为人知的设计模式，这种模式抽象了创建具体对象的过程。由于Javascript无法创建类，开发人员发明了一种函数，用函数来封装以特定接口创建对象的细节，如下面的例子所示。

{% highlight javascript %}
function createPerson(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };    
    return o;
}
        
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
        
person1.sayName();   //"Nicholas"
person2.sayName();   //"Greg"
{% endhighlight %}

工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎么知道一个对象的类型）。

#### 构造函数模式
使用构造函数将前面的例子重写如下。

{% highlight javascript %}
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    };    
}
        
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
        
person1.sayName();   //"Nicholas"
person2.sayName();   //"Greg"
{% endhighlight %}

在这个例子中，Person()函数取代了 createPerson()函数。其中存在以下不同之处：

* 没有显示地创建对象;
* 直接将属性和方法赋给了this对象；
* 没有return语句。

{% highlight javascript %}
alert(person1 instanceof Object);  //true
alert(person1 instanceof Person);  //true
alert(person2 instanceof Object);  //true
alert(person2 instanceof Person);  //true
        
alert(person1.constructor == Person);  //true
alert(person2.constructor == Person);  //true
        
alert(person1.sayName == person2.sayName);  //false 
{% endhighlight %}

创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型；而这正是构造函数模式胜过工厂模式的地方。  

构造函数与其它函数的唯一区别，就在于调用他们的方式不同。任何函数，只要通过new操作符来调用，那它就可以作为构造函数；而任何函数，如果不通过new操作符来调用，那它跟普通函数也不会有什么两样。例如，前面例子中定义的Person()函数可以通过下列任何一种方式来调用。

{% highlight javascript %}
var person = new Person("Nicholas", 29, "Software Engineer");
person.sayName();   //"Nicholas"
        
Person("Greg", 27, "Doctor");  //adds to window
window.sayName();   //"Greg"
        
var o = new Object();
Person.call(o, "Kristen", 25, "Nurse");
o.sayName();    //"Kristen"
{% endhighlight %}

#### 原型模式
我们创建的每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的实例共享的属性和方法。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中，如下面的例子所示。

{% highlight javascript %}
function Person(){
}
        
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};
        
var person1 = new Person();
person1.sayName();   //"Nicholas"
        
var person2 = new Person();
person2.sayName();   //"Nicholas"
      
alert(person1.sayName == person2.sayName);  //true
{% endhighlight %}

##### __理解原型对象__  


无论什么时候，只要创建一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。默认情况下，所有原型对象都会自动获得一个constructor属性，这个属性包含一个指向prototype属性所在函数的指针。  

以前面使用Person()构造函数和Person.prototype创建实例的代码为例，下图展示了各个对象之间的关系。

![inheritance01](/images/inheritance01.png)

上图展示了Person()构造函数、Person的原型属性以及Person现在的两个实例之间的关系。在此，Person.prototype指向了原型对象，而Person.prototype.constructor又指回了Person。原型对象中除了包含constructor属性之外，还包含后来添加的其它属性。Person的每个实例person1和person2都包含一个内部属性，该属性仅仅指向了Person.prototype；换句话说，它们与构造函数没有直接关系。此外，要格外注意的是，虽然这两个实例都不包含属性和方法，但我们却可以调用person1.sayName()。这是通过查找对象属性的过程来实现的，即首先搜索对象实例，如果没有找到则继续搜索指针指向的原型对象，如果在原型对象中找到了这个属性，则返回属性的值。  

虽然可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。如果我们在实例中添加一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。如下面的例子。

{% highlight javascript %}
person1.name = "Greg";
alert(person1.name);   //"Greg" – from instance
alert(person2.name);   //"Nicholas" – from prototype
        
delete person1.name;
alert(person1.name);   //"Nicholas" - from the prototype
{% endhighlight %}

##### __原型对象的问题__  

首先，它省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值。其次原型中所有属性是被很多实例共享的，这种共享对于函数非常合适。对于包含基本值的属性倒也说得过去，可以通过在实例上添加一个同名属性，可以隐藏原型中的对应属性。然而，对于包含引用类型值的属性来说，问题就比较突出了。如下面的例子。

{% highlight javascript %}
function Person(){
}
        
Person.prototype = {
    constructor: Person,
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    friends : ["Shelby", "Court"],
    sayName : function () {
        alert(this.name);
    }
};
        
var person1 = new Person();
var person2 = new Person();
        
person1.friends.push("Van");
        
alert(person1.friends);    //"Shelby,Court,Van"
alert(person2.friends);    //"Shelby,Court,Van"
alert(person1.friends === person2.friends);  //true
{% endhighlight %}

#### 组合使用构造函数模式和原型模式
创建自定义类型的最常见方式，就是组合使用构造函数模式和原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本，但同时有共享着对方法的引用，最大限度地节省了内存。

{% highlight javascript %}
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}
        
Person.prototype = {
    constructor: Person,
    sayName : function () {
        alert(this.name);
    }
};
        
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
        
person1.friends.push("Van");
        
alert(person1.friends);    //"Shelby,Court,Van"
alert(person2.friends);    //"Shelby,Court"
alert(person1.friends === person2.friends);  //false
alert(person1.sayName === person2.sayName);  //true
{% endhighlight %}

这种构造函数与原型混成的模式，是目前Javascript中使用最广泛、认同度最高的一中创建自定义类型的方法。可以说，这是用来定义引用类型的一种默认模式。

