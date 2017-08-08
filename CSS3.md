# CSS3

CSS负责为网页提供样式  
* 任何元素都是矩形的
* 盒子模型  
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


[CSS 参考](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)  
[CSS Almanac](https://css-tricks.com/almanac/)  
### 开发者工具DevTools
Chrome (F12/Ctrl+Shift+I)

### CSS规则集

标签选择器
```
p {
	color: blue;
}
```

类选择器
```
.book-summary {
  color: blue;
}
```

id选择器
```
#site-description {
  color: red;
}
```

### CSS注释

```
/* add CSS here */
```