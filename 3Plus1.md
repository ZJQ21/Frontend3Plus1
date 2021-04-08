# Day 1
## 页面导入样式时，使用link和@import有什么区别？
* link是HTML标签, @import 是CSS规则
* link引入的样式可以和页面同时加载 @import引入的样式需要等待页面加载完成后再加载
* link没有兼容性问题, @import ie5下不兼容
* link可以通过js操作动态引入样式, @import不可以
* https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import
* https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link

## 圣杯布局和双飞翼布局的理解和区别，并用代码实现
作者：知乎用户
链接：https://www.zhihu.com/question/21504052/answer/50053054
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

圣杯布局的来历是2006年发在a list part上的这篇文章：In Search of the Holy Grail · An A List Apart Article圣杯是西方表达“渴求之物"的意思，不是一种对页面的形象表达。双飞翼据考源自淘宝UED，应该是一种页面的形象的表达。圣杯布局和双飞翼布局解决的问题是一样的，就是两边顶宽，中间自适应的三栏布局，中间栏要在放在文档流前面以优先渲染。圣杯布局和双飞翼布局解决问题的方案在前一半是相同的，也就是三栏全部float浮动，但左右两栏加上负margin让其跟中间栏div并排，以形成三栏布局。不同在于解决”中间栏div内容不被遮挡“问题的思路不一样：圣杯布局，为了中间div内容不被遮挡，将中间div设置了左右padding-left和padding-right后，将左右两个div用相对布局position: relative并分别配合right和left属性，以便左右两栏div移动后不遮挡中间div。双飞翼布局，为了中间div内容不被遮挡，直接在中间div内部创建子div用于放置内容，在该子div里用margin-left和margin-right为左右两栏div留出位置。多了1个div，少用大致4个css属性（圣杯布局中间divpadding-left和padding-right这2个属性，加上左右两个div用相对布局position: relative及对应的right和left共4个属性，一共6个；而双飞翼布局子div里用margin-left和margin-right共2个属性，6-2=4），个人感觉比圣杯布局思路更直接和简洁一点。简单说起来就是”双飞翼布局比圣杯布局多创建了一个div，但不用相对布局了“，而不是你题目中说的”去掉relative"就是双飞翼布局“。


### 圣杯布局
* 1: 先写middle,然后是left和right，因为需要先渲染middle
* 2: left、right需设置position:relative以及相应的left、right值
* 3:理解负边距的作用，left的margin-left:-100%使它上移一行，同时right向左移占据left原先位置，同理，right的margin-left:-100px使它上移并靠右

```html
<html>
    <style type="text/css">
	body, html {
	   padding: 0;
	   margin: 0;
	}
        .header {
			width: 100%;
			height: 30px;
			background: red;
		}

		.content {
			overflow: hidden;
			padding: 0 100px;
		}

		.footer {
			width: 100%;
			height: 30px;
			background: red;
		}

		.middle {
			position:relative;			
			width: 100%;
			float: left;
			height: 80px;
			background: green;
		}

		.left {
			position:relative;
			width: 100px;
			float: left;
			left:-100px;
			height: 80px;
			margin-left: -100%;
			background: yellow;
		}

		.right {
			position:relative;			
			width: 100px;
			float: left;
			right:-100px;
			height: 80px;
			margin-left: -100px;
			background: pink
		}
	</style>
<body>
    <div class="header"></div>
    <div class="content">
		<div class="middle"></div>
		<div class="left"></div>
		<div class="right"></div>
	</div>
	<div class="footer"></div>
</body>
</html>
```
### 双飞翼布局
跟圣杯布局没多大区别，就是middle的实现不一样，圣杯布局是middle+padding，双飞翼采用子元素+margin，最主要的还是负边距的使用
```html
<html>
    <style type="text/css">
	body, html {
	   padding: 0;
	   margin: 0;
	}
               .header {
			width: 100%;
			height: 30px;
			background: red;
		}

		.content {
			overflow: hidden;
		}

		.footer {
			width: 100%;
			height: 30px;
			background: red;
		}

		.middle {			
			width: 100%;
			float: left;
		}
        .inner-middle{
			width:100%;
			height: 80px;
			
			background: green;			
		}
		.left {
			width: 100px;
			float: left;
			height: 80px;
			margin-left: -100%;
			background: yellow;
		}

		.right {			
			width: 100px;
			float: left;
			height: 80px;
			margin-left: -100px;
			background: pink
		}
	</style>
<body>
        <div class="header"></div>
	<div class="content">
		<div class="middle">
			<div class="inner-middle"></div>
		</div>
		<div class="left"></div>
		<div class="right"></div>
	</div>
	<div class="footer"></div>
</body>
</html>
```

## 用递归算法实现，数组长度为5且元素的随机数在2-32间不重复的值
```javascript
function random5(source, dest, limit) {
    if (dest.length >= limit) {
        return
    }

    var rand = Math.floor(Math.random() * source.length)
    var number = source.splice(rand, 1)
    dest.push(number[0])
    random5(source, dest, limit)
}

var source = []
for (var i = 2; i <= 32; ++i) {
    source.push(i)
}

var dest = []
random5(source, dest, 5)
return
```