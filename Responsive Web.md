## 响应式Web设计
### 响应式web概念
> **响应式网站是一个设计概念,它是多项技术的综合体.**

* flexible grid layout 弹性网格布局
* flexible image 弹性图片
* media queries 媒体查询

### 响应式web的优点
* 减少工作量
  1. 网站,设计,代码,内容都只需要一份
  2. 多出来的工作量只是JS脚本,CSS样式做一些改动
* 节省时间
* 每个设备都能得到正确的设计
* 搜索优化

### 响应式web的缺点
* 会加载更多的样式和脚本资源
* 设计比较难精确定位和控制
* 老版本浏览器兼容性不好

### 需要掌握的技术
#### 国内外主流浏览器
浏览器|内核|JS引擎
:-:|:-:|:-:
IE|Trident|Jscript=>Chakra
Edge|EdgeHTML|Chakra
Firefox|Gecko|SpiderMonkey
Safari|Webkit|SquirrelFish Extreme
Opera|Presto=>Blink|Carakan
Chrome|Webkit=>Blink|V8
QQ/微信|X5|

### 媒体查询
CSS2
```html
<link rel="stylesheet" type="text/css" href="site.css"
	media="screen" />

<link rel="stylesheet" type="text/css" href="print.css"
	media="print" />
```

CSS3
```css
@media all and (min-width: 800px) 
	and (orientation: landscape) {
	...
}	
```
逻辑操作符
not and only 逗号(,)等同于or
only最好不要省略
加only，老的浏览器不认识，后面的不会执行，媒体查询不起作用；
不加only，老的浏览器会认识 screen，但是不认识后面的条件，会忽略条件，把媒体查询里面的东西全部应用，可能会导致页面错乱。
```
media="only screen and (min-width: 401px) 
	and (max-width: 600px)"
// 旧浏览器解析为 media="only"
media="screen and (min-width: 401px) 
	and (max-width: 600px)"
// 旧浏览器解析为 media="screen"
```
CSS3媒体属性简介
* width 视口宽度
* height 视口高度
* device-width 渲染表面的宽度,就是设备屏幕的宽度
* device-height 渲染表面的高度,就是设备屏幕的高度
* orientation 检查设备处于横向还是纵向
* aspect-ratio 基于视口的宽高比
* device-aspect-ratio 设备屏幕宽高比
* color 每种颜色的位数bits 如 min-color: 16位,8位
* resolution 检测屏幕或打印机的分辨率,如 min-resolution: 300dpi

以上属性都可以添加 min- 或 max- 前缀
### viewport 视口
* 布局视口 layout viewport 页面实际大小
* 可视视口 visual viewport 由用户手动调节的设备窗户比例
* 理想视口 ideal viewport 布局视口在一个设备上的最佳尺寸

>理想视口就是为构建手机浏览器优化的页面而添加的

告诉设备让布局视口成为理想视口
```html
<meta name="viewport" content="width=device-width"/>
```
很多网站直接禁用了用户缩放,默认放大倍数为1
如,百度
```html
<meta name="viewport" content="width=device-width,
	minimum-scale=1.0, maximum-scale=1.0, 
	user-scalable=no"/>
```
### 网站开发前的工作
1. 需求调研
2. 原型设计
3. UI/UE设计,评审

##### 怎样分析设计图
和设计师交流网站如何交互
是否有相应的设计规范(字体 颜色 字号 间距等)
什么地方必须100%还原,什么地方可以灵活处理
分析结构(头部,导航,主体,页脚等),代码复用
不同设备的布局分析