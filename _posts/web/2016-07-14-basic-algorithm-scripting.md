---
layout: page
title: Basic Algorithm Scripting
subheadline: "freeCodeCamp基础算法"
teaser: "该部分为freeCodeCamp训练中Javascript基础算法，在此只记录题目和解法，思路略。 "
categories:
    - web
tags:
    - web
    - algorithm
header: no
image:
    title: mediaplayer_js-title.jpg
    thumb: mediaplayer_js-thumb.jpg
    homepage: mediaplayer_js-home.jpg
    caption: Photo by Corey Blaz
    caption_url: https://blaz.photography/
---

freeCodeCamp训练中Javascript基础算法地址[戳这里][1]。


### Reverse a String
翻转字符串

先把字符串转化成数组，再借助数组的reverse方法翻转数组顺序，最后把数组转化成字符串。

你的结果必须得是一个字符串

{% highlight javascript %}
function reverseString(str) {
  var arr = str.split("");
  arr.reverse();
  str = arr.join("");
  return str;
}

reverseString("hello");
{% endhighlight %}



### Check for Palindromes
如果给定的字符串是回文，返回true，反之，返回false。

如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)。

注意你需要去掉字符串多余的标点符号和空格，然后把字符串转化成小写来验证此字符串是否为回文。

函数参数的值可以为"racecar"，"RaceCar"和"race CAR"。

{% highlight javascript %}
function palindrome(str) {
  str = str.replace(/[\W\s_]/gi, "");
  return str.toLowerCase() === str.split("").reverse().join("").toLowerCase();
}

palindrome("eye");
{% endhighlight %}



### Find the Longest Word in a String 

找到提供的句子中最长的单词，并计算它的长度。

函数的返回值应该是一个数字。

{% highlight javascript %}
function findLongestWord(str) {
  var s = str.split(" ");
  var t = s[0];
  for(var i = 1; i < s.length; i++) {
    if(t.length < s[i].length)
      t = s[i];
  }
  return t.length;
}

findLongestWord("The quick brown fox jumped over the lazy dog");
{% endhighlight %}



### Title Case a Sentence
确保字符串的每个单词首字母都大写，其余部分小写。

像'the'和'of'这样的连接符同理。

{% highlight javascript %}
function titleCase(str) {
   str = str.toLowerCase();
   var e = /^\w|\s\w/g;
   return str.replace(e, function(m) {
     return m.toUpperCase();
   });
}

titleCase("I'm a little tea pot");
{% endhighlight %}



### Chunky Monkey 
猴子吃香蕉可是掰成好几段来吃哦！

把一个数组arr按照指定的数组大小size分割成若干个数组块。

例如:chunk([1,2,3,4],2)=[[1,2],[3,4]];

chunk([1,2,3,4,5],2)=[[1,2],[3,4],[5]];

{% highlight javascript %}
function chunk(arr, size) {
  var t = [];
  var i;
  for(i = 0; i< arr.length - size; i+=size)
    t.push(arr.slice(i, i + size));
  t.push(arr.slice(i, arr.length));
  return t;
}

chunk([0, 1, 2, 3, 4, 5], 2);
{% endhighlight %}



### Mutations
蛤蟆可以吃队友，也可以吃对手。

如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回true。

举例，["hello", "Hello"]应该返回true，因为在忽略大小写的情况下，第二个字符串的所有字符都可以在第一个字符串找到。

["hello", "hey"]应该返回false，因为字符串"hello"并不包含字符"y"。

["Alien", "line"]应该返回true，因为"line"中所有字符都可以在"Alien"找到。

{% highlight javascript %}
function mutation(a) {
  var t = a[0].toLowerCase().split("");
  var s = a[1].toLowerCase().split("");
  var j;
  for(var i = 0; i < s.length; i++) {
    for(j = 0; j < t.length; j++){
        if(s[i] === t[j]) {
          console.log(j);
          break;
        }    
    }
    if(j === t.length)
      return false;    
  }
  return true;
}
mutation(["hello", "hey"]);
{% endhighlight %}



### Falsy Bouncer
真假美猴王！

删除数组中的所有假值。

在JavaScript中，假值有false、null、0、""、undefined 和 NaN。

{% highlight javascript %}
function bouncer(arr) {
  return arr.filter(function(e) {
      return Boolean(e);
  });
}

bouncer([false, null, 0, NaN, undefined, ""]);
{% endhighlight %}



### Seek and Destroy

金克斯的迫击炮！

实现一个摧毁(destroyer)函数，第一个参数是待摧毁的数组，其余的参数是待摧毁的值。

{% highlight javascript %}
function destroyer() {
  var arr = arguments[0];
  var params = [];

  for (var k = 1; k < arguments.length; k++)
    params.push(arguments[k]);

  return arr.filter(function(item) {
    return params.indexOf(item) < 0 ? true : false;
  });
}

destroyer([1, 2, 3, 1, 2, 3], 2, 3);
{% endhighlight %}



### Where do I belong

我身在何处？

先给数组排序，然后找到指定的值在数组的位置，最后返回位置对应的索引。

举例：where([1,2,3,4], 1.5) 应该返回 1。因为1.5插入到数组[1,2,3,4]后变成[1,1.5,2,3,4]，而1.5对应的索引值就是1。

同理，where([20,3,5], 19) 应该返回 2。因为数组会先排序为 [3,5,20]，19插入到数组[3,5,20]后变成[3,5,19,20]，而19对应的索引值就是2。

{% highlight javascript %}
function where(arr, num) {
  arr.push(num);
  arr.sort(function(a, b) {
    return a - b;
  });
  return arr.indexOf(num);
}

where([2, 5, 10], 15);
{% endhighlight %}



### All Algorithm-Level
{: .t60 }

{% include list-posts tag='algorithm' %}




 [1]: https://www.freecodecamp.cn/challenges/get-set-for-our-algorithm-challenges

