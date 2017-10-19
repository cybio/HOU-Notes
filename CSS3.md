# CSS3

[CSS 参考](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)  
[CSS Almanac](https://css-tricks.com/almanac/)  
[W3C CSS验证](https://jigsaw.w3.org/css-validator/#validate_by_input)  
[CSS 压缩](https://cssminifier.com/)  
[占位图片1](http://iph.href.lu/400x300)  
![占位图片1](http://iph.href.lu/200x50)  
[占位图片2](https://placeholder.com/)  
![占位图片2](http://via.placeholder.com/200x50)  
[小猫占位图](https://placekitten.com/)  
![小猫占位图](https://placekitten.com/200/300)  



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
* 内联元素没有宽高,大小只和内容有关,__可添加内外边距__.

### 浮动
一个元素浮动了,就脱离标准文档流了,即使是原来是行内元素,也可以设置宽高了
```
span {
	float: left;
	width: 200px;
	height: 200px;
	background-color: orange;
}
```

### 清除浮动
* 给浮动的元素的祖先元素加高度
* clear:both; 消除别人的浮动对自己的影响,这种方法有一个非常大的、致命的问题，因为没有高度,所以margin失效了
* 隔墙法 , 在没有高度的两个容器间做一个有高度且clear:both的div墙
* 内墙法 , 在没有高度的容器里做一个clear:both的内墙div, 容器会自动被浮动内容撑高
* overflow:hidden; 只要给父容器加上这个属性,浮动的子元素可以给父元素撑出高度

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

###### 后代选择器
```
选择了 div类下的所有p元素
.div p {
	color: red;
}
```

###### 交集, 并集选择器, 通配符
```
选择某个元素并且有某个类
h3.class1 {
	color: red;
}
并集
h1, p, div { color: red; }

通配符
* { color: red; }
```

###### 儿子选择器
```
div>p {}  /* 选择第一层子元素.不包括所有后代 (IE7开始兼容) */

ul li:first-child {} /* (IE8开始兼容) */
ul li:last-child {}
ul li:nth-child(n) {}
```

###### 下一个兄弟选择器
```
h3+p {} /* 选中h3后面的第一个p */
```

###### CSS注释

```
/* add CSS here */
```

### CSS优先级与权重
从上到下,后声明的优先级高于先声明的,ID选择器高于类选择器
行内样式高于外部样式

选择器权重 先看ID 再看类 最后看标签  
一个ID,0个类,4个标签  
`#div div ul li a {}`  
权重 = 1,0,4  
如果没有具体选中文字语义标签,那么继承的权重为0,如下  
`#div1 #div2 {}`  权重0  
`#div1 #div2 p {}` 权重2,0,1  
`p {}` 权重0,0,1  

权重问题大总结：  
1） 先看有没有选中，如果选中了，那么以（id数，类数，标签数）来计权重。谁大听谁的。如果都一样，听后写的为准。  
2） 如果没有选中，那么权重是0。如果大家都是0，就近原则。


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

## CSS常用属性

CSS属性有上千种,但是根据20-80原则,常用的也就那么几十种.

文本水平对齐
```
text-align: left | right | center | justify;
```

垂直对齐
```
仅针对内联元素（或 内联块）以及表的单元格有效
vertical-align: top | middle | bottom;
```

行高度
```
推荐使用无单位数值
line-height: 1.2 | 1.4em | 140% | 14px;
```

弹性布局 FlexBox
```
display: flex;	/* 让子元素横向对齐并自动调整大小 */
flex-wrap: wrap;	/* 加上这句,不自动调整子元素大小,一行装不下就会换行 */
```

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

背景图
```
background-image: url(http://www.example.com/bck.png);
background-size: cover;
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
* (min-width: 500px) 宽度大于等于500像素时应用媒体查询
* (max-width: 800px) 宽带小于等于800像素时应用媒体查询  
```
@media only screen and (min-width: 500px) and (max-width: 800px)
{
	p {
		display: none;	/* 将段落隐藏 */
	}
}

<link rel="stylesheet" media="screen and (min-width: 500px)" href="yes.css">
```

### srcset
根据设备和视口选择合适的图片  

语法, srcset 有两种自定义方式，一种使用 x 来区分设备像素比 (DPR)，另一种使用 w 来描述图像的宽度。
将 srcset 设置为与逗号分隔的一连串 filename multiplier 对相等，其中每个 multiplier 必须是后跟 x 的整数。

例如，1x 表示 1 倍显示屏，2x 表示像素密度为两倍的显示屏，如 Apple 的 Retina 显示屏。
```
<img src="image_2x.jpg" srcset="image_2x.jpg 2x, image_1x.jpg 1x" alt="a cool image">
```
将 srcset 设置为与逗号分隔的一连串 filename widthDescriptor 对相等，其中每个 widthDescriptor 都以像素为测量单位， 并且必须是后跟 w 的整数。在这里，widthDescriptor 指定每个图片文件的自然宽度，使浏览器能够根据窗口大小和 DPR 选择要请求的最适当的图片。 （如果没有 widthDescriptor，浏览器需要下载图片才能知道其宽度！）
```
<img src="image_200.jpg" srcset="image_200.jpg 200w, image_100.jpg 100w" alt="a cool image">
```
##### srcset 与 sizes 配合使用的语法
```
<img  src="images/great_pic_800.jpg"
      sizes="(max-width: 400px) 100vw, (min-width: 401px) 50vw"
      srcset="images/great_pic_400.jpg 400w, images/great_pic_800.jpg 800w"
      alt="great picture">
```
```
img {
  transition: width 0.5s;
  width: 50vw;
}
@media screen and (max-width: 250px) {
  img {
    width: 100vw;
  }
}

<img src="small.jpg" srcset="small.jpg 500w, medium.jpg 1000w,
large.jpg 1500w" sizes="(max-width: 250px) 100vw, 50vw" alt="Wallaby">
```

### 关于IE6的BUG
* 不支持高度小于12px的微型盒子(设置字体小于高度即可)
* overflow: hidden; 无法清除浮动(加_zoom: 1; 属性解决)
* 左浮动双倍margin(改为右浮动)
* margin-right对父容器有3px bug(不要用子元素顶父元素,要用父元素padding子元素)

