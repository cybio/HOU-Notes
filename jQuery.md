# jQuery

### 选择器
```
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