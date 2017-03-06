---
layout: page
title: Javascript 闭包及应用
teaser: "在 Javascript 编程中，函数表达式是一种非常有用的技术。本篇涉及函数表达式、函数声明、闭包等概念以及闭包的相关应用。"
categories:
    - web
tags:
    - Javascript
---

定义函数的方式有两种：一种是函数声明，另一种是函数表达式。函数声明的语法是这样的。

{% highlight javascript %}
function functionName(arg0, arg1, arg2) {
	//函数体
}
{% endhighlight %}

第二种创建函数的方式是使用函数表达式。函数表达式有几种不同的语法形式。下面是最常见的一种形式。

{% highlight javascript %}
var functionName = function(arg0, arg1, arg2) {
	//函数体
};
{% endhighlight %}

这种形式看起来好像是常规的变量赋值语句，即创建一个函数并将它赋值给变量 functionName。这种情况下创建的函数叫做匿名函数(Anonymous Function)，因为 function 关键字后面没有标示符。（匿名函数也叫拉姆达函数）

#### __闭包__

闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数，如下面的例子所示。

{% highlight javascript %}
function createCoparisonFunction(propertyName) {
	return function(object1, object2) {
		var value1 = object1[propertyName];     //1
		var value2 = object2[propertyName];     //2

		if (value1 < value2) {
			return -1;
		} else if (value1 > value2) {
			return 1;
		} else {
			return 0;
		}
	};
}
{% endhighlight %}

在这个例子中，标注1、2行代码是内部函数（一个匿名函数）中的代码，这两行代码访问了外部函数的变量 propertyName。由于闭包会携带包含它的函数的作用域，因此会比其它函数占用更多的内存，过度使用闭包可能会导致内存占用过多。

作用域这种配置机制引出了一个值得注意的副作用，即闭包只能取得包含函数中任何变量的最后一个值。如下面的例子。

{% highlight javascript %}
function createFunctions(){
    var result = new Array();
                
    for (var i=0; i < 10; i++){
        result[i] = function(){
             return i;
        };
    }
                
    return result;   
}         
var funcs = createFunctions();
            
//every function outputs 10
for (var i=0; i < funcs.length; i++){
    document.write(funcs[i]() + "<br />");
}   
{% endhighlight %}

每个函数都将返回 10。因为每个函数的作用域链中都保存着 createFunction() 函数的活动对象，所以它们引用的都是同一个变量 i。当 createFunction() 函数返回后，变量 i 的值是 10，此时每个函数都引用着保存变量 i 的同一个变量对象，所以每个函数内部 i 的值都是 10。

我们创建另一个匿名函数强制让闭包的行为符合预期，如下所示。

{% highlight javascript %}
function createFunctions(){
    var result = new Array();
                
    for (var i=0; i < 10; i++){
        result[i] = function(num){
            return function(){
                return num;
            };
        }(i);
    }
                
    return result;
}
{% endhighlight %}

重写了前面的 createFunction() 函数后，每个函数就会返回各自不同的索引值了。这里的匿名函数有一个参数 num，也就是最终的函数要返回的值。在调用每个匿名函数时，我们传入了变量 i。由于函数参数是按值传递的，所以就会将变量 i 的当前值赋值给参数 num。而在这个匿名函数内部，又创建并返回了一个访问 num 的闭包。这样一来，result数组中的每个函数都有自己 num 变量的一个副本，因此就可以返回各自不同的值了。

#### 关于 this 对象

在闭包中使用 this 对象也可能会导致一些问题。在全局函数中，this 等于 window，而当函数被作为某个对象的方法调用时，this等于那个对象。不过，匿名函数的执行环境具有全局性，因此其 this 对象通常指向 window。如下面的例子所示。

{% highlight javascript %}
var name = "The Window";
        
var object = {
    name : "My Object",
        
    getNameFunc : function(){
        return function(){
            return this.name;
        };
    }
};
        
alert(object.getNameFunc()());  //"The Window"
{% endhighlight %}

每个函数在被调用时，其活动对象都会自动取得两个特殊变量：this 和 arguments。内部函数在搜索着两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量。不过，把外部作用域中的 this 对象保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了，如下所示。

{% highlight javascript %}
var name = "The Window";
            
var object = {
    name : "My Object",
            
    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };
    }
};
            
alert(object.getNameFunc()());  //"MyObject"
{% endhighlight %}

在定义匿名函数之前，我们把 this 对象赋值给了一个名叫 that 的变量。而在定义了闭包之后，闭包也可以访问这个变量，因为它是我们在包含函数中特意声明的一个变量。即使在函数返回之后，that 也仍然引用着 object，所以调用object.getNameFunc()()就返回了"My Object"。

#### 模仿块级作用域

Javascript 没有块级作用域的概念，这意味着在块语句中定义的变量，实际上是在包含函数中而非语句中创建的，如下面的例子。

{% highlight javascript %}
function outputNumbers(count){
    for (var i=0; i < count; i++){
        alert(i);
    }
            
    alert(i);   //count
}

outputNumbers(5);
{% endhighlight %}

在Java、C++等语言中，变量 i 只会在 for 循环的语句块中有定义，循环一旦结束，变量 i 就会被销毁。可是在 Javascript 中，变量 i 是定义在 outputNumbers()的活动对象中的，因此从它有定义开始，就可以在函数内部随处访问它。

Javascript从来不会告诉你是否多次声明了同一个变量；遇到这种情况，它只会对后续的声明视而不见（不过，它会执行后续声明中的变量初始化）。匿名函数可以用来模仿块级作用域并避免这个问题。语法如下所示。

{% highlight javascript %}
(function() {
	//这里是块级作用域
}) ();
{% endhighlight %}

