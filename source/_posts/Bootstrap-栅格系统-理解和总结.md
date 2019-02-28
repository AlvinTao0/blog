---
title: Bootstrap 栅格系统 理解和总结
date: 2015-12-02 10:41:27
tags: css
---
对Bootstrap中非常重要好用的栅格系统做一个以实例为向导的总结：

### 第一步：创建栅格系统的容器　
```html
<div class="container-fluid">
    <div class="row">
 		  ...
    </div>
</div>
```
解释：为了寄予栅格系统合适的排列和padding，要把每一行“row”包含在一个容器中，而这个容器我们用class名为“container”或者“container-fluid”,这两个class是Bootstrap为我们事先设计好的

.container是固定宽度，居中显示:下面是Bootstrap中.container类的代码
![image](http://o71pfzm86.bkt.clouddn.com/boot2.png)

.container-fluid是 100% 宽度：下面是Bootstrap中.container-fluid类的代码
![image](http://o71pfzm86.bkt.clouddn.com/boot3.png)

### 第二步：创建合适的栅格系统
```html
<div class="row">
 	<div class="col-md-1">.col-md-1</div>
 	<div class="col-md-1">.col-md-1</div>
 	<div class="col-md-1">.col-md-1</div>
 	<div class="col-md-1">.col-md-1</div>
	<div class="col-md-1">.col-md-1</div>
 	<div class="col-md-1">.col-md-1</div>
	<div class="col-md-1">.col-md-1</div>
 	<div class="col-md-1">.col-md-1</div>
 	<div class="col-md-1">.col-md-1</div>
	<div class="col-md-1">.col-md-1</div>
	<div class="col-md-1">.col-md-1</div>
	<div class="col-md-1">.col-md-1</div>
</div>
<div class="row">
	<div class="col-md-8">.col-md-8</div>
 	<div class="col-md-4">.col-md-4</div>
</div>
<div class="row">
 	<div class="col-md-4">.col-md-4</div>
 	<div class="col-md-4">.col-md-4</div>
 	<div class="col-md-4">.col-md-4</div>
</div>
<div class="row">
 	<div class="col-md-6">.col-md-6</div>
 	<div class="col-md-6">.col-md-6</div>
</div>
```

> 解释：
> 1. 上面这段是我从Bootstrap官网复制下来的，每一个“row”代表一行，而内部的“col-md-数字”代表一个单元格；
> 2. Bootstrap把每一行分成12等份，“col-md-数字”中的“数字”从1-12中取，数字等于几，就占几份；
> 3. 合理的选择单元格的数字配置，再往单元格中添加我们想要的内容，这样一个栅格系统就完成了！

### 进阶：单元格的类还有其他选择 ,一共有四种：

- .c0l-xs-　　无论屏幕宽度如何，单元格都在一行，宽度按照百分比设置；试用于手机；
- .col-sm-　　屏幕大于768px时，单元格在一行显示；屏幕小于768px时，独占一行；试用于平板；
- .col-md-　　屏幕大于992px时，单元格在一行显示；屏幕小于992px时，独占一行；试用于桌面显示器；
- .col-lg-　　屏幕大于1200px时，单元格在一行显示；屏幕小于1200px时，独占一行；适用于大型桌面显示器；
 
以上的情况可以通过下面的代码清晰的理解：
```html
<div class="row">
 	<div class="col-xs-12 col-sm-6 col-md-8">.col-xs-12 .col-sm-6 .col-md-8</div>
 	<div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>
<div class="row">
 	<div class="col-xs-6 col-sm-4">.col-xs-6 .col-sm-4</div>
 	<div class="col-xs-6 col-sm-4">.col-xs-6 .col-sm-4</div>
 	<div class="col-xs-6 col-sm-4">.col-xs-6 .col-sm-4</div>
</div>
```

屏幕大于992px时：.col-md-8 和.col-md-4 还处于 作用范围内，如下:
![image](http://o71pfzm86.bkt.clouddn.com/boot6.png)

屏幕在769px-992px之间时：.col-md-已失效，而.col-sm- 还处在 作用范围内，如下:
![image](http://o71pfzm86.bkt.clouddn.com/boot7.png)

屏幕宽度小于768px时，.col-sm-已失效 只有.col-xs- 不受屏幕宽度影响，这时候就由.col-xs-起作用，如下:
![image](http://o71pfzm86.bkt.clouddn.com/boot8.png)