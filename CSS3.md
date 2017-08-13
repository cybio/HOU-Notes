# CSS3

[CSS 参考](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)  
[CSS Almanac](https://css-tricks.com/almanac/)  
### 开发者工具DevTools
Chrome (F12/Ctrl+Shift+I)
### CSS重设文件
为兼容各浏览器,导入CSS重设文件  
[Normalize.css](https://necolas.github.io/normalize.css/)  

### 任何元素都是矩形的
### 盒子模型
![Box Model][1]

[1]:https://developer.mozilla.org/files/72/boxmodel%20(1).png
"Box Model"

* box-sizing: border-box;
  * 计算元素大小时,把边框和内边距也计算在内
  * 如果没有设置 box-sizing, 则只计算盒子内容的宽度 
* 浏览器专属前缀
  * -webkit- (Chrome/Safari)
  * -moz- (Firefox)
  * -ms- (IE)

### 块元素,内联元素
* 块元素尽可能的宽,宽度取决于父元素,尽可能的矮,高度取决于内容的多少,没有内容就没有高度.
* 百分比是根据最近的父元素计算.
* 内联元素没有宽高,大小只和内容有关,可添加内外边距.

###### 标签选择器
```
p {
	color: blue;
}
```

###### 类选择器
```
.book-summary {
  color: blue;
}
```

###### id选择器
```
#site-description {
  color: red;
}
```

###### CSS注释

```
/* add CSS here */
```

### CSS优先级
从上到下,后声明的优先级高于先声明的,ID选择器高于类选择器
行内样式高于外部样式
###### 最高优先级保证
```
/* !important */
color: pink !important;
```

###### 颜色表示
```
red
rgb(255, 0, 0)
#ff0000
#f00
```

###### 图片样式示例
```
.pic {
	border: 5px dashed #fa8072;	/* 5px虚线边框 */
	border-radius: 5px;	/* 边框圆角 */
	cursor: pointer;	/* 鼠标样式 */
	box-shadow: 5px 5px 5px 5px rgba(204, 204, 204, 0.8); 
	/* 阴影样式: 水平距离,垂直距离,模糊度,大小,颜色和透明度(0~1) */
}
``` 

### CSS常用属性

CSS属性有上千种,但是根据20-80原则,常用的也就那么几十种.

字体颜色
```
color: red;
```

字体大小
```
font-size: 16px;
```

字体
```
font-family: Lobster, Monosapce; /* 字体自降,如果找不到前一种字体,就使用后面的 */
```

元素边框
```
border-color: red;
border-width: 5px;
border-style: solid;
border: 5px solid red;	/* 简写 */
```

边框半径
```
border-radius: 10px;
border-radius: 50%;
```

背景色
```
background-color: green;
```

内外边距
```
padding-top: 40px;
padding-right: 20px;
padding-bottom: 20px;
padding-left: 40px;
margin-top: 40px;
margin-right: 20px;
margin-bottom: 20px;
margin-left: 40px;
padding: 20px;
padding: 10px 20px 10px 20px;	/* 顺时针方式排列：上右下左 */
margin: 20px;	/* 外边距为负值会让元素变大 */
margin: 0 auto;	/* 水平居中 */
```

溢出
```
overflow: auto; /* 内容溢出时会自动显示滚动条 */
```

### 媒体查询
* @media 表示媒体查询
* only 兼容旧浏览器
* screen 设备针对屏幕(print 设备针对打印机)
* and 条件(也有 or 等等)
* (max-width: 500px) 宽度小于等于500像素时应用媒体查询
```
@media only screen and (max-width: 500px)
{
	p {
		display: none;	/* 将段落隐藏 */
	}
}
```