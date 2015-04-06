# 2015阿里巴巴前端实习生在线笔试题

前几天参加了2015阿里巴巴前端实习生在线笔试，发现题目真的挺难的，后来再重新回想的时候，除了本来就不会做的，还发现自己做错了几道题，现在整理一下那些笔试题，这里的笔试题部分是自己的，也有部分是工作室其他小伙伴们提供的。感谢楷豪师兄和闻东师兄的相关指导，感谢工作室其他小伙伴们提供的题目和答案。下面选择题给出的一些解析和答案都是自己上网查过相关资料然后根据自己的理解给出的，不保证全部正确哈，仅作参考作用。

------
## 一、（单项选择）对于下列程序运行结果，符合预期的是
```JavaScript
function f1() {

    console.time('time span');
    
}
function f2() {

    console.timeEnd('time span');
    
}

setTimeout(f1, 100);

setTimeout(f2, 200);

function waitForMs(n) {

    var now = Date.now();
    
    while (Date.now() - now < n) {}
    
}

waitForMs(500);
```
>* A、time apan:700.077ms
>* B、time apan:0.066ms
>* C、time apan:500.077ms
>* D、time apan:100.077ms

<i class="icon-pencil"></i> 解析：

`console.time()`语句和`console.timeEnd()`语句是用来对程序的执行进行计时的。因为`f1`和`f2`被都`setTimeout()`事先设置的定时器装到一个事件队列里面。本来`f1`应该在100ms后就要执行了，但是因为`waitForMs()`占用了线程，而执行`JavaScript`是单线程的，所以就没办法在100ms后执行那个`f1`，所以需要等500ms等`waitForMs()`执行完，然后再执行`f1`和`f2`，这时候`f1`和`f2`就几乎同时执行了。所以应该选时间最短的一项，所以答案应该选`B` **（楷豪师兄提供的解答）**

------

##二、（单项选择）请选择结果为真的表达式
>* A、null instanceof Object
>* B、null == undefined
>* C、NaN == NaN
>* D、false == undefined

<i class="icon-pencil"></i> 解析：

**A、**未定义的值和定义未赋值的为undefined，null是一种特殊的object，所以`typeof null` 返回的应该是`object`，但是为什么`null instanceof Object`返回的是`false`呢？原因就是`null`是个特殊的`Object`类型的值 ，表示空引用的意思 。`instanceof` 表示某个变量是否是某个对象的实例 ,所以为`false` 。

**B、**`undefined == null`是正确的，尽管如此，和其他相似之处，但`null`和`undefined`并不是等价的。每个作为其独特的类型的唯一成员,`undefined`是`Undefined`类型和`null`是`Null`类型。所以`undefined === null`是不正确的，因为他们虽然值相等，但是类型不相等。区分这两个值，可以认为`undefined`代表一个意想不到的没有值而`null`作为预期没有值的代表。

**（null:）**是一个对象，但是为空。因为是对象，所以`typeof null`返回`object` 。`null`是`JavaScript`保留关键字。 
`null`参与数值运算时其值会自动转换为`0`，因此，下列表达式计算后会得到正确的数值： 
表达式：`123 + null` 结果值：`123` 
表达式：`123 * null` 结果值：`0`

**（undefined：）**是全局对象`（window）`的一个特殊属性，其值是未定义的。但`typeof undefined`返回`undefined`。

**C、**`NaN`是一个值类型,同是也是一个数值.意思是`Not A
Number`,这个都知道是什么意思.值比较特殊,特殊在于`NaN`是一个数字,是一个与任何数值都不相等的数字。所以`NaN == NaN`返回`false`。

**D、**`undefined`被转换为布尔值为`false`，`Boolean(undefined)`返回的是`false`，但是`undefined`不等于`false`。所以`false == undefined`返回`false`。

所以最终答案应该为`B`。

------

##三、（单项选择）下面程序的执行结果是
```JavaScript
var name = 'World!';
(function () {
  if (typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();

```
>* A、Goodbye Jack	
>* B、Hello Jack
>* C、Hello undefined
>* D、Hello World

<i class="icon-pencil"></i> 解析：

