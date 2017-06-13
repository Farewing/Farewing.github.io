---
layout: page
title: Build a Markdown Previewer
teaser: "React Project (一)"
categories:
    - web
tags:
    - project
---

### A Markdown Previewer

目标: 在 [CodePen.io][1] 上创建一个这样的应用，它的功能类似于这个 [http://codepen.io/FreeCodeCamp/full/obYYqW][1]。

功能：实时预览解析后的Markdown文档。


![clock](/images/markdown.png)
https://farewing.github.io/Markdown-previewer/

此项目为freeCodeCamp中React的第一个实践项目，学习过程中主要参考 [博客][2]。

项目代码： 

{% highlight html %}
<!DOCTYPE html>
<html>
  <head>
  <link href="css/bootstrap.min.css" rel="stylesheet">
  <link href="css/bootstrap-theme.min.css" rel="stylesheet">
  <link rel="stylesheet" type="text/css" href="css/style.css">
    <script src="build/react.js"></script>
    <script src="build/react-dom.js"></script>
    <script src="build/browser.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.6/marked.min.js"></script>
  </head>

  <body>
  <h1>Markdown Previewer</h1>
    <div class="container" id="container">
    </div>

    <script type="text/babel">
    var DisplayText = React.createClass({
      getInitialState: function() {
        return {
        value:'Heading\n=======\n\nSub-heading\n-----------\n \n### Another deeper heading\n \nParagraphs are separated\nby a blank line.\n\nLeave 2 spaces at the end of a line to do a  \nline break\n\nText attributes *italic*, **bold**, \n`monospace`, ~~strikethrough~~ .\n\nShopping list:\n\n  * apples\n  * oranges\n  * pears\n\nNumbered list:\n\n  1. apples\n  2. oranges\n  3. pears\n\nThe rain---not the reign---in\nSpain.\n\n *[Herman Fassett](https://freecodecamp.com/hermanfassett)*' }
      },
      handleChange: function(event) {
          this.setState({value: event.target.value});
      },
      rawMarkup: function(value) {
        var rawMarkup = marked(value, {sanitize: true});
        return { __html: rawMarkup };
    },
       render: function() {
       var value = this.state.value;
       return (
      <div className="row">
        <div className="col-md-6">
          <textarea rows="24" type="text" value={value} onChange={this.handleChange} className="form-control"/>
        </div>
        <div className="col-md-6">
            <span dangerouslySetInnerHTML={this.rawMarkup(value)} />
        </div>
      </div>
      );
  }
});

      ReactDOM.render(<DisplayText />, document.getElementById('container'));
    </script>
  </body>
</html>

{% endhighlight %}

### All Javascript Project
{: .t60 }

{% include list-posts tag='project' %}

[1]: http://www.ruanyifeng.com/blog/2012/06/sass.html
[2]: http://www.ruanyifeng.com/blog/2015/03/react.html
