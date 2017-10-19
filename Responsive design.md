## 一些响应式相关的事项

### 设备像素比
硬件像素 除以 DPR像素比 等于CSS像素  

### 元素最大宽度建议
max-width 属性用来给元素设置最大宽度值. 定义了max-width的元素会在达到max-width值之后避免进一步按照width属性设置变大.

max-width 会覆盖width设置, 但 min-width设置会覆盖 max-width.
```
img, embed, object, video {
	max-width: 100%;
}
```

### 小屏幕设备点按目标
48px适合手指点按宽度  
如果只用width,height属性元素不会自动调整  
有可能会导致内容溢出
```
nav a, button {
	min-width: 48px;
	min-height: 48px;
}
```

### 从小设备开始设计
* 优先考虑什么是用户需要的,最想看到的
* 利于兼容性
* 从一开始就思考性能问题

### 间断点
* 利用媒体查询, 设置的页面布局变化的点  
* 没有正确的点位,要以内容为指引,找到合适的间断点  
* 从最小屏幕往大设置断点

### Flexbox
弹性布局 FlexBox
```
display: flex;	/* 让子元素横向对齐并自动调整大小 */
flex-wrap: wrap;	/* 加上这句,不自动调整子元素大小,一行装不下就会换行 */
/* 配合媒体查询,可以设置元素的出现顺序 */
@media screen and (min-width: 700px) {
	.class1 { order: 2; }
	.class2 { order: 3; }
	.class3 { order: 1; }
}
```

### 常见四种响应式布局
大体流动模型  
```
.container {
	display: flex;
	flex-wrap: wrap;
}
子元素初始宽100% (div1, div2, div3)
当设备变大后,在断点处改变子元素的宽度
@media screen and (min-width: 450px) {
	.div1 {
		width: 25%;	
	}
	.div2 {
		width: 75%;
	}
}
@media screen and (min-width: 550px) {
	.div1, .div3 {
		width: 25%;	
	}
	.div2 {
		width: 50%;
	}
}
当宽度最大时,容器保持不变,居中,两边加外边距
@media screen and (min-width: 800px) {
	.container {
		width: 800px;
		margin-left: auto;
		margin-right: auto;
	}
}
```
掉落列模型
```
.container {
	display: flex;
	flex-wrap: wrap;
}
子元素初始宽100% (div1, div2, div3)
当设备变大后,在断点处改变子元素的宽度
@media screen and (min-width: 450px) {
	.div1 {
		width: 25%;	
	}
	.div2 {
		width: 75%;
	}
}
@media screen and (min-width: 550px) {
	.div1, .div3 {
		width: 25%;	
	}
	.div2 {
		width: 50%;
	}
}
```
活动布局模型(布局切换器)  
```
<div class="container">
    <div class="box dark_blue"></div>
    <div class="container" id="container2">
        <div class="box light_blue"></div>
        <div class="box green"></div>
    </div>
    <div class="box red"></div>
</div>

.container {
	width: 100%;
	display: flex;
	flex-wrap: wrap;
}
.box {
	width: 100%;
}

当设备变大后,在断点处改变子元素的宽度
@media screen and (min-width: 500px) {
	.dark_blue {
		width: 50%;	
	}
	#container2 {
		width: 50%;
	}
}
@media screen and (min-width: 600px) {
	.dark_blue {
		width: 25%;	
		order: 1;
	}
	#container2 {
		width: 50%;
	}
	.red {
		width: 25%;
		order: -1;
	}
}
```
画布溢出模型  
```
<nav id="drawer" class="dark_blue"></nav>

<main class="light_blue"></main>
确保主要元素占据整个屏幕
html, body, main {
	height: 100%;
	width: 100%;
}

nav {
	width: 300px;
	height: 100%;
	position: absolute;
	transform: translate(-300px, 0);
	transition: transform 0.3s ease;
}

nav.open {
	transform: translate(0, 0);
}

@media screen and (min-width: 600px) {
	nav {
		position: relative;
		transform: translate(0, 0);
	}
	body {
		display: flex;
		flex-flow: row nowrap;
	}
	main {
		width: auto;
		flex-grow: 1;
	}
}

menu.addEventListener('click', function(e) {
	drawer.classList.toggle('open');
	e.stopPropagation();
});
```