因为`JavaScript`中的变量的查找是就近原则去寻找`var`定义的变量，当就近没有找到的话就会找外层。题目中因为`if`判段语句`(typeof name === 'undefined')`就近定义的`name`就在其执行完的下一行，所以`name`就被预解析了，实际上可以理解成在`if`判段语句`(typeof name === 'undefined')`上面`var name`这样定义了`name`，但是尚未被赋值。而在它执行完后面再去为`name`赋值`name = 'Jack';`，所以`name`的值是`undefined`。所以`typeof name === 'undefined'`成立，所以判断语句会走`if`成立部分。

所以最终答案应该为`A`。

------

##四、（不定项选择）以下关于DOM事件流的表述哪些是正确的
>* A、事件流包括两个阶段：事件捕获阶段、事件冒泡阶段
>* B、IE跟标准浏览器对于DOM事件流实现不一样
>* C、假设parentEle是childEle的父节点，绑定事件：parentEle.addEventListener("click", fn1, false)和childEle.addEventListener("click", fn2,false),当点击childEle的时候fn1将先于fn2触发
>* D、addEventListener第三个参数true代表支持捕获，false代表不支持捕获

<i class="icon-pencil"></i> 解析：

在`W3C`事件模型中，任何事件会首先被捕获直至到达目标元素然后再冒泡回去。事件流包括`3`个阶段：`事件捕获阶段`、`处于目标阶段`和`事件冒泡阶段`。所以`A`选项是`错`的。Web开发者可以选择将事件处理程序注册在捕获或者冒泡阶段。这可以通过`addEventListener()`方法来实现。如果该方法传入的最后一个参数值为`true`，表示事件处理程序被注册在捕获阶段，如果为`false`表示件处理程序被注册在冒泡阶段。所以`D`选项也是`错`的。

假设有如下程序(`childEle`是`parentEle`的子元素)：

1.`parentEle.addEventListener("click", parentDoSomething, true);`

2.`childEle.addEventListener("click", childEleDoSomething, false);`

如果用户点击子元素`childEle`会发生如下事情：

>a、点击事件开始于`捕获阶段`。它会先查询是否有`childEle`的任何祖先元素在`捕获阶段`绑定了`onclick`事件。

>b、它发现祖先元素1在捕获阶段绑定了`onclick`事件，于是`parentEle.parentDoSomething()`首先被执行。

>c、事件一直查询到目标元素`childEle`都没有再发现别的在`捕获阶段`绑定的`onclick`事件，事件转到它的`冒泡阶段`并执行`childEleDoSomething()`(注册在`childEle`上的在`冒泡阶段`执行的事件处理程序)。

>d、事件再次向上查询并检查是否有任何祖先元素在`冒泡阶段`绑定了`onclick`事件，并没有查询到，所以什么都没有发生。


再看相反的例子：

1.`parentEle.addEventListener("click", parentDoSomething, false);`

2.`childEle.addEventListener("click", childEleDoSomething, false);`

现在如果用户点击`childEle`，下面的事情会按顺序发生：

>a、点击事件发生于`捕获阶段`。事件查询`childEle`是否有任何祖先元素在`捕获阶段`绑定了`onclick`事件并且没有查找到这样的元素。

>b、事件查询到目标元素`childEle`自己。事件转为`冒泡阶段`并执行`childEleDoSomething()`。

>c、事件再次向上查询并检查目标元素是否有任何祖先元素在`冒泡阶段`绑定了`onclick`事件。

>d、它找到了满足条件的`parentEle`，然后执行`parentDoSomething()`。所以选项`C`是`错`的，应该是`fn2`先触发。


因为`IE`没有提供对`事件捕获阶段`的支持，所以`IE`跟`标准浏览器`对于`DOM事件流`实现不一样。
所以最终答案应该为`B`。

------

##五、（不定项选择）通过下面的哪些方法可以获取页面的html元素
>* A、document.getElementById	
>* B、document.getElementsByClassName
>* C、document.querySelector
>* D、document.querySelectorAll

<i class="icon-pencil"></i> 解析：

**A、**页面的`html元`素可以通过`id`获取，具有唯一性，如：`var divObjId = document.getElementsById("test");`。所以`A`是正确的。