无论在什么地方，只要临时需要一些变量，就可以使用私有作用域，例如;

{% highlight javascript %}
funtion outputNumbers (count) {
	(function () {
		for(var i = 0; i < count; i++) {
			alert(i);
		}
	}) ();

	alert(i)	//导致一个错误！
}
{% endhighlight %}

#### 私有变量

任何函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。私有变量包括函数的参数、局部变量和在函数内部定义的其它函数。我们把有权访问私有变量和私有函数的公有方法称为特权方法。有两种在对象上创建特权方法的方式。第一种是在构造函数中定义特权方法，基本模式如下。

{% highlight javascript %}
funtion MyObject() {
	//私有变量和私有函数
	var privateVariable = 10;

	function privateFunction() {
		return false;
	}

	//特权方法
	this.publicMethod = function() {
		privateVariable++;
		return privateFunction();
	};
}
{% endhighlight %}

{% highlight javascript %}
function Person(name){            
    this.getName = function(){
        return name;
    };
            
    this.setName = function (value) {
        name = value;
    };
}
            
var person = new Person("Nicholas");
alert(person.getName());   //"Nicholas"
person.setName("Greg");
alert(person.getName());   //"Greg"
{% endhighlight %}

能够在构造函数中定义特权方法，是因为特权方法作为闭包有权访问在构造函数中定义的所有变量和函数。不过，在构造函数中定义特权方法也有一个缺点，那就是必须使用构造函数模式来达到这个目的。构造函数模式的缺点是针对每个实例都会创建同样一组方法，而使用静态私有变量来实现特权方法就可以避免这个问题。

#### 静态私有变量

通过在私有作用域定义私有变量或函数，同样也可以创建特权方法，其基本模式如下所示。

{% highlight javascript %}
(function() {
	
	//私有变量和私有函数
	var privateVariable = 10;

	function privateFunction() {
		return false;
	}

	//构造函数
	MyObject = function() {
	};

	//公有/特权方法
	MyObject.prototype.publicMethod = function() {
		privateVariable++;
		return privateFunction();
	};

}) ();
{% endhighlight %}

这种模式创建了一个私有作用域，并在其中封装了一个构造函数及相应的方法。在私有作用域中，首先定义了私有变量和私有函数，然后又定义了构造函数及其公有方法。公有方法是在原型上定义的，这一点体现了典型的原型模式。需要注意的是，这个模式在定义构造函数时并没有使用函数声明，而是使用了函数表达式。函数声明只能创建局部函数，那并不是我们想要的。出于同样的原因，我们也没有在声明 MyObject 时使用 var 关键字。但是在严格模式下给未经声明的变量赋值会导致错误。

这个模式与构造函数中定义特权方法的主要区别就在于私有变量和函数是由于实例共享的。由于特权方法是在原型上定义的，因此所有实例都使用同一个函数。而这个特权方法，作为一个闭包，总是保存着对包含作用域的引用。再看一个例子。

{% highlight javascript %}
(function(){           
    var name = "";
                
    Person = function(value){                
        name = value;                
    };
                
    Person.prototype.getName = function(){
        return name;
    };
                
    Person.prototype.setName = function (value){
        name = value;
    };
})();
            
var person1 = new Person("Nicholas");
alert(person1.getName());   //"Nicholas"
person1.setName("Greg");
alert(person1.getName());   //"Greg"
                               
var person2 = new Person("Michael");
alert(person1.getName());   //"Michael"
alert(person2.getName());   //"Michael"
{% endhighlight %}

在这种模式下，变量 name 就会变成了一个静态的，由所有实例共享的属性。也就是说，在一个实例上调用 setName() 会影响所有实例。而调用setName() 或新建一个 Person 实例都会赋予 name 属性一个新值。

以这种方式创建静态私有变量会因为使用原型而增进代码复用，但每个实例都没有自己的私有变量。

#### 模块模式

前面的模式适用于为自定义类型创建私有变量和特权方法的。而模块模式市委单例创建私有变量和特权方法。所谓单例是指只有一个实例的对象。如下所示。

{% highlight javascript %}
var singleton = {
	name : value,
	method : function() {
		//方法代码
	}
}；
{% endhighlight %}
模块模式通过为单例添加私有变量和特权方法能够使其增强，语法形式如下。

{% highlight javascript %}
var singleton = funtion() {
	
	//私有变量和私有函数
	var privateVariable = 10;

	function privateFunction() {
		return false;
	}

	//特权/公有方法和属性
	return {
		publicProperty : true,
		publicMethod : function() {
			privateVariable++;
			return privateFunction();
		}
	};
} ();
{% endhighlight %}

这个模块模式使用了一个返回对象的匿名函数。在这个匿名函数内部，首先定义了私有变量和函数，然后，将一个对象字面量作为函数的值返回。由于这个对象是在匿名函数内部定义的，因此它的公有方法有权访问私有变量和函数。从本质上讲，这个对象字面量定义的是单例的公共接口。这种模式在需要对单例进行某些初始化，同时又需要维护其私有变量时是非常有用的。例如。

{% highlight javascript %}
function BaseComponent(){
}
            
function OtherComponent(){
}
        
var application = function(){
            
    //private variables and functions
    var components = new Array();
            
    //initialization
    components.push(new BaseComponent());
            
    //public interface
    return {
        getComponentCount : function(){
            return components.length;
        },
            
        registerComponent : function(component){
            if (typeof component == "object"){
                components.push(component);
            }
        }
    };
}();

application.registerComponent(new OtherComponent());
alert(application.getComponentCount());  //2
{% endhighlight %}

简言之，如果必须创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有数据的方法，那么就可以使用模块模式。以这种模式创建的每个单例都是 Object 的实例，因为最终要通过一个对象字面量来表示它。