### 响应式表格

##### 隐藏列

在小屏幕上将某些列隐藏(display:none),只显示主要信息  
用媒体查询在屏幕变大以后把隐藏的列显示出来  
如果可能,请尽量使用列缩写,而不是完全隐藏它们  

##### 不再有表格技术(The no more tables technique)

当视窗小于一定值, 表格会瓦解重组成长列表
```
<table>
	<thead>
	    <tr>
	      <th>Team</th>
	      <th>1st</th>
	      <th>2nd</th>
	      <th>3rd</th>
	      <th>4th</th>
	      <th>5th</th>
	      <th>6th</th>
	      <th>7th</th>
	      <th>8th</th>
	      <th>9th</th>
	      <th>Final</th>
	    </tr>
	</thead>
	<tbody>
	    <tr>
	      <td data-th="Team">Toronto</td>
	      <td data-th="1st">0</td>
	      <td data-th="2nd">0</td>
	      <td data-th="3rd">0</td>
	      <td data-th="4th">4</td>
	      <td data-th="5th">0</td>
	      <td data-th="6th">1</td>
	      <td data-th="7th">0</td>
	      <td data-th="8th">0</td>
	      <td data-th="9th">0</td>
	      <td data-th="Final">5</td>
	    </tr>
	    <tr>
	      <td data-th="Team">San Francisco</td>
	      <td data-th="1st">0</td>
	      <td data-th="2nd">0</td>
	      <td data-th="3rd">0</td>
	      <td data-th="4th">4</td>
	      <td data-th="5th">0</td>
	      <td data-th="6th">0</td>
	      <td data-th="7th">0</td>
	      <td data-th="8th">0</td>
	      <td data-th="9th">0</td>
	      <td data-th="Final">4</td>
	    </tr>
	</tbody>
</table>

table {
	border: 1px solid #ddd;
}

tr:nth-child(odd) {
	background-color: #f9f9f9;
}

@media screen and (max-width: 500px) {
	table, thead, tbody, th, td, tr {
		display: block;   /* 将表格各个元素设置成块级元素 */
	}
	thead tr {
		position: absolute;
		top: -9999px;
		left: -9999px;	/* 为使用屏幕阅读器的人有可访问性,不要隐藏,只需移出屏外即可 */
	}
	td { 
  		position: relative;
  		padding-left: 50%; 
	}

	td:before { 
  		position: absolute;
  		left: 6px;
  		content: attr(data-th); /* 获取data-th的值给content */
  		font-weight: bold;
	}
	td:first-of-type {
  		font-weight: bold;
	}
}
```

##### 表格内滚动
将表格放在一个div容器里
```
div.contained_table {
	width: 100%;
	overflow-x: auto;
}
```

### 字体
建议的段落字体大小16px, 行高最少1.2em, 每行推荐60个字符左右.  

### 副断点
使用副断点做一些细微调整, 如字体, 间距, 小图片大小等等.  

### 段落添加省略号
```
.truncate {
  width: 250px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

## 响应式图片
### 相对大小
推荐给图片使用max-width: 100%,好过width: 100%, 这样图片不会随窗口变大而变大.  

##### 给并列的图片加外边距
```
img {
	margin-right: 10px;
	max-width: 426px;
	width: calc((100% - 10px) / 2);
}
/* 使用伪类,找到最后一张图片,不设置右外边距 */
img:last-of-type {
	margin-right: 0;
}
```
##### 鲜为人知的CSS单位
如果想要设置一个图片高度为100%, 得保证body的高度也是100%的.  
一个简单的方法是使用VH单位, 是视口高度的缩写  
```
1vh = 1%视口高度
1vw = 1%视口宽度
1vmin = 1%视口宽高之间最小的那一边
1vmax = 1%视口宽高之间最大的那一边

height: 100vh; /* 视口有多高,图片就有多高 */
```

### 光栅与矢量图
* 光栅图就是一般的照片 (jpg, png)
* 矢量图是由线条,颜色,渐变色等构成的,缩放不影响质量 (svg)

光栅优选jpeg, 矢量选svg格式,不能用svg就用png