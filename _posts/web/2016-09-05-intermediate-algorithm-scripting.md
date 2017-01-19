---
layout: page
title: Intermediate Algorithm Scripting（一）
subheadline: "freeCodeCamp中阶算法（一）"
teaser: "该部分为freeCodeCamp训练中JavaScript中阶算法第一部分，其中笔者认为有难度的附有解题思路。 "
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


###  Sum All Numbers in a Range 
我们会传递给你一个包含两个数字的数组。返回这两个数字和它们之间所有数字的和。

最小的数字并非总在最前面。

{% highlight javascript %}
function sumAll(arr) {
  var count = Math.max.apply(null, arr);
  for(var i = Math.min.apply(null, arr); i < Math.max.apply(null, arr); i++)
    count += i;
  return count;
}

sumAll([1, 4]);
{% endhighlight %}



### Diff Two Arrays 
比较两个数组，然后返回一个新数组，该数组的元素为两个给定数组中所有独有的数组元素。换言之，返回两个数组的差异。

{% highlight javascript %}
function diff(arr1, arr2) {
  var newArr = [];
  
  newArr = arr1.filter(function(e) {
    return arr2.indexOf(e) === -1;
  });
  
  return newArr.concat(arr2.filter(function(e){
    return arr1.indexOf(e) === -1;
  }));
}

diff([1, 2, 3, 5], [1, 2, 3, 4, 5]);

{% endhighlight %}



### Roman Numeral Converter

将给定的数字转换成[罗马数字][2]。

所有返回的 罗马数字 都应该是大写形式。

{% highlight javascript %}
function convert(num) {
  
  var arr = num.toString().split(""),
      key = ["","C","CC","CCC","CD","D","DC","DCC","DCCC","CM",
               "","X","XX","XXX","XL","L","LX","LXX","LXXX","XC",
               "","I","II","III","IV","V","VI","VII","VIII","IX"],
        roman = "",
        i = 3;
  
  while(i--) 
     roman = (key[+arr.pop() + (i * 10)] || "") + roman;
  return Array(+arr.join("") + 1).join("M") + roman;
}

convert(876);
{% endhighlight %}

### 递归解法

{% highlight javascript %}
var romanMatrix=[
  [1000,'M'],
  [900,'CM'],
  [500,'D'],
  [400,'DC'],
  [100,'C'],
  [90, 'XC'],
  [50, 'L'],
  [40, 'XL'],
  [10, 'X'],
  [9, 'IX'],
  [5, 'V'],
  [4, 'IV'],
  [1, 'I']
];
function convert(num) {
  //给定数字为0时，返回空值
  if(num===0)
    return '';
  //递归找出给定数字对应的罗马数字
  for(var i=0;i<romanMatrix.length;i++){
    if(num>=romanMatrix[i][0])
      return romanMatrix[i][1]+convert(num-romanMatrix[i][0]);
  }
}

convert(36);
{% endhighlight %}



### Where art thou
写一个 function，它遍历一个对象数组（第一个参数）并返回一个包含相匹配的属性-值对（第二个参数）的所有对象的数组。如果返回的数组中包含 source 对象的属性-值对，那么此对象的每一个属性-值对都必须存在于 collection 的对象中。

例如，如果第一个参数是 [{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }]，第二个参数是 { last: "Capulet" }，那么你必须从数组（第一个参数）返回其中的第三个对象，因为它包含了作为第二个参数传递的属性-值对。。

{% highlight javascript %}
function where(collection, source) {
  var arr = [];
  var value = Object.keys(source);
   for(var i = 0; i < collection.length; i++) {
      if(Object.keys(collection[i]).length >= Object.keys(source).length){
         var flag = true;
         for(var key in source) {
            if(collection[i].hasOwnProperty(key) ) {
               if(collection[i][key] != source[key]) {
                  flag = false;
               }
            } else {
               flag = false;
            }
         }
         if(flag) {
            arr.push(collection[i]);
         }
      }
   }
  return arr;
}

where([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" });
{% endhighlight %}



### Search and Replace
使用给定的参数对句子执行一次查找和替换，然后返回新句子。

第一个参数是将要对其执行查找和替换的句子。

第二个参数是将被替换掉的单词（替换前的单词）。

第三个参数用于替换第二个参数（替换后的单词）。

注意：替换时保持原单词的大小写。例如，如果你想用单词 "dog" 替换单词 "Book" ，你应该替换成 "Dog"。

{% highlight javascript %}
function myReplace(str, before, after) {
  
  if(before.charCodeAt(0) > 64 && before.charCodeAt(0) < 90)
    after = after.replace(after.charAt(0), after.charAt(0).toUpperCase());
  
  return str.replace(before, after);
}

myReplace("A quick brown fox jumped over the lazy dog", "jumped", "leaped");
{% endhighlight %}



### Missing letters
从传递进来的字母序列中找到缺失的字母并返回它。

如果所有字母都在序列中，返回 undefined。

{% highlight javascript %}
function fearNotLetter(str) {
   var f = str.charCodeAt(0);
   var i = 1;
   var str1 = "";
   while(i < str.length) {
     while(str.charCodeAt(i) !== ++f) {
       str1 += String.fromCharCode(f);
     }
     i++;
   }
  if(str1 === "")
    return undefined;
  return str1;
}

fearNotLetter("abce");
{% endhighlight %}



### Pig Latin
把指定的字符串翻译成 pig latin。

Pig Latin 把一个英文单词的第一个辅音或辅音丛（consonant cluster）移到词尾，然后加上后缀 "ay"。

如果单词以元音开始，你只需要在词尾添加 "way" 就可以了。

{% highlight javascript %}
function translate(str) {
  
  var arr = ["a","e","i","o","u","A","E","I","O","U"];
  var index = 0;
  for(var i = 0; i < str.length; i++) {
    if(arr.indexOf(str[i]) >= 0) {
      index = i;
      break;
    }
  }
    if(index > 0) {
      var word1 = str.substr(0, index);
      var word2 = str.substr(index, str.length - index);
      str = word2 + word1 + "ay";
    } else if(index === 0)
      str += "way";

  return str;
}

translate("california");
{% endhighlight %}



### DNA Pairing

DNA 链缺少配对的碱基。依据每一个碱基，为其找到配对的碱基，然后将结果作为第二个数组返回。

Base pairs[（碱基对）][3] 是一对 AT 和 CG，为给定的字母匹配缺失的碱基。

在每一个数组中将给定的字母作为第一个碱基返回。

例如，对于输入的 GCG，相应地返回 [["G", "C"], ["C","G"],["G", "C"]]

字母和与之配对的字母在一个数组内，然后所有数组再被组织起来封装进一个数组。

{% highlight javascript %}
function pair(str) {
  var word = str.split("");
  var arr1 = ["A", "T", "C", "G"];
  var arr2 = ["T", "A", "G", "C"];
  var index, arrAll=[];
  
  for(var i = 0; i < word.length; i++) {
    index = arr1.indexOf(word[i]);
    var arrOne = [];
    arrOne[0] = word[i];
    arrOne[1] = arr2[index];
    arrAll.push(arrOne);
  }
  
  return arrAll;
}

pair("GCG");
{% endhighlight %}



### All Algorithm-Level
{: .t60 }

{% include list-posts tag='algorithm' %}




 [1]: https://www.freecodecamp.cn/challenges/get-set-for-our-algorithm-challenges
 [2]: http://www.mathsisfun.com/roman-numerals.html
 [3]: https://en.wikipedia.org/wiki/Base_pair
