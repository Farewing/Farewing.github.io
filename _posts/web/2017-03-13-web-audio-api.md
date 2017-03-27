---
layout: page
title: Web Audio API
subheadline: "HTML5 之 Web Audio API"
teaser: "Web Audio API 提供了在Web上控制音频的一个非常有效通用的系统，允许开发者来自选音频源，对音频添加作用，创建可视化音频，应用空间效果等等。"
categories:
    - web
tags:
    - HTML5
---

### Web Audio 概念与使用
Web Audio API 涉及音频环境内的音频操作， 并且已经设计为允许模块化路由。基础的音频操作在音频节点上进行， 它们连接在一起构成音频路由图。多个源 — 不同类型的通道布局 — 支持在单一的环境里。这种模块化设计提供了灵活地创建具有动态效果的复合音频的方法。

音频节点通过它们的输入输出相连，形成从一个或多个源开始的链，经过一个或多个节点，直到一个目地（虽然你可能并不需要一个目地，比如说，只是想可视化音频数据）一个简单、典型的 web audio 工作流是这样的：

* 创建音频环境
* 在音频环境里创建源 — 例如 , 振荡器, 流
* 创建效果节点，例如混响、双二阶滤波器、平移、压缩
* 为音频选择一个目地，例如你的系统扬声器
* 连接源到效果器，以及效果器和目地

下面将按照笔者的学习顺序介绍补充相关内容。

### AudioContext
AudioContext 接口表示由音频模块连接而成的音频处理图，每个模块对应一个 AudioNode。AudioContext 可以控制它所包含的节点的创建，以及音频处理、解码操作的执行。做任何事情之前都要先创建 AudioContext 对象，因为一切都发生在这个环境之中。创建 AudioContext：

{% highlight javascript %}
//跨浏览器
var AudioContext = window.AudioContext || window.webkitAudioContext || false;
if (!AudioContext) {
        alert('Sorry, but the Web Audio API is not supported by your browser.');
    } else {
        var audioCtx = new window.AudioContext();
    }
{% endhighlight %}

### 产生指定频率声音
{% highlight javascript %}
var audioCtx = new window.AudioContext();
var oscillator = audioCtx.createOscillator();
oscillator.frequency.value = 440;
oscillator.connect(audioCtx.destination);
oscillator.start(0);
{% endhighlight %}

[OscillatorNode][1] 接口表示一个正弦波。它是一个 AudioNode 音频处理模块，使用指定频率生成正弦波。通过 oscillator 对象 frequency 属性的 value 值，上面的代码产生了一个440HZ的音。运行代码浏览器就会持续发出440HZ的音。调用 oscillator.start() 停止。其中 start 方法只能调用一次，复用需重新创建对象。

OscillatorNode 可以产生4种波形，通过设置type属性来指定波形的种类。

* sine: 正弦波
* square: 矩形波
* triangle: 三角波
* sawtooth: 锯齿波

### 设置音量
{% highlight javascript %}
var oscillator = audioCtx.createOscillator();
oscillator.frequency.value = 110;
oscillator.type = 'triangle';
oscillator.start(0.0);
var errNode = audioCtx.createGain();
oscillator.connect(errNode);
errNode.gain.value = 0;
errNode.connect(audioCtx.destination);
{% endhighlight %}

[GainNode][2] 通过设置GainNode.gain.value 的值来控制音量，通过设置音量为 0，模仿声音停止。AudioParam 对象有一些可以根据时间值自动调整 value 属性与 defaultValue 属性等值的方法，被称为自动方法(AutomaticMethod)。
linearRampToValueAtTime 是自动方法中的一种，操作value属性的值使其在一定时间内从当前值变化到设定值。可以这样操作 GainNode 的 gain 属性来实现淡入·淡出。Web Audio API 将当前时刻的值保存在了 AudioContext 的 currentTime 属性中，单位为秒。

{% highlight javascript %}
errNode.gain.linearRampToValueAtTime(1, audioCtx.currentTime + 0.05);
{% endhighlight %}

#### 应用上述几个函数制作的小游戏 [Simon game][3]
[源码][4]

### 获取短音频
从本地获取单音文件，Chrome浏览器不支持 Ajax 跨域请求获取本地文件。解决办法，直接在 chrome 快捷键图标右键，点击属性，修改目标（在后面加上–allow-file-access-from-files，注意–前面有一个空格），这样就可以正确访问了。Firefox可以正常获取文件。

{% highlight javascript %}
var xml = new XMLHttpRequest();
xml.responseType = 'arraybuffer';
xml.open('GET', 'media/piano.mp3', true);
xml.onload = function() {
	// 音源ファイルをバイナリデータからデコード
	audioCtx.decodeAudioData(
		xml.response,
		function(_data) {
			data = _data;
		},
		function(e) {
			alert(e.err);
		}
	);
};
xml.send();
{% endhighlight %}

### 改变音调
AudioNode有可以改变自身状态的可变属性。

这次使用的AudioBufferSourceNode，有一个可以改变自身播放速度的playbackRate属性，加快或减慢播放速度时声音的频率会随着改变，利用这个来改变声音的音调。

通过AudioBufferSourceNode的playbackRate属性的value值来增减播放速度，不改变的情况下设定为1。

{% highlight javascript %}
bufferSource.buffer = data;
// 设定为2倍的播放速度，频率会变为之前的2倍，音调会升高
bufferSource.playbackRate.value = 2;
bufferSource.connect(ctx.destination);
bufferSource.start(0);
{% endhighlight %}

由于加快播放速度时，频率会随其等比例增大，因此音调会升高。

{% highlight javascript %}
bufferSource.buffer = data;
// 设定为0.5倍的播放速度，频率会变为之前的一般，音调会降低
bufferSource.playbackRate.value = 0.5;
bufferSource.connect(ctx.destination);
bufferSource.start(0);
{% endhighlight %}

[Demo][6]

### 待续.....


扩展阅读 [博客][5]






[1]: https://developer.mozilla.org/zh-CN/docs/Web/API/OscillatorNode
[2]: https://developer.mozilla.org/en-US/docs/Web/API/GainNode/gain
[3]: hhttps://farewing.github.io/Simon/
[4]: https://github.com/Farewing/Simon
[5]: http://yrq110.me/2017/02/18/20170218-sheng-chord-web-audio-api/
[5]: https://farewing.github.io/Piano/
