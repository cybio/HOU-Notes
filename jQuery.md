# jQuery

### 选择器
```JavaScript
<script>
  $(document).ready(function() {
	// 添加类
    $("button").addClass("animated bounce"); // 动画回弹效果(需要Animate.css)
	$(".well").addClass("animated shake");	// 动画抖动
	$("#target3").addClass("animated fadeOut");	// 渐隐
	$("body").addClass("animated hinge");	// 折叶效果

	// 移除类
	$("button").removeClass("btn-default");

	// 改变元素样式
	$("#target1").css("color", "red");

	// 改变元素属性
	$("#target1").prop("disabled", true);
	
	// 修改元素里的内容
	$("#target4").html("<em>#target4</em>");
	// 修改元素里的文本
	$("#target4").text("target4");

	// 移除元素
	$("#target4").remove();

	// 移动元素
	$("#target2").appendTo("#right-well");

	// 元素复制拷贝
	$("#target2").clone().appendTo("#right-well");

	// 父元素
	$("#target1").parent().css("background-color", "red");

	// 子元素
	$("#right-well").children().css("color", "orange");

	// 选择子元素含有.target类的第N个元素
	$(".target:nth-child(2)").addClass("animated bounce");
	
	// 选择奇数偶数的类
	$(".target:odd").addClass("animated bounce");
	$(".target:even").addClass("animated shake");

  });
</script>
```

## 追加元素
```JavaScript
$().append(element);	// 追加到所有子元素后面
$().prepend(element);	// 追加到所有子元素前面
```

```
$.toggleClass()	// 切换类
$.next()			// 获取同级的下一个元素
$.attr(attrStr)		// 或者元素某个属性值
$.attr(attrStr, value) // 设置元素某个属性的值			
$.css('font-size', '20px')	// 修改元素的CSS
$.html()	// 获取选择器内的全部html内容
$.html(content) // 设置选择器内的内容
$.text()	// 同上 , 但只获取或设置选择器的文本内容
$.val()		// 获取某元素value的值
$.remove()	// 移除元素
```

```
.insertBefore(target) 	// 把某内容插入到target之前
.insertAfter(target)	// 把某内容插入到target之后
.after(content)	// 在选择器之后插入内容
```
.each()
```
$( "li" ).each(function( index ) {
  console.log( index + ": " + $( this ).text() );
});
```

DOM加载完毕后自动执行函数
```
$(function() {
	// do something
});
```
monitorEvents() Chrome的事件检测器
```
// 在开发者工具中监测元素的事件
monitorEvents(element);
// 取消检测
unmonitorEvents(element);
```

## 事件
```
$(element).on('click', function(event) {
	// do something
});

// 简易方式
$(element).click(function(event) {
	// do something
});
```
### 事件代理
使用事件代理，我们将侦听点击父元素的事件， 并关注那些事件的目标。  
jQuery 的事件代理使用我们一直在使用的相同代码，向 "on" 方法另外又传递了一个参数。  
即使有1000个li,也不必为每个li元素添加监听了,   
使用 jQuery 的事件代理仅在一个 元素 (ul#rooms) 上设置事件侦听器并检查目标元素是否为列表项
```
$( '#rooms' ).on( 'click', 'li', function() {
    ...
});
```