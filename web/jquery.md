# jQuery

### 选择器

```JavaScript
:first
:first-child
:first-of-type
:nth-child(n)
:nth-of-type(n)
:only-child
:only-of-type
:even
:odd
parent > child //$("div > p")
parent descendant //$("div p")
element + next //$("div + p")
element ~ siblings //$("div ~ p")
:eq(index)
:button //所有带有 type="button" 的 input 元素
```

### 效果方法 ==（可以用来实现 显示/隐藏 元素）==
- hide([speed,callback]) 和 show([speed,callback])

  可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是隐藏或显示完成后所执行的函数名称。

- slideDown([speed,easing,callback])、slideUp([speed,easing,callback])

- fadeIn([speed,easing,callback])、fadeOut([speed,easing,callback])


### 设置内容和属性

- 设置内容 - text()、html() 以及 val()

  text()、html() 以及 val()，同样拥有回调函数。回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。

- 设置属性 - attr()

  attr()，也提供回调函数。回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。
  
  attr() 方法也允许您同时设置多个属性。
  

```JavaScript
$("#btn1").click(function(){
    $("#test1").text(function(i,origText){
        return "旧文本: " + origText + " 新文本: Hello world! (index: " + i + ")"; 
    });
});

$("button").click(function(){
  $("#runoob").attr("href", function(i,origValue){
    return origValue + "/jquery"; 
  });
});

$("button").click(function(){
    $("#runoob").attr({
        "href" : "http://www.runoob.com/jquery",
        "title" : "jQuery 教程"
    });
});
```
  

### 添加元素
- append() prepend() 是在选择元素内部嵌入。
- after() before() 是在元素外面追加。
- appendTo() prependTo()
- insertAfter() insertBefore()
```
$("<span>Hello World!</span>").appendTo("p");//p元素内容后插入span => <p>原来的内容<span>Hello World!</span></p>

$(content).appendTo(selector)
1. content	必需。规定要插入的内容（必须包含 HTML 标签）。
注意：如果 content 是已存在的元素，它将从当前位置被移除，并在被选元素的结尾被插入。
2. selector	必需。规定把内容追加到哪个元素上。

$(content).insertBefore(selector)
1. content	必需。规定要插入的内容（必须包含 HTML 标签）。
注意：如果 content 是已存在的元素，它将从它的当前位置被移除，并被插入在被选元素之前。
2. selector	必需。规定在何处插入内容。
```

### 删除元素
- remove() 方法删除被选元素及其子元素。
  
  remove() 方法也可接受一个参数，允许对被删元素进行过滤，该参数可以是任何 jQuery 选择器的语法。

  $("p").remove(".italic");//删除 class="italic" 的所有 p 元素
  
- empty() 方法删除被选元素的子元素。

### 尺寸方法
- width() height() 

返回元素的宽度、高度（不包括内边距、边框或外边距）

- innerWidth() innerHeight()

返回元素的宽度、高度（包括内边距、不包括边框或外边距）

- outerWidth() outerHeight()

返回元素的宽度、高度（包括内边距和边框）

outerWidth(ture) 返回元素的宽度、高度（包括内边距和边框和外边距）

![image](http://www.runoob.com/images/img_jquerydim.gif)


### 遍历 - 祖先
- parent() 返回被选元素的直接父元素。
- parents() 返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。
- parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素。

  $("span").parentsUntil("div");//返回介于 span 与 div 元素之间的所有祖先元素
  
### 遍历 - 同胞(siblings)
- siblings() 返回被选元素的所有同胞元素。
- next() 返回被选元素的下一个同胞元素。
- nextAll()
- nextUntil()
- prev()
- prevAll()
- prevUntil()
 
### 遍历- 过滤
- first()
- last()
- eq() ,索引号从 0 开始
- filter()
- not() ,与 filter() 相反

### Jsonp(JSON with Padding) 
```JavaScript
<body>
<script type="text/javascript">
    function callbackFunction(result, methodName){}
</script>
<script type="text/javascript" src="http://www.runoob.com/try/ajax/jsonp.php?jsoncallback=callbackFunction"></script>
</body>

//jQuery 使用 JSONP
<script>
    $.getJSON("http://www.runoob.com/try/ajax/jsonp.php?jsoncallback=?", function(data) {});
</script>
```
### 其他

属性 | 描述
---|---
onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。
readyState | 存有 XMLHttpRequest 的状态。
status | 200: "OK" 404: 未找到页面
readyState 从 0 到 4 发生变化。
- 0: 请求未初始化
- 1: 服务器连接已建立
- 2: 请求已接收
- 3: 请求处理中
- 4: 请求已完成，且响应已就绪)

```JavaScript
//ajax
var xmlhttp = new XMLHttpRequest();
xmlhttp.open("POST","/try/ajax/demo_post2.php",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Henry&lname=Ford");
xmlhttp.onreadystatechange = () => {
  if (xmlhttp.readyState==4 && xmlhttp.status==200){
    console.log(xmlhttp.responseText, xmlhttp.responseXML)
  }
}
  
//jQuery处理多个异步事件
$(function () { 
	var d1 = $.Deferred();
	var d2 = $.Deferred();
	var d3 = $.Deferred();
	$.when( d1, d2, d3 ).done(function ( v1, v2, v3 ) {
		alert( v1 ); // v1 is undefined
		alert( v2 ); // v2 is "abc"
		alert( v3 ); // v3 is an array [ 1, 2, 3, 4, 5 ]
	});
	d1.resolve();
	d2.resolve( "abc" );
	d3.resolve( 1, 2, 3, 4, 5 );
})
</script>
```
