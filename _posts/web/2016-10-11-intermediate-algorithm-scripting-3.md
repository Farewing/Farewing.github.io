---
layout: page
title: Intermediate Algorithm Scripting（三）
subheadline: "freeCodeCamp中阶算法（三）"
teaser: "该部分为freeCodeCamp训练中JavaScript中阶算法第三部分，其中笔者认为有难度的附有解题思路。 "
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
    caption: Photo by Farewing
    caption_url: https://blaz.photography/
---

freeCodeCamp训练中JavaScript中阶算法地址[戳这里][1]。


###  Smallest Common Multiple

找出能被两个给定参数和它们之间的连续数字整除的最小公倍数。

范围是两个数字构成的数组，两个数字不一定按数字顺序排序。

例如对 1 和 3 —— 找出能被 1 和 3 和它们之间所有数字整除的最小公倍数。

{% highlight javascript %}
function smallestCommon(m, n){
  var max = m > n ? m : n;
  
  for(var i = max; ;i += max){
    if((i % m === 0) && (i % n === 0)){
      return i;
     }
  }
}

function divide(arr) {
  if(arr.length === 1)
    return arr[0];
  var mid = arr.length / 2;
  var left = arr.slice(0, mid);
  var right = arr.slice(mid);
  return smallestCommon(divide(left), divide(right));
}

function smallestCommons(arr) {
 if(arr[0] > arr[1]) {
   var temp = arr[1];
   arr[1] = arr[0];
   arr[0] = temp;
 }
  var res = [];
  while(arr[0] <= arr[1]) {
    res.push(arr[0]);
    arr[0]++;
  }
  return divide(res);
}

smallestCommons([1,5]);
{% endhighlight %}



### Finders Keepers

写一个 function，它浏览数组（第一个参数）并返回数组中第一个通过某种方法（第二个参数）验证的元素。

{% highlight javascript %}
function find(arr, func) {
  var num = arr.filter(func);
  
  return num[0];
}

find([1, 2, 3, 4], function(num){ return num % 2 === 0; });
{% endhighlight %}



### Drop it

队友该卖就卖，千万别舍不得。

当你的队伍被敌人包围时，你选择拯救谁、抛弃谁非常重要，如果选择错误就会造成团灭。

如果是AD或AP，优先拯救。

因为AD和AP是队伍输出的核心。

其次应该拯救打野。

因为打野死了对面就可以无所顾虑地打龙。

最后才是辅助或上单。

因为辅助和上单都是肉，死了也不会对团队造成毁灭性影响，该卖就卖。

但真实中的团战远比这要复杂，你的队伍很可能会被敌人分割成2个或3个部分。

当你救了一个重要的人时，很可能其他队友也会因此获救。

举个例子：

辅助和AD经常是在一起的，打野和中单在一起，上单经常一个人。

你救了AD，辅助也经常因此获救。

让我们来丢弃数组(arr)的元素，从左边开始，直到回调函数return true就停止。

第二个参数，func，是一个函数。用来测试数组的第一个元素，如果返回fasle，就从数组中抛出该元素(注意：此时数组已被改变)，继续测试数组的第一个元素，如果返回fasle，继续抛出，直到返回true。

最后返回数组的剩余部分，如果没有剩余，就返回一个空数组。

{% highlight javascript %}
function drop(arr, func) {
  var tmp;
      var res = [];
      for(var i=0,len=arr.length;i<len;i++){
          tmp = arr.shift();
          if(func(tmp)){
            //需要置回弹出的元素
            arr.unshift(tmp);
            break;
          }
      }
      return arr;
}

drop([1, 2, 3], function(n) {return n < 3; });
{% endhighlight %}



### Steamroller

对嵌套的数组进行扁平化处理。你必须考虑到不同层级的嵌套。

steamroller([[["a"]], [["b"]]]) 应该返回 ["a", "b"]。
steamroller([1, [2], [3, [[4]]]]) 应该返回 [1, 2, 3, 4]。
steamroller([1, [], [3, [[4]]]]) 应该返回 [1, 3, 4]。

{% highlight javascript %}
function steamroller(arr, flatArr) {
  // I'm a steamroller, baby
  if (!flatArr) 
    flatArr = [];
  
  for (var i in arr) {
    if(!Array.isArray(arr[i])){
      flatArr.push(arr[i]);
    }else {
      steamroller(arr[i], flatArr);
    } 
  }
  return flatArr;
}

steamroller([1, [2], [3, [[4]]]]);

{% endhighlight %}



### Binary Agents

传入二进制字符串，翻译成英语句子并返回。

二进制字符串是以空格分隔的。

{% highlight javascript %}
function binaryAgent(str) {
  var t = str.split(" ");
  var s = "";
  for(var i in t) {
    s += String.fromCharCode(parseInt(t[i], 2));
  }
  return s;
}

binaryAgent("01000001 01110010 01100101 01101110 00100111 01110100 00100000 01100010 01101111 01101110 01100110 01101001 01110010 01100101 01110011 00100000 01100110 01110101 01101110 00100001 00111111");
{% endhighlight %}



### Everything Be True

所有的东西都是真的！

完善编辑器中的every函数，如果集合(collection)中的所有对象都存在对应的属性(pre)，并且属性(pre)对应的值为真。函数返回ture。反之，返回false。

记住：你只能通过中括号来访问对象的变量属性(pre)。

提示：你可以有多种实现方式，最简洁的方式莫过于[Array.prototype.every()][2]。

{% highlight javascript %}
function every(collection, pre) {
  return collection.every(function(e){
    return (e.hasOwnProperty(pre) && e[pre]);
  });
}

every([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}, {"user": "Laa-Laa", "sex": "female"}, {"user": "Po", "sex": "female"}], "sex");
{% endhighlight %}







### All Algorithm-Level
{: .t60 }

{% include list-posts tag='algorithm' %}




 [1]: https://www.freecodecamp.cn/challenges/smallest-common-multiple
 [2]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every
 [3]: https://en.wikipedia.org/wiki/Base_pair
