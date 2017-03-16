---
layout: page
title: Build a Pomodoro Clock
teaser: "Advanced Front End Development Projects of freeCodeCamp(二)."
categories:
    - web
tags:
    - project
---

### A Pomodoro Clock

目标: 在 [CodePen.io][1] 上做一个类似于 [FreeCodeCamp clock][2] 的APP。

功能：可以启动一个 25 分钟的番茄钟, 计时器将在 25 分钟后停止。

功能: 可以重置番茄钟的状态以便启动下一次计时。

功能: 可以为每个番茄钟自定义时长。

![clock](/images/clock.png)

其中前端采用Bootstrap框架，图形和逻辑处理应用d3库，和JQuery库。

齿轮旋转部分代码为d3库[例子][3]，代码如下：

### HTML
{% highlight html %}
<!DOCTYPE html>
<meta charset="utf-8">
<style>

body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  width: 960px;
  height: 500px;
  position: relative;
}

form {
  position: absolute;
  top: 1em;
  left: 1em;
}

path {
  fill-rule: evenodd;
  stroke: #333;
  stroke-width: 2px;
}

.sun path {
  fill: #6baed6;
}

.planet path {
  fill: #9ecae1;
}

.annulus path {
  fill: #c6dbef;
}

</style>
<form>
  <input type="radio" name="reference" id="ref-annulus">
  <label for="ref-annulus">Annulus</label><br>
  <input type="radio" name="reference" id="ref-planet" checked>
  <label for="ref-planet">Planets</label><br>
  <input type="radio" name="reference" id="ref-sun">
  <label for="ref-sun">Sun</label>
</form>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>

var width = 960,
    height = 500,
    radius = 80,
    x = Math.sin(2 * Math.PI / 3),
    y = Math.cos(2 * Math.PI / 3);

var offset = 0,
    speed = 4,
    start = Date.now();

var svg = d3.select("body").append("svg")
    .attr("width", width)
    .attr("height", height)
  .append("g")
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")scale(.55)")
  .append("g");

var frame = svg.append("g")
    .datum({radius: Infinity});

frame.append("g")
    .attr("class", "annulus")
    .datum({teeth: 80, radius: -radius * 5, annulus: true})
  .append("path")
    .attr("d", gear);

frame.append("g")
    .attr("class", "sun")
    .datum({teeth: 16, radius: radius})
  .append("path")
    .attr("d", gear);

frame.append("g")
    .attr("class", "planet")
    .attr("transform", "translate(0,-" + radius * 3 + ")")
    .datum({teeth: 32, radius: -radius * 2})
  .append("path")
    .attr("d", gear);

frame.append("g")
    .attr("class", "planet")
    .attr("transform", "translate(" + -radius * 3 * x + "," + -radius * 3 * y + ")")
    .datum({teeth: 32, radius: -radius * 2})
  .append("path")
    .attr("d", gear);

frame.append("g")
    .attr("class", "planet")
    .attr("transform", "translate(" + radius * 3 * x + "," + -radius * 3 * y + ")")
    .datum({teeth: 32, radius: -radius * 2})
  .append("path")
    .attr("d", gear);

d3.selectAll("input[name=reference]")
  .data([radius * 5, Infinity, -radius])
    .on("change", function(radius1) {
      var radius0 = frame.datum().radius, angle = (Date.now() - start) * speed;
      frame.datum({radius: radius1});
      svg.attr("transform", "rotate(" + (offset += angle / radius0 - angle / radius1) + ")");
    });

d3.selectAll("input[name=speed]")
    .on("change", function() { speed = +this.value; });

function gear(d) {
  var n = d.teeth,
      r2 = Math.abs(d.radius),
      r0 = r2 - 8,
      r1 = r2 + 8,
      r3 = d.annulus ? (r3 = r0, r0 = r1, r1 = r3, r2 + 20) : 20,
      da = Math.PI / n,
      a0 = -Math.PI / 2 + (d.annulus ? Math.PI / n : 0),
      i = -1,
      path = ["M", r0 * Math.cos(a0), ",", r0 * Math.sin(a0)];
  while (++i < n) path.push(
      "A", r0, ",", r0, " 0 0,1 ", r0 * Math.cos(a0 += da), ",", r0 * Math.sin(a0),
      "L", r2 * Math.cos(a0), ",", r2 * Math.sin(a0),
      "L", r1 * Math.cos(a0 += da / 3), ",", r1 * Math.sin(a0),
      "A", r1, ",", r1, " 0 0,1 ", r1 * Math.cos(a0 += da / 3), ",", r1 * Math.sin(a0),
      "L", r2 * Math.cos(a0 += da / 3), ",", r2 * Math.sin(a0),
      "L", r0 * Math.cos(a0), ",", r0 * Math.sin(a0));
  path.push("M0,", -r3, "A", r3, ",", r3, " 0 0,0 0,", r3, "A", r3, ",", r3, " 0 0,0 0,", -r3, "Z");
  return path.join("");
}

d3.timer(function() {
  var angle = (Date.now() - start) * speed,
      transform = function(d) { return "rotate(" + angle / d.radius + ")"; };
  frame.selectAll("path").attr("transform", transform);
  frame.attr("transform", transform); // frame of reference
});

</script>
{% endhighlight %}


[项目源码][4]


### All Javascript Project
{: .t60 }

{% include list-posts tag='project' %}

[1]: http://codepen.io/
[2]: http://codepen.io/FreeCodeCamp/full/VemPZX
[3]: https://bl.ocks.org/mbostock/1353700
[4]: https://github.com/Farewing/Pomodoro-Clock