**B、**页面的`html`元素可以通过`class`获取，会选择页面上所有`class`名为`test`的所有元素，返回的对象是`NodeList`。如：`var divObjClass = document.getElementsByClassName("test");`。所以`B`是正确的。

**C、D 、**`document.querySelector`只返回匹配的第一个元素，如果没有匹配项，返回`null`。`document.querySelectorAll`返回匹配的元素集合，如果没有匹配项，返回空的`NodeList`(节点数组)。这两个方法都可以接受三种类型的参数：`id(#)`，`class(.)`，`标签`，很像`jquery`的选择器。如：

```JavaScript
var obj = document.querySelector("#id");
var obj = document.querySelector(".classname");
var obj = document.querySelector("div");
var el = document.body.querySelector("style[type='text/css'], style:not([type])");
var elements = document.querySelectorAll("#score>tbody>tr>td:nth-of-type(2)");
var elements = document.querySelectorAll("#id1, #id2, .class1, class2, div a, #list li img");

```
所以`C`和`D`都是正确的。

所以最终的答案是`A,B,C,D`。

------

##六、（不定项选择）下面选项中，对javascript事件的描述不正确的是
>* A、IE使用attachEvent/detachEvent方法来添加和删除事件监听器；w3c使用addEventListener/removeEventListener方法
>* B、IE是将event对象作为参数传递给监听器,w3c事件监听器内使用的是一个全局的Event对象
>* C、IE提供了对事件捕获阶段的支持
>* D、要停止事件的传递，IE的做法是设置event对象的cancelBubble为true，而w3c的做法是设置执行stopPropagation方法

<i class="icon-pencil"></i> 解析：

**A、**`IE`使用`attachEvent/detachEvent`方法来添加和删除事件监听器；`w3c`使用`addEventListener/removeEventListener`方法。这是正确的。

**B、**`IE`事件监听器内使用的是一个全局的`Event`对象，而`w3c`是将`event`对象作为参数传递给监听器。所以`B`是错误的。

**C、**`IE`没有提供对事件捕获阶段的支持。所以`C`也是错误的。

**D、**要想阻止冒泡，在`Microsoft`模型中，需要将事件的`cancelBubble`属性设置为`true`。在`W3C`模型中，需要调用事件的`stopPropagation()`方法。

这两种方法阻止了事件的所有冒泡。如果想解决浏览器兼容问题，可以像下面这样写：

```JavaScript
function doSomething(e){

    e = window.event || e;
    
    e.cancelBubble = true;
    
    if(e.stopPropagation){
    
        e.stopPropagation();
        
    }
    
}
```

在不支持`cancelBubble`属性的浏览器中设置它的值并不会报错。浏览器会忽略它并创建这个属性。当然，它并不能真正地阻止冒泡，但是给它分配值的操作本身是安全的。
所以最终的答案是`B,C`。

------

##七、（单项选择）
```JavaScript
var array1 = [1,2];

var array2 = array1;

array1[0] = array2[1];

array2.push(3);

console.log(array1);

console.log(array2);
```
执行上面的代码`array1`和`array2`的值分别是什么？
>* A、Array1的值为[2,2];Array2的值为[1,2,3]
>* B、Array1的值为[2,2,3];Array2的值为[1,2,3]
>* C、Array1的值为[2,2,3];Array2的值为[2,2,3]
>* D、Array1的值为[1,2,3];Array2的值为[1,2,3]

<i class="icon-pencil"></i> 解析：

数组对象是引用的关系，`array2`改变，`array1`也会改变。`array1`改变，`array2`也会改变。
所以最终的答案是`C`。

------

##八、（不定项选择）有如下代码
```JavaScript
function Test(name,age){
	this.name = name;
this.age = age;
};
Test.prototype = {
	name:'aliyun',
	hasOwnproperty:function(){
	return false;
}
};
var instance = new Test('alibaba',102);
```
以下关于原型链的说法正确的是：
>* A、JavaScript对象有两种不同的属性，一种是对象自身的属性，另一种是继承于原型链上的属性
>* B、instance.name == 'aliyun'为true
>* C、instance.hasOwnproperty('age')结果将是false
>* D、所有对象都继承自Object.prototype

<i class="icon-pencil"></i> 解析：

**A、**`javascript`对象有两种不同的属性来源，一个是对象自身属性，另一是继承于原型链上的属性,所以`A`是正确的。

**B、**因为`instance`是`Test`对象的一个实例，如果我们在该实例中创建了`name`这个属性，这个属性的值将会屏蔽原型中的那个属性。所以`instance.name == 'aliyun'`为`false`，`instance.name`的值应为`alibaba`，所以`B`是错误的。

**C、**因为`instance`是`Test`对象的一个实例，所以同样拥有`hasOwnproperty`这个方法，所以返回的结果是`false`.

**D、**每个`JavaScript`对象都继承一个原型链，而所有原型都终止于`Object.prototype`。注意，这种继承是活动对象之间的继承。它不同于继承的常见概念，后者是指在声明类时类之间的发生的继承。因此，`JavaScript`继承动态性更强。它使用简单算法实现这一点，如下所示：当您尝试访问对象的属性/方法时，`JavaScript`将检查该属性/方法是否是在该对象中定义的。如果不是，则检查对象的原型。如果还不是，则检查该对象的原型的原型，如此继续，一直检查到`Object.prototype`。下图说明了此解析过程

![这里写图片描述](http://img.blog.csdn.net/20150404154242137)

所以`D`是正确的。

所以最终的答案是`A、C、D`。

------

##九、实现函数range([start,]stop[,step])返回一个数组（step大于1）
```JavaScript
> range(1,11); => [1,2,3,4,5,6,7,8,9,10]

> range(0); => []

> range(10); => [0,1,2,3,4,5,6,7,8,9]

> range(0,30,5); => [0,5,10,15,20,25]
```
<i class="icon-pencil"></i> 解析：
实现代码如下：
```JavaScript
function range(){
    
	var argLength = arguments.length,
	
	    newArray = [],
	
	    i = 0,
	
	    start = arguments[0],
	
	    stop = arguments[1],
	
	    step = arguments[2];
	
	switch(argLength){
	
		case 0 : throw Error('至少输入一个参数,限止数组在哪里结束');
		
		case 1 : {
		
		    stop = arguments[0];
		    
		    for( i = 0 ; i < stop ; i++){
		    
		    	newArray.push(i);
		    	
		    }
		    
		    return newArray;
		    
		}
		
		case 2 : {
		
		    for( i = start ; i < stop ; i++){
		    
		    	newArray.push(i);
		    	
		    }
		    
		    return newArray;
		    
		}
		
		case 3 : {
		    
		    if(step < 1) {
		    
		        throw Error('step > 1');
		
		    }else{
		    
			for( i = start ; i < stop ; i += step){
		    
		    		newArray.push(i);
		    		
		    	}
		    	
		    	return newArray;
		    }
		
		}
		
		default : throw Error('最多传入三个参数');
		
	}
	
}
```
------

##十、背景：
>1、对象A直接调用对象B的某个方法，实现交互逻辑。但是导致的问题是A和B紧密耦合，修改B可能导致A调用B的方法失效。
>2、为了解决耦合导致的问题，我们可以设计成：
对象A生成消息 -> 将消息通知给一个消息处理器（Observable）-> 消息处理器将消息传递给B
具体的调用过程变成：
A.emit(‘message’,data); B.on(‘message’,function(data){});
请实现这一事件消息代理功能
//请将事件消息功能补充完整
function EventEmitter(){}

<i class="icon-pencil"></i> 解析：
实现代码如下：
```JavaScript
function EventEmitter() {

	this.eventFunctionMap = {};
	
}

EventEmitter.prototype.emit = function(eventName, data){

	this.eventFunctionMap[eventName].call(this, data);
	
}

EventEmitter.prototype.on = function(eventName, callback) {

	this.eventFunctionMap[eventName] = callback;
	
}
```
------

##十一、实现下图的布局
```html
<main>
    <div>
        A
    </div>
    <div>
        B
    </div>
    <div>
        C
    </div>
</main>
```

![这里写图片描述](http://img.blog.csdn.net/20150404154230487)

<i class="icon-pencil"></i> 解析：
实现代码如下：
```css
div{
	height: 50px;
	width: 50px;
	background-color: black;
	display: inline-block;
	margin-left: 10px;
	color: white;
	font-size: 20px;
	text-align: center;
	line-height: 50px;
}
```
下面是锦欣大神提供的另一种答案：
```css
/**base style**/
div{
  background:#000;
  color:#fff;
  height:50px;
  width:50px;
  text-align:center;
  line-height:50px;
}

/**start here**/
main{
  text-align:center;
}
div{
  display:inline-block;
  _display:inline;/*hank IE*/
  _zoom:0;/*hank IE*/
}
```
------

##十二、有一个包含数据列表的页面，数据行数不确定。每一行数据都有一个删除按钮，单击删除按钮删除该列数据，请用JavaScript实现该功能。
<i class="icon-pencil"></i> 解析：
实现代码如下：
```html
<script type="text/javascript">

window.onload = function(){

	var oUl = document.getElementsByTagName('ul')[0];
	
	oUl.onclick = function(ev){
	
		var ev = ev || window.event;
		
		var target = ev.target || ev.srcElement;
		
		if(target.tagName.toLowerCase() == 'button'){
		
			var tr = target.parentNode;
			
			tr.parentNode.removeChild(tr);
			
		}
		
	}
	
}
</script>
<body> 
	<ul>
		<li>一<button>删除一</button></li>
		<li>二<button>删除二</button></li>
		<li>三<button>删除三</button></li>
		<li>四<button>删除四</button></li>
		<li>五<button>删除五</button></li>
		<li>六<button>删除六</button></li>
	</ul>
</body>
```
------

##十三、编写CSS让一个已知宽高的DIV,在PC/手机端水平垂直居中。
<i class="icon-pencil"></i> 解析：

实现代码如下：
```css
div{
	width : 300px;
	height : 300px;
	border : 1px solid red;
	position : absolute;
	top : 50%;
	left : 50%;
	margin-top : -150px;
	margin-left : -150px;  
}
```

![这里写图片描述](http://img.blog.csdn.net/20150404154352512)

------

##十四、使用语义化的 HTML 标签及css完成以下布局

![这里写图片描述](http://img.blog.csdn.net/20150404154425147)

• 容器默认宽度320px，图片100*100
• hover 时容器宽度变为400px
• 右侧文字宽度自适应，考虑模块化和扩展性
<i class="icon-pencil"></i> 解析：

实现代码如下：
```html
<style type="text/css">
div{
	width : 320px;
}
div:hover{
	width : 400px;
}
img{
	float : left;
	width : 100px;
	height : 100px;
}
h1{
	color : #333;
	margin-bottom : 8px;
	font-size : 20px;
}
p{
	color : #666;
	font-size : 12px;
	line-height: 1.2em;
}
</style>
<div>
	<img src="d:\1.png">
	<h1>(最多两行20px #333,顶部对齐图片，底部间距8px)</h1>
	<p>(12px #666 行高1.2)使用语义化的HTML标签完成以下布局，考虑模块化和扩展性。容器默认为320px，右侧文字宽度自适应</p>
</div>
```
------

##十五、写一个可以暂停执行的JavaScript函数
<i class="icon-pencil"></i> 解析：

####1、使用函数闭包来实现
```html
<input type="button" value="继续" onclick='st();'/>
<script>
/*需要执行的函数*/
function test(x){
    alert(x++);
    alert(x++);
    return function(){
        alert(x++);
    }
}
var st = test(10);
</script>
```
####2、使用setTimeOut来实现
```html
<script language="javascript"> 
/*Javascript中暂停功能的实现 
Javascript本身没有暂停功能（sleep不能使用）同时 vbscript也不能使用doEvents，故编写此函数实现此功能。 
javascript作为弱对象语言，一个函数也可以作为一个对象使用。 
比如： 
function Test(){ 
   alert("hello world"); 
   this.NextStep=function(){ 
      alert("NextStep"); 
   } 
} 
我们可以这样调用 var myTest=new Test();myTest.NextStep();
我们做暂停的时候可以吧一个函数分为两部分，暂停操作前的不变，把要在暂停后执行的代码放在this.NextStep中。 
为了控制暂停和继续，我们需要编写两个函数来分别实现暂停和继续功能。 
暂停函数如下： 
*/
function Pause(obj,iMinSecond){ 
    if (window.eventList==null) window.eventList=new Array(); 
    var ind=-1; 
    for (var i=0;i<window.eventList.length;i++){ 
        if (window.eventList[i]==null) { 
            window.eventList[i]=obj; 
            ind=i; 
            break; 
        } 
    } 
    if (ind==-1){ 
        ind=window.eventList.length; 
        window.eventList[ind]=obj; 
    } 
    setTimeout("GoOn(" + ind + ")",iMinSecond); 
}
/* 
该函数把要暂停的函数放到数组window.eventList里，同时通过setTimeout来调用继续函数。 
继续函数如下： 
*/
function GoOn(ind){ 
    var obj=window.eventList[ind]; 
    window.eventList[ind]=null; 
    if (obj.NextStep) obj.NextStep(); 
    else obj(); 
} 
/* 
该函数调用被暂停的函数的NextStep方法，如果没有这个方法则重新调用该函数。 
函数编写完毕，我们可以作如下测试： 
*/
function Test(){ 
   alert("hellow"); 
   Pause(this,1000);//调用暂停函数  
   this.NextStep=function(){ 
     alert("NextStep"); 
  } 
} 
</script>
```
------

##十六、用JavaScript写一个Ajax的get请求

<i class="icon-pencil"></i> 解析：
```JavaScript
/* 创建 XMLHttpRequest 对象 */
var xmlHttp = null;
function GetXmlHttpObject(){
    if (window.XMLHttpRequest){
      // code for IE7+, Firefox, Chrome, Opera, Safari
      Xmlhttp = new XMLHttpRequest();
    }else{// code for IE6, IE5
      Xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    return xmlhttp;
}
// -----------ajax方法-----------//
function getLabelsGet(){
    xmlHttp = GetXmlHttpObject();
    if (xmlHttp == null){
        alert('您的浏览器不支持AJAX！');
        return;
    }
    var id = document.getElementById('id').value;
    var url="http://timtsang.github.io?id="+id+"&t/"+Math.random();
    xmlHttp.open("GET",url,true);
    xmlHttp.onreadystatechange=favorOK;//发送事件后，收到信息了调用函数
    xmlHttp.send(null);
}
function getOkGet(){
    if(xmlHttp.readyState == 1||xmlHttp.readyState == 2||xmlHttp.readyState == 3){
        // 本地提示：加载中...
    }else if (xmlHttp.readyState == 4){
            if((xmlHttp.status >= 200 && xmlHttp.status < 300) || xmlHttp.status == 304){
                var d= xmlHttp.responseText;
                // 处理返回结果
            }else{
                alert("Request was unsuccessful:" + xmlHttp.status);
            }  
    }
}
```
------

##十七、使用html+css实现以下图形（不能用图片）

![这里写图片描述](http://img.blog.csdn.net/20150404154549977)

<i class="icon-pencil"></i> 解析：

这道题感觉是做的最蛋疼的一道题，因为我之前根本就不理解题目到底要我实现哪个图形，狂刷新了N多遍网页，也没看到什么图形，后来经过闻东师兄提醒之后才恍然大悟，原来真的是要我实现右边那个插入图片的小图标。不过就算当初知道这要实现这个小图标，我也做不出来，因为我根本不知道那两个三角形要怎么实现。
```html
<title>picture</title>
<style type="text/css">
#rectangle{
	background: #FFF;
    border: 10px solid #999999;
    padding:10px;
    height: 120px;
    width: 200px;
}
#circle{
	position:absolute;
	margin-left: 140px;
	margin-top: 10px;
	width: 40px;
	height: 40px;
	background: #999999; 
	-moz-border-radius: 20px; 
	-webkit-border-radius: 20px; 
	border-radius: 20px;
}
#triangle{
	width: 0; 
	height: 0; 
	position:absolute;
	margin-top: 40px;
	border-bottom: 80px solid #999999; 
	border-left: 50px solid transparent; 
	border-right: 70px solid transparent; 
}
#triangle2{
	width: 0; 
	height: 0; 
	position:absolute;
	margin-top: 60px;
	margin-left: 80px;
	border-bottom: 60px solid #999999; 
	border-left: 30px solid transparent; 
	border-right: 70px solid transparent; 
}
</style>
</head>
<div id="rectangle"><span id="circle"></span><span id="triangle"></span><span id="triangle2"></span></div>
</body>
```
效果图如下：

![这里写图片描述](http://img.blog.csdn.net/20150404154532898)

------

##十八、数组去重，然后使数组元素按降序排序
<i class="icon-pencil"></i> 解析：

在排序的算法中，快速排序是相对比较高效的：
```JavaScript
Array.prototype.quickSort = function(){

	if(this.length <= 1){
	
		return this;
		
	}else{
	
		var leftArray = [],
		
		 	rightArray = [],
		 	
		 	num = Math.floor((this.length)/2),
		 	
		 	middleValue = this.splice(num,1);
		 	
		for(var i = 0; i < this.length; i++){
		
			if(this[i] > middleValue){
			
				leftArray.push(this[i]);
				
			}else{
			
				rightArray.push(this[i]);
				
			}
			
		}
		
		return leftArray.quickSort().concat(middleValue,rightArray.quickSort());
		
	}
	
}
```
对于数组去重，有以下三种方法：

第一种：
```JavaScript
Array.prototype.unique1 = function(){

	var newArray = [this[0]];//结果数组
	
	for(var i = 1; i < this.length; i++){
	
		if(newArray.indexOf(this[i]) == -1){
		
			newArray.push(this[i]);
			
		}
		
	}
	
	return newArray;
	
}
```
第二种：
```JavaScript
Array.prototype.unique2 = function(){

	var newArray = [this[0]];//结果数组
	
	for(var i = 1; i < this.length; i++){
	
		if(this.indexOf(this[i]) == i){
		
			newArray.push(this[i]);
			
		}
		
	}
	
	return newArray;
	
}
```
第三种：
```JavaScript
Array.prototype.unique3 = function(){

	var arrayMap = {}, newArray = [];
	
	for(var i = 0; i < this.length; i++){
	
		if(!arrayMap[this[i]]){
		
			arrayMap[this[i]] = true;
			
			newArray.push(this[i]);
			
		}
		
	}
	
	return newArray;
	
}
```
先对数组按降序排序，再去重便可实现题目的要求,这样效率也会高一点：
```JavaScript
var newArray = [1,2,1,9,10,3,4,3,5,2,4,6,5,7,4,8,7,3,5,3];

var nowArray = newArray.quickSort().unique1();

console.log(nowArray);
```
------

##十九、已知：
```JavaScript
var data = {
	name: 'xiaoming',
	age: '18'
};
var template = "My name is {$name}, my age is {$age}".
//写一个函数，实现：
function shenmegui(template, data) {

} => My name is xiaoming, my age is 18.
```
<i class="icon-pencil"></i> 解析：

```JavaScript
var data = {

	name:'xiaoming',
	
	age:'18'
	
};

var template = "My name is {$name},my age is {$age}."

function outputTemplate(template,data){

	var regName = /(\{\$name\})/g;
	
	var regAge = /(\{\$age\})/g;
	
	template = template.replace(regName,data.name);
	
	template = template.replace(regAge,data.age);
	
	return template;
}

alert(outputTemplate(template,data));
```
------

##二十、利用闭包，实现点击&lt;p>时，被点击的p的背景色变为#eee
<i class="icon-pencil"></i> 解析：
```JavaScript
window.onload = function(){

	var pObj = document.getElementsByTagName("p");
	
	for(var i = 0; i < pObj.length; i++){
	
		pObj[i].onclick = (function(){
		
			return function(){
			
				this.style.background = "#eee";
				
			}
			
		})(i);
		
	};
	
}
```
```html
<p>第一个</p>
<p>第二个</p>
<p>第三个</p>
<p>第四个</p>
<p>第五个</p>
```
