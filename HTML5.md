# HTML5

[MDN HTML 元素参考(英)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)  
[MDN HTML 元素参考(中)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)  
[W3C 验证](https://validator.w3.org/)
### 开发者工具DevTools
Chrome (F12/Ctrl+Shift+I)

### 一个标准的 HTML5 文档
```
<!DOCTYPE html>
<html lang="zh-Hans">
<head>
	<meta charset=”utf-8”>
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
	<title>This is Window Title</title>	
	<script src=”scripts.js”></script>
	<style>
		* {
			box-sizing: border-box;
		}
	</style>
	<link rel=”stylesheet” href=”styles.css”>
</head>
<body>
```
* 用\<strong\>代替\<b\> __文本加粗__
* 用\<em\>代替\<i\> _文本强调_
* \<!-- ... --\>  HTML注释

###### 图像,按钮,标题,段落,引用
```
<h1>topic title</h1> <!-- h1~h6 -->

<button name="button">Click me</button>

<img src=”xxx.jpg” alt="required">

<p>hello, world.  <q>this is new.</q><br>
helloworld again.
</p>

<blockquote>
<em>块引用</em>
</blockquote>
```

###### 超链接
```
<a href=”xxx.html” title="title" target="_blank">this is a link.</a>
```

###### 无序列表,有序列表
```
<ol>
	<li>有序列表项</li>
<li>有序列表项</li>
<li>有序列表项</li>
<li>有序列表项</li>
</ol>
<ul>
	<li>无序列表项</li>
	<li>无序列表项</li>
	<li>无序列表项</li>
	<li>无序列表项</li>
</ul>
```

###### 图
```
<!-- Just a figure -->
<figure>
  <img src="https://developer.cdn.mozilla.net/media/img/mdn-logo-sm.png" alt="An awesome picture">
</figure>
<p></p>
<!-- Figure with figcaption -->
<figure>
  <img src="https://developer.cdn.mozilla.net/media/img/mdn-logo-sm.png" alt="An awesome picture">	
  <figcaption>Fig1. MDN Logo</figcaption>
</figure>
```

###### 表单
```
<form action="xxx.php">
	<label><input type="radio" name="indoor-outdoor" checked> Indoor</label>
	<label><input type="radio" name="indoor-outdoor"> Outdoor</label>
	<label><input type="checkbox" name="personality" checked> Loving</label>
	<label><input type="checkbox" name="personality"> Lazy</label>
	<label><input type="checkbox" name="personality"> Energetic</label>
	<input type="text" placeholder="cat photo URL" required>

	<input type="submit">
	<button type="submit">Submit</button>
</form>
```

###### HTML5 新标签
```
<h3>mark</h3>
<p>Do not forget to buy <mark>milk</mark> today.</p>

<h3>progress</h3>
<progress value="77" max="100">
    <span>85</span>%
</progress>

<h3>time</h3>
<p>
    Post date:
    <time datetime="2017-07-29T02:16Z" pubdate>2017-7-29 2:16</time>
</p>

<h3>ruby用来展示东亚文字,rp(不支持ruby时显示括号),rt(注音)</h3>
<ruby>
    叼<rp>(</rp><rt>Diao</rt><rp>)</rp>
    閪<rp>(</rp><rt>Hai</rt><rp>)</rp>
</ruby>

<h3>wbr 软换行,窗口很窄时换行</h3>
如果想学习 Ajax, 那么您必须熟悉 <wbr></wbr>XML Http Request 对象.
<p>http://this<wbr>.is<wbr>.a<wbr>.really<wbr>.long<wbr>.example<wbr>.com/With<wbr>/deeper<wbr>/level<wbr>/pages<wbr>/deeper<wbr>/level<wbr>/pages<wbr>/deeper<wbr>/level<wbr>/pages<wbr>/deeper<wbr>/level<wbr>/pages<wbr>/deeper<wbr>/level<wbr>/pages</p>

<h3>canvas</h3>
<canvas id="myCanvas"></canvas>
<script>
    var canvas = document.getElementById('myCanvas');
    var ctx = canvas.getContext('2d');
    ctx.fillStyle = '#f00';
    ctx.fillRect(0, 0, 302, 100);
</script>

<h3>details</h3>
<details>
    <summary>不要点我</summary>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
        <li>Item 4</li>
    </ul>
</details>

<h3>datalist</h3>
<input type="text" id="myCar" list="cars">
<datalist id="cars">
    <option value="bmw"></option>
    <option value="ford"></option>
    <option value="volvo"></option>
    <option value="cherry"></option>
</datalist>

<h3>output</h3>
<form oninput="result.value=parseInt(a.value)+parseInt(b.value)">
    <input type="range" name="b" value="50" /> +
    <input type="number" name="a" value="10" /> =
    <output name="result"></output>
</form>

<br><br><br><br><br><br><br>
</body>
</html>
```

### 用语义化元素替代div
* header
* nav
* article
* aside
* section
* footer



### HTML5 Global attributes

* data-yourvalue
* hidden
* spellcheck
* contenteditable
* designMode(JS)