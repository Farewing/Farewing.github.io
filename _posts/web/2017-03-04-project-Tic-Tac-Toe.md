---
layout: page
title: Build a Tic Tac Toe Game
teaser: "Advanced Front End Development Projects of freeCodeCamp(三)."
categories:
    - web
tags:
    - project
---

### Tic Tac Toe

目标: 在 [CodePen.io][1] 上做一个类似于 [FreeCodeCamp clock][2] 的APP。

功能：可以和计算机对手进行 井字游戏.

功能:  游戏会在结束时重置并立即开始新的一局.

功能: 可以自己选择自己的角色是 X 还是 O.

![game](/images/game.png)

![game1](/images/game1.png)

[界面参考][http://codepen.io/freeCodeCamp/full/adBpvw]


### JavaScript
{% highlight javascript %}
var player = 0, flag = 0;
var cells = [];
var model = [];

function newGame() {
	console.log("newGame() is run...");
	$('#myModal').modal({
					show: true,
					backdrop: true,
					keyboard: false
				});
	for(i = 0; i < 3; i++) {
		for(j = 0; j < 3; j++) {
			model[i][j] = 0;
			cells[i][j] = "#panel" + i + j;
			$(cells[i][j]).text(" ");
		}
	}
}

function pickXO() {
	console.log("pickXO() is run...");
	$("#xButton").on("click", function() {
		player = -1;	
		//select X first
	});
	$("#oButton").on("click", function() {
		player = 1;
		console.log("selected " + player);
		//select O second
	});
	$("myModal").modal("hide");
}

function checkDraw() {
	var i, j;
	for(i = 0; i < 3; i++) {
		for(j = 0; j < 3; j++) {
			if(model[i][j] == 0) 
				return false;
		}
	}
	return true;
}

function checkWin() {
	var count = 0;
	var i, j;
	for(i = 0; i < 3; i++) {
		for(j = 0; j < 3; j++) {
			count += model[i][j];
		}
		if(count == 3 || count == -3)
			return count / 3;
		else
			count = 0;
	}

	for(i = 0; i < 3; i++) {
		for(j = 0; j < 3; j++) {
			count += model[j][i];
		}
		if(count == 3 || count == -3)
			return count / 3;
		else
			count = 0;
	}

	count = model[0][0] + model[1][1] + model[2][2];
	if(count == 3 || count == -3)
		return count / 3;

	count = model[0][2] + model[1][1] + model[2][0];
	if(count == 3 || count == -3)
		return count / 3;
	return 0;
}

function AI(f) {
	console.log("AI() is run... and f is " + f);
		if(attack(f)){

		}else if (defend(0 - f)) {

		}else if (center(f)) {

		}else
			planB(f);
	flag = 1;
}

function attack(f) {
	console.log("attack() is run... and f is " + f);
	return twoP(f, 0);
}
 
function defend(f) {
	console.log("defend() is run... and f is " + f);
	return twoP(f, 1);
}

function drop(f, x, y) {
	var tag = (f == -1) ? "X" : "O";
	$(cells[x][y]).text(tag);
	model[x][y] = f;
	console.log("drop() is run... and f is " + f + " ... " + " point is ( " + x + ", " + y + " ) " + model[x][y]);

} //落子

function twoP(f, t) {
	console.log("twoP() is run... and f is " + f);
	var i;
	var e = (t == 0) ? f : (0 - f);

	for (i = 0; i < 3; i++) {
		if(model[i][0] == model[i][1] && model[i][0] == f && model[i][2] == 0) {
			drop(e, i, 2);
			return true;
		}
		else if(model[i][0] == model[i][2] && model[i][2] == f && model[i][1] == 0) {
			drop(e, i, 1);
			return true;
		}
		else if(model[i][2] == model[i][1] && model[i][1] == f && model[i][0] == 0) {
			drop(e, i, 0);
			return true;
		}
		else if(model[0][i] == model[1][i] && model[0][i] == f && model[2][i] == 0) {
			drop(e, 2, i);
			return true;
		}
		else if(model[0][i] == model[2][i] && model[0][i] == f && model[1][i] == 0) {
			drop(e, 1, i);
			return true;
		}
		else if(model[2][i] == model[1][i] && model[1][i] == f && model[0][i] == 0) {
			drop(e, 0, i);
			return true;
		}
	}

	if(model[0][0] == model[1][1] && model[1][1] == f && model[2][2] == 0) {
		drop(e, 2, 2);
		return true;
	}
	else if(model[2][2] == model[1][1] && model[1][1] == f && model[0][0] == 0) {
		drop(e, 0, 0);
		return true;
	}
	else if(model[2][2] == model[0][0] && model[0][0] == f && model[1][1] == 0) {
		drop(e, 1, 1);
		return true;
	}
	else if(model[2][0] == model[1][1] && model[1][1] == f && model[0][2] == 0) {
		drop(e, 0, 2);
		return true;
	}
	else if(model[0][2] == model[1][1] && model[1][1] == f && model[2][0] == 0) {
		drop(e, 2, 0);
		return true;
	}
	else if(model[0][2] == model[2][0] && model[0][2] == f && model[1][1] == 0) {
		drop(e, 1, 1);
		return true;
	}
	else 
		return false;
}//确定连线

function center(f) {
	console.log("center() is run... and f is " + f + "......" + model[1][1]);
	var rand, x, y, e = 0 - f;
	var i = 100;
	function pos() {
		rand = parseInt(Math.random() * 4);
		switch(rand) {
			case 0:
				x = 0;
				y = 0;
				break;
			case 1:
				x = 0;
				y = 2;
				break;
			case 2:
				x = 2;
				y = 0;
				break;
			case 3:
				x = 2;
				y = 2;
				break;
		}
	}

		if(model[0][0] == f && model[2][2] == 0) {
			drop(f, 2, 2);
			return true;
		}
		else if(model[0][2] == f && model[2][0] == 0) {
			drop(f, 2, 0);
			return true;
		}
		else if(model[2][0] == f && model[0][2] == 0) {
			drop(f, 0, 2);
			return true;
		}
		else if(model[2][2] == f && model[0][0] == 0) {
			drop(f, 0, 0);
			return true;
		}
		else if(model[0][0] == model[2][2] && model[0][0] == f) {
			if(model[2][0] == 0) {
				drop(f, 2, 0);
				return true;
			}
			else if(model[0][2] == 0){
				drop(f, 0, 2);
				return true;
			}
		}
		else if(model[0][2] == model[2][0] && model[0][2] == f) {
			if(model[0][0] == 0) {
				drop(f, 0, 0);
				return true;
			}
			else if(model[2][2] == 0){
				drop(f, 2, 2);
				return true;
			}
		}	
	while(i > 0) {
		pos();
		if(model[x][y] == 0) {
			drop(f, x, y);
			return true;
		}
		i--;
	}
	if(model[1][1] == 0) {
		drop(f, 1, 1);
		console.log("drop at centerrrrrrrr...." + model[1][1]);
		return true;
	}
	else
		return false;
}//中间空位

function planB(f) {
	console.log("planB() is run... and f is " + f);
	var e = 0 - f;
	if(model[0][0] == model[2][2] && model[0][0] == e || 
	   model[0][2] == model[2][0] && model[0][2] == e) {
		if(model[0][2] == 0) 
			drop(f, 0, 2);
		else
			drop(f, 0, 0);
	}
	else if((model[0][1] == e || model[1][2] == e) && model[2][0] == 0) {
		drop(f, 2, 0);
	}
	else if((model[1][0] == e || model[2][1] == e) && model[0][2] == 0) {
		drop(f, 0, 2);
	}

	else if(model[0][0] == 0)
		drop(f, 0, 0);
	else if(model[0][2] == 0)
		drop(f, 0, 2);
	else if(model[2][0] == 0)
		drop(f, 2, 0);
	else if(model[2][2] == 0)
		drop(f, 2, 2);
	else if(model[0][1] == 0)
		drop(f, 0, 1);
	else if(model[1][0] == 0)
		drop(f, 1, 0);
	else if(model[1][2] == 0)
		drop(f, 1, 0);
	else
		drop(f, 2, 1);

}

$(document).ready(function() {
	var i, j;
	//$('#myModal').modal("show");}
	for(i = 0; i < 3; i++) {
		model[i] = [];
		cells[i] = [];
	}	

	newGame();
	
	$("#xButton").on("click", function() {
		player = -1;
		flag = 1;
		//AI(0 - player);
	});
	$("#oButton").on("click", function() {
		player = 1;
		flag = 0;
		setTimeout(function() {
			if(flag == 0) {
				AI(0 - player);
			}
		}, 500);
		
	});
	$("#sButton").on("click", function() {
		flag = 3;
		for(i = 0; i < 3; i++) {
			for(j = 0; j < 3; j++) {
				model[i][j] = 0;
				cells[i][j] = "#panel" + i + j;
				$(cells[i][j]).text(" ");
			}
		}
	});
	$("#gButton").on("click",function(){
		setTimeout(function() {
			newGame();
		}, 1000);
		
	});

	// if(player == -1){
	// 	flag = 1;
	// }

	// console.log(player);
	// if( player != 0) {
	// 	console.log("first step xxxxxxxxxxxxxxxxxxxxxxxxxxxxx" + player);
	// 	AI(0 - player);
		
	// }

	for(i = 0; i < 3; i++) {
		for(j = 0; j < 3; j++) {
			(function(i, j) {
				$(cells[i][j]).on("click", function() {
					
					if(flag == 1 && model[i][j] == 0) {
						
						if(player == -1)
							$(cells[i][j]).text("X");
						else
							$(cells[i][j]).text("O");
						model[i][j] = player;
						console.log("player drop atttttttttttt " + i + "...." + j + " ..... " + model[i][j]);	
						flag = 0;	

						if(checkWin() == player) {
							flag = 3;
							console.log("flag is " + flag);
							setTimeout(function() {
								$("#over1").text("厉害了 ！");
								$("#over2").text("再赢一局我吃屎T^T。")
								gameOver();
							}, 500);
							
						}

						if(checkDraw()) {
							flag = 3;
							setTimeout(function() {
								$("#over1").text("算你运气好");
								$("#over2").text("棋盘再大点你就输了(╰_╯)")
								gameOver();
							}, 500);
							//newGame();
						}

						setTimeout(function() {
							if(flag == 0) {
								AI(0 - player);
							}		
							setTimeout(function() {
								if(checkWin() == (0 - player)) {
									flag = 3;
									console.log("chekchekchekchekchekchekchekchekchekchekchekchek");
									$("#over1").text("哈哈哈 笨蛋");
									$("#over2").text("程序都赢不了蠢死了呢(^ω^)")
									gameOver();
								}
							}, 400);
						}, 400);
							
					}
				});

			})(i, j); //闭包 
	
		}
	}
	
});

function gameOver() {
	console.log("gameOver() is run...");
	$('#hhModal').modal({
					show: true,
					backdrop: true,
					keyboard: false
				});
}
{% endhighlight %}


[项目源码][3]


### All Javascript Project
{: .t60 }

{% include list-posts tag='project' %}

[1]: http://codepen.io/
[2]: http://codepen.io/freeCodeCamp/full/adBpvw
[3]: https://github.com/Farewing/Tic-Tac-Toe