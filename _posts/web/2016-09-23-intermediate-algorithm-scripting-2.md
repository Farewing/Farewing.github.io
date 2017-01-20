---
layout: page
title: Intermediate Algorithm Scripting（二）
subheadline: "freeCodeCamp中阶算法（二）"
teaser: "该部分为freeCodeCamp训练中JavaScript中阶算法第二部分，其中笔者认为有难度的附有解题思路。 "
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


###  Boo who 
检查一个值是否是基本布尔类型，并返回 true 或 false。

基本布尔类型即 true 和 false。
{% highlight javascript %}
function boo(bool) {
  return typeof bool === "boolean";
}

boo(null);
{% endhighlight %}



### Sorted Union
写一个 function，传入两个或两个以上的数组，返回一个以给定的原始数组排序的不包含重复值的新数组。

换句话说，所有数组中的所有值都应该以原始顺序被包含在内，但是在最终的数组中不包含重复值。

非重复的数字应该以它们原始的顺序排序，但最终的数组不应该以数字顺序排序。

unite([1, 3, 2], [5, 2, 1, 4], [2, 1]) 应该返回 [1, 3, 2, 5, 4]。
unite([1, 3, 2], [1, [5]], [2, [4]]) 应该返回 [1, 3, 2, [5], [4]]。

{% highlight javascript %}
function unite(arr1, arr2, arr3) {
  var args = Array.prototype.slice.call(arguments);
  var t = args.reduce(function(a, b) {
    return a.concat(b);
  });
  var arr = [];
  for(var i in t) {
    if(arr.indexOf(t[i]) === -1)
      arr.push(t[i]);
  }
  return arr;
}

unite([1, 3, 2], [5, 2, 1, 4], [2, 1]);
{% endhighlight %}



### Convert HTML Entities

将字符串中的字符 &、<、>、" （双引号）, 以及 ' （单引号）转换为它们对应的 HTML 实体。。

{% highlight javascript %}
function convert(str) {
  return str.replace(/&/g, '&amp;').replace(/'/g, '&apos;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;');
}

convert("Shindler's List");
{% endhighlight %}



### Spinal Tap Case

将字符串转换为 spinal case。Spinal case 是 all-lowercase-words-joined-by-dashes 这种形式的，也就是以连字符连接所有小写单词。

{% highlight javascript %}
function spinalCase(str) {
  if(!/[^a-zA-Z]/.test(str) ){
        var res = "";
        for(var i = 0; i < str.length; i++){
            if(/[A-Z]/.test(str[i])){
                var tmp = " " + str[i];
                res += tmp;
            }else {
                res += str[i];
            }
        }
        str = res;
    }
  return str.toLowerCase().replace(/[^a-zA-Z]/g," ").replace(/\s/g,'-');
}

spinalCase('This Is Spinal Tap');
{% endhighlight %}



### Sum All Odd Fibonacci Numbers
给一个正整数num，返回小于或等于num的斐波纳契奇数之和。

斐波纳契数列中的前几个数字是 1、1、2、3、5 和 8，随后的每一个数字都是前两个数字之和。

例如，sumFibs(4)应该返回 5，因为斐波纳契数列中所有小于4的奇数是 1、1、3。

提示：此题不能用递归来实现斐波纳契数列。因为当num较大时，内存会溢出，推荐用数组来实现。

{% highlight javascript %}
function sumFibs(num) {
  var arr = [1, 1];
  var i = 1, count = 1;
  while(arr[i] <= num) {
    
    if(arr[i] % 2 !== 0)
      count += arr[i];
    i++;
    arr[i] = arr[i - 1] + arr[i - 2];
  }
  return count;
}

sumFibs(4);
{% endhighlight %}



### Sum All Primes

求小于等于给定数值的质数之和。

只有 1 和它本身两个约数的数叫质数。例如，2 是质数，因为它只能被 1 和 2 整除。1 不是质数，因为它只能被自身整除。

给定的数不一定是质数。。

{% highlight javascript %}
function isPrime(num){
      for(var i = 2; i <= Math.sqrt(num); i++){
          if(num % i === 0){
                return false;
          }
      }
      return true;
}

function sumPrimes(num) {
   var sum = 0;
   for(var i = 2; i <= num; i++){
          if(isPrime(i)){
              sum += i;
          }
      }
      return sum;
}

sumPrimes(10);
{% endhighlight %}







### All Algorithm-Level
{: .t60 }

{% include list-posts tag='algorithm' %}




 [1]: https://www.freecodecamp.cn/challenges/boo-who
 [2]: http://www.mathsisfun.com/roman-numerals.html
 [3]: https://en.wikipedia.org/wiki/Base_pair
