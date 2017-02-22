---
layout: page
title: Build a JavaScript Calculator
teaser: "Advanced Front End Development Projects of freeCodeCamp."
categories:
    - web
tags:
    - project
---

### A Simple Calculator

目标: 在 [CodePen.io][1] 上做一个类似于 [FreeCodeCamp Calculator][2] 的APP。

功能：可以对两个数字进行加、减、乘、除的运算。

功能: 可以使用清除按钮清空当前的所有输入内容.

功能: 可以把多个运算连接起来操作, 直到按下等号键, 计算器输出正确的运算结果.

### HTML
{% highlight html %}
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">

<head>
	<meta http-equiv="content-type" content="text/html"; charset="utf-8" >
	<title>A Simple Calculator</title>
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
	<script type="text/javascript" src="01.js"></script>
	<link rel="stylesheet" type="text/css" media="screen" href="01.css">
	
</head>

<body>
	<div class="calculator">
		<p class="title">A Simple Calculator</p>
		<div class="top">
			<button id="clear" value="C">C</button>
			<input type="textbox" class="textbox" readonly>
			<div class="clearboth"></div>
		</div>

		<div class="buttons">
			<button value="7">7</button>
			<button value="8">8</button>
			<button value="9">9</button>
			<button class="operator" value="+">+</button>
			<button value="4">4</button>
			<button value="5">5</button>
			<button value="6">6</button>
			<button class="operator" value="-">-</button>
			<button value="1">1</button>
			<button value="2">2</button>
			<button value="3">3</button>
			<button class="operator" value="/">÷</button>
			<button value="0">0</button>
			<button value=".">.</button>
			<button id="eval" value="=">=</button>
			<button class="operator" value="*">x</button>
		</div>
		<div class="clearboth"></div>
	</div>
</body>
{% endhighlight %}

### CSS
{% highlight css %}
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font: bold 14px Arial, sans-serif;
}

body {
	height: 100%;
	background: white;
	background: radial-gradient(circle, #fff 20%, #ccc);
	background-size: cover;
}

.calculator {
	width: 325px;
	height: auto;
	margin: 100px auto;
	padding: 20px 20px 9px;
	background: #9dd2ea;
	background: linear-gradient(#9dd2ea, #8bceec);
	border-radius: 3px;
	box-shadow: 0px 4px #009de4, 0px 10px 15px rgba(0, 0, 0, .2);

}

#clear {
	float: left;
}

.title {
	font-size: 20px;
	font-family: 'Oleo Script', cursive;
	color: #F19953;
	margin: 0px 10px 20px;
	text-align: center;
}

.textbox {
	border: none;
	height: 40px;
	width: 212px;
	float: right;
	padding: 0 10px;
	background: rgba(0, 0, 0, 0.2);
	border-radius: 3px;
	box-shadow: inset 0px 4px rgba(0, 0, 0, 0.2);

	font-size: 17px;
	line-height: 40px;
	color: white;
	text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
	text-align: right;
	letter-spacing: 1px;
}

.clearboth {
	clear: both;
}

button {
	border: none;
	width: 66px;
	height: 36px;
	float: left;
	position: relative;
	top: 0;
	background: white;
	border-radius: 3px;
	box-shadow: 0px 4px rgba(0, 0, 0, 0.2);

	margin: 0 7px 11px 0;
	color: #888;
	line-height: 36px;
	text-align: center;
	user-select: none;
	transition: all 0.2s ease;
}

.operator {
	background: #FFF0F5;
	margin-right: 0;
}

#eval {
	background: #f1ff92;
	box-shadow: 0px 4px #9da853;
	color: #888e5f;
}

#clear {
	background: #ff9fa8;
	color: white;
	box-shadow: 0px 4px #ff7c87;
}

.buttons button:hover {
	background: #9c89f6; 
	box-shadow: 0px 4px #6b54d3;
	color: white;
}

#eval:hover {
	background: #abb850;
	box-shadow: 0px 4px #717a33;
	color: #ffffff;
}

#clear:hover {
	background: #f68991;
	box-shadow: 0px 4px #d3545d;
	color: white;
}

.buttons button:active {
	box-shadow: 0px 0px #6b54d3;
	top: 4px;
}

#eval:active {
	box-shadow: 0px 0px #717a33;
	top: 4px;
}

#clear:active {
	top: 4px;
	box-shadow: 0px 0px #d3545d;
}
{% endhighlight %}

### Javascript
{% highlight javascript %}
var ans = "";
var clear = false;
var calc = "";
$(document).ready(function() {
  $("button").click(function() {
    var text = $(this).attr("value");
    if(parseInt(text, 10) == text || text === "." || text === "/" || text === "*" || text === "-" || text === "+") {
      if(clear === false) {
        calc += text;
        $(".textbox").val(calc);
      } else {
        calc = text;
        $(".textbox").val(calc);
        clear = false;
      }
    } else if(text === "C") {
      calc = "";
      $(".textbox").val("");
    } else if(text === "=") {
      ans = eval(calc);
      $(".textbox").val(ans);
      clear = true;
    }
  });
});
{% endhighlight %}

[源码下载][3]

[1]: http://codepen.io/
[2]: http://codepen.io/FreeCodeCamp/full/zrRzMR
[3]: https://github.com/Farewing/A-Simple-Calculator