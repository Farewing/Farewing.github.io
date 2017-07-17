---
layout: page
title: Build a Camper Leaderboard
teaser: "React Project (二)"
categories:
    - web
tags:
    - React project
---

### A Camper Leaderboard

目标: 在 [CodePen.io][1] 上创建一个这样的应用，它的功能类似于这个 [http://codepen.io/FreeCodeCamp/full/qbqqJm/][2]。

功能：我能在表格中看到在过去30天内，Free Code Camp 中谁完成的任务最多。

功能：我能看到他们在过去30内，完成了那些任务，并且知道他们总共完成了哪些任务。

功能：我能够分别对“过去30天完成的任务量” 和 “总共完成的任务量”两列表格进行排序。


![camper](/images/camper.png)


[项目链接][3]


此项目为freeCodeCamp中React的第二个实践项目，重点包括Ajax请求获取数据，根据用户点击展示不同的数据。

项目代码： 

{% highlight html %}
<!DOCTYPE html>
<html>
  <head>

  <link rel="stylesheet" type="text/css" href="css/style.css">
    <script src="build/react.js"></script>
    <script src="build/react-dom.js"></script>
    <script src="build/browser.min.js"></script>
    <script src="build/jquery.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.6/marked.min.js"></script>
  </head>

  <body>
  <div id="container">
    
  </div>
  
  <script type="text/babel">
  var ListNode = React.createClass({
  render: function() {
  var profileLink = 'https://www.freecodecamp.com/' + this.props.info.username;

    return (
      <tr>
        <td>{this.props.id}</td>
        <td>
          <img src={this.props.info.img} className="img-responsive" />
          <span className="spann"><a className="aa" href={profileLink} target='_blank'>{this.props.info.username}</a></span>
        </td>
        <td>{this.props.info.recent}</td> 
                <td>{this.props.info.alltime}</td>
      </tr>
      );
  }
});

var UserList = React.createClass({
  getInitialState: function(){
    return {data: []};
  },
  ajaxRequest: function(){
    $.ajax({
      url: 'https://fcctop100.herokuapp.com/api/fccusers/top/' + this.props.order,
      dataType: 'json',
      cache: false,
      success: function(result) {
        this.setState({data: result});
      }.bind(this),
      error: function(xhr, status, err) {
          console.error(this.props.url, status, err.toString());
          }.bind(this)
    });
  },
  componentDidMount: function() {
      this.ajaxRequest();
    },  
  render: function() {
  this.ajaxRequest();
    var userNodes = this.state.data.map(function(user, idx){
      return (
        <ListNode info={user} id={idx + 1} key={idx} />
      );
    });
    return (
      <tbody>
        {userNodes}
      </tbody>
    );
  }
});

var LeaderBoard = React.createClass({
  getInitialState: function () {
    return {
      order: 'recent'
    }
  },
  componentDidUpdate: function() {
      $(".hidden").hide(); 
      $(".visible").show();
    },  
  render: function(){
    var recentClass = '';
      var allTimeClass = '';

      if(this.state.order == 'recent') {
          recentClass = 'visible';
          allTimeClass = 'hidden';
      }
      else{
          recentClass = 'hidden';
          allTimeClass = 'visible';
      } 
      return (
        <table className="table table-striped">
          <thead>
            <tr>
              <th>#</th>
              <th>Camper Name</th>
              <th><a className="ret" href="#" onClick={this.recentOrder}>Points Last 30 Days <span className={recentClass}>▼</span></a></th>
              <th><a className="all" href="#" onClick={this.allTimeOrder}>All Time Points <span className={allTimeClass}>▼</span></a></th>
            </tr>
          </thead>
          <UserList order={this.state.order} />
        </table>
      );
  },
  recentOrder: function () {
      this.setState ({ order: 'recent' });
      
    },
    allTimeOrder: function () {
      this.setState ({ order: 'alltime' });
    }
});

ReactDOM.render (<LeaderBoard />, document.getElementById ('container'));
$(".hidden").hide();


</script>
</body>
</html>
{% endhighlight %}

### All Javascript Project
{: .t60 }

{% include list-posts tag='React project' %}

[1]: http://codepen.io/FreeCodeCamp/full/qbqqJm/
[2]: http://codepen.io/FreeCodeCamp/full/qbqqJm/
[3]: https://github.com/Farewing/Camper-Leaderboard
