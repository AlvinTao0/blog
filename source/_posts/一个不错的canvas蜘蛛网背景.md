---
title: 一个不错的canvas蜘蛛网背景
date: 2016-05-02 09:57:01
tags: canvas
---
经常在搜寻canvas效果的时候,能够看到蜘蛛网效果,而且蜘蛛网还会跟随鼠标的移动有一定的交互效果,很是耐玩;

其实,思考之后,这个效果实现起来并不难:
1. 绘制随机点
2. 随机点的距离小于某值,就连线
3. 在一定范围内,随机点缓慢靠近鼠标位置

先给出我自己做的一个[demo](http://goldtao.github.io/project/canvas%E8%9C%98%E8%9B%9B%E7%BD%91%E8%83%8C%E6%99%AF/%E8%9C%98%E8%9B%9B%E7%BD%91.html),已经很接近网上看到的效果.更多详细请查看[git源码](https://github.com/GoldTao/canvas-demo/tree/master/canvas%E8%9C%98%E8%9B%9B%E7%BD%91%E8%83%8C%E6%99%AF)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>蜘蛛网</title>
	<style type="text/css">
	*{margin:0;}
	body{overflow: hidden;}
	canvas{display: block;}
	</style>
</head>
<body>
	<canvas id="canvas"></canvas>
	<script type="text/javascript">
		var canvas = document.getElementById("canvas");
		var ctx = canvas.getContext("2d");

		canvas.width = document.documentElement.clientWidth;
		canvas.height = document.documentElement.clientHeight;

		var dots=[];
		var dis = 70;

		window.onload=function(e){
			
			for (var i = 0; i < 400; i++) {
				var x = Math.random()*canvas.width;
				var y = Math.random()*canvas.height;
				var vx = (Math.random()*2-1)*1;
				var vy = Math.random()>0.5 ? Math.sqrt(1-vx*vx) : -Math.sqrt(1-vx*vx);
				// var vx = Math.random()>0.5 ? 1 : -1;
				// var vy = Math.random()>0.5 ? 1 : -1;

				var dot = new Dot(x,y,vx,vy);
				dots.push(dot);
			}

			setInterval(function(){
				
				for (var i = 0; i < dots.length; i++) {
					dots[i].move()
				}		
				
				drawScreen();
			},20)
					
		}

		var mouse = {x:null,y:null};
		var isMouseIn = false;
		canvas.onmousemove=function(e){
			isMouseIn = true;
			var e = e || window.event;
			mouse.x = e.clientX;
			mouse.y = e.clientY;
		}
		canvas.onmouseout=function(){
			isMouseIn = false;
			mouse = {x:null,y:null};
		}

		function drawScreen(){
			ctx.clearRect(0,0,canvas.width,canvas.height);

			drawLine()
		}

		function Dot(x,y,vx,vy){
			this.x = x;
			this.y = y;
			this.vx = vx;
			this.vy = vy;
			this.bol = false;
		}

		function drawLine(){
			var dot1;
			var dot2;
			for (var i = 0; i < dots.length; i++) {
				dot1 = dots[i];
				for(var j = i+1; j< dots.length; j++){
					dot2= dots[j];
					if(distance(dot1,dot2)<dis){
						var alpha = (dis-distance(dot1,dot2))/dis+0.1;
						ctx.beginPath();
						ctx.strokeStyle = "rgba(0,0,0,"+alpha+")";
						ctx.lineWidth = 0.2;
						ctx.moveTo(dot1.x,dot1.y);
						ctx.lineTo(dot2.x,dot2.y);
						ctx.stroke();
					}
				}
				if(distance(dot1,mouse)<130 && isMouseIn == true){
					ctx.beginPath();
					ctx.strokeStyle = "#777";
					ctx.lineWidth = 0.2;
					ctx.moveTo(dot1.x,dot1.y);
					ctx.lineTo(mouse.x,mouse.y);
					ctx.stroke();
				}
				ctx.beginPath();
				ctx.fillStyle = "#1E90FF";
				ctx.arc(dot1.x,dot1.y,1,0,Math.PI,false);
				ctx.fill();
			}
		}

		function distance(dot1,dot2){
			return Math.sqrt((dot1.x-dot2.x)*(dot1.x-dot2.x)+(dot1.y-dot2.y)*(dot1.y-dot2.y))
		}


		Dot.prototype.move=function(){
			this.x += this.vx;
			this.y += this.vy;

			if(this.x<=-100){
				this.vx = -this.vx;
			}
			if(this.x >= canvas.width+100){
				this.vx = -this.vx;
			}
			if(this.y <= -100){
				this.vy = -this.vy;
			}
			if(this.y >= canvas.height +100){
				this.vy = -this.vy;
			}

			if(distance(this,mouse)<200 && isMouseIn == true){
				this.bol = true;
				if(this.x<mouse.x){
					this.x += (mouse.x-this.x)*0.01;
				}else{
					this.x -= (this.x-mouse.x)*0.01;
				}
				if(this.y<mouse.y){
					this.y += (mouse.y-this.y)*0.01;
				}else{
					this.y -= (this.y-mouse.y)*0.01;
				}
			}else if(distance(this,mouse)>=200 && distance(this,mouse)<220 && isMouseIn == true){
				if(this.bol == true){
					this.vx = (Math.random()*2-1)*1;
					this.vy = Math.random()>0.5 ? Math.sqrt(1-this.vx*this.vx) : -Math.sqrt(1-this.vx*this.vx);
					this.bol = false;
				} 
			}else if(distance(this,mouse)>=250 && isMouseIn == true){
				this.bol = true;
			}

		}

	</script>
</body>
</html>
```