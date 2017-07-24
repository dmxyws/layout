# layout    
#### 1. 圣杯布局  
1. HTML结构：
```
	<div id='header'></div>
	<div id='container'>
		<div id='middle'></div>
		<div id='left'></div>
		<div id='right'></div>
	</div>
	<div id='footer'></div>
``` 
2. 设置 container 里面元素的宽度，不设置高度（也可根据需求设置最小高度）；left 和 right 的宽度固定，middle 的宽度为100%
3. middle、left、right 都设置左浮动
4. 由于 middle 独占一行，所以 left 和 right 被挤到了下一行，此时为 left 设置 margin-left: -100%; 即会跟 middle 在同一行，且在行首的位置，然后将right 的 margin-left 的值设为负的自身盒子的宽度，即可与 middle 同在一行且在行末  
5. 为 container 设置 padding: 0 200px（即左右盒子的宽度）; 此时三个盒子被挤到了中间，在两边为 left 和 right 留出了空白
6. 给 left 和 right 开启相对定位，left 的 left 值设置为负的自身宽度大小，right 的 right 值也设置为负的自身宽度大小
7. 等高布局：
```
#middle,#left,#right{
　　padding-bottom: 10000px;
　　margin-bottom: -10000px;
}
```
 

#### 2. 双飞翼布局
1. HTML结构：与圣杯布局的区别在于，在 container 里面加了一个 `<div id='con_inner'></div>` 用于包裹其原来的子元素
2. CSS：container 上的 overflow: hidden 不再是用于解决高度塌陷问题，而是为了实现等高布局效果；给 con_inner 设置 margin: 0 200px;使子元素往中间挤，其他样式与圣杯布局相同；（ps：也可把 container 上的 overflow: hidden 设置给 con_inner）
3. 弊端：（1）.会改变原本的结构；（2）.如果把 overflow: hidden 设置给 container 则 con_inner 上的高度塌陷问题没有解决


#### 3. 等高布局
1. 核心代码
```
#middle,#left,#right{
　　padding-bottom: 10000px;
　　margin-bottom: -10000px;
}
```
2. 原理：
	1. padding-bottom: 10000px; 用于视觉上的“补缺”
	2. margin-bottom: -10000px; 用于视觉上的“等高”或者说是裁剪“补缺”后多余的部分；而 margin-bottom 的值为负，意思是在文档流中腾出一部分空间，如果其后有元素则后面元素会向上靠拢，对比为正值时可把父元素的 content-box（内容区） 的下边距往下挤，此处则是把父元素的内容区下边距往上“拉” ，当父元素内容区下边距在向上运动的过程中遇到其他元素的下边距时则停止运动，即可达到视觉上的等高效果，也就是“伪等高”
