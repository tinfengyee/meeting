## 文章

[Haiya_32ef](https://www.jianshu.com/u/858ef4d71bb6) 博客

[JS中的call、apply、bind方法详解](https://www.cnblogs.com/moqiutao/p/7371988.html)

## for-of/in

不要使用for in遍历数组

[for in 和for of的区别](https://www.jianshu.com/p/c43f418d6bf0)`推荐`

#### 1 遍历数组通常用for循环

ES5的话也可以使用forEach，ES5具有遍历数组功能的还有map、filter、some、every、reduce、reduceRight等，只不过他们的返回结果不一样。但是使用foreach遍历数组的话，使用break不能中断循环，使用return也不能返回到外层函数。

```
Array.prototype.method=function(){
　　console.log(this.length);
}
var myArray=[1,2,4,5,6,7]
myArray.name="数组"
for (var index in myArray) {
  console.log(myArray[index]);
}
```

#### 2 for in遍历数组的毛病

1.index索引为字符串型数字，不能直接进行几何运算
 2.遍历顺序有可能不是按照实际数组的内部顺序
 3.**使用for in会遍历数组所有的可枚举属性**，包括原型。例如上栗的原型方法method和name属性
 所以for in更适合遍历对象，不要使用for in遍历数组。

那么除了使用for循环，如何更简单的正确的遍历数组达到我们的期望呢（即不遍历method和name），ES6中的for of更胜一筹.

```
Array.prototype.method=function(){
　　console.log(this.length);
}
var myArray=[1,2,4,5,6,7]
myArray.name="数组";
for (var value of myArray) {
  console.log(value);
}
```

*记住，for in遍历的是数组的索引（即键名），而for of遍历的是数组元素值。*

for of遍历的只是数组内的元素，而不包括数组的原型属性method和索引name

#### 3 遍历对象

遍历对象 通常用for in来遍历对象的键名

```
Object.prototype.method=function(){
　　console.log(this);
}
var myObject={
　　a:1,
　　b:2,
　　c:3
}
for (var key in myObject) {
  console.log(key);
}
```

for in 可以遍历到myObject的原型方法method,如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性

```
for (var key in myObject) {
　　if（myObject.hasOwnProperty(key)){
　　　　console.log(key);
　　}
}
```

同样可以通过ES5的Object.keys(myObject)获取对象的实例属性组成的数组，不包括原型方法和属性

```
Object.prototype.method=function(){
　　console.log(this);
}
var myObject={
　　a:1,
　　b:2,
　　c:3
}
```

#### 总结

- for..of适用遍历数/数组对象/字符串/map/set等拥有迭代器对象的集合.但是不能遍历对象,因为没有迭代器对象.与forEach()不同的是，它可以正确响应break、continue和return语句
- for-of循环不支持普通对象，但如果你想迭代一个对象的属性，你可以用for-in循环（这也是它的本职工作）或内建的Object.keys()方法：

```
for (var key of Object.keys(someObject)) {
  console.log(key + ": " + someObject[key]);
}
```

- 遍历map对象时适合用解构,例如;

```
for (var [key, value] of phoneBookMap) {
   console.log(key + "'s phone number is: " + value);
}
```

-  **当你为对象添加myObject.toString()方法后，就可以将对象转化为字符串，同样地，当你向任意对象添加myObjectSymbol.iterator方法，就可以遍历这个对象了。**
   举个例子，假设你正在使用jQuery，尽管你非常钟情于里面的.each()方法，但你还是想让jQuery对象也支持for-of循环，你可以这样做：

```
jQuery.prototype[Symbol.iterator] = Array.prototype[Symbol.iterator];
```

所有拥有[Symbol.iterator]()的对象被称为可迭代的。在接下来的文章中你会发现，可迭代对象的概念几乎贯穿于整门语言之中，不仅是for-of循环，还有Map和Set构造函数、解构赋值，以及新的展开操作符。

- for...of的步骤
   or-of循环首先调用集合的[Symbol.iterator]()方法，紧接着返回一个新的迭代器对象。迭代器对象可以是任意具有.next()方法的对象；for-of循环将重复调用这个方法，每次循环调用一次。举个例子，这段代码是我能想出来的最简单的迭代器：

```
var zeroesForeverIterator = {
 [Symbol.iterator]: function () {
   return this;
  },
  next: function () {
  return {done: false, value: 0};
 }
};
```

```js
// for - in 
Object.prototype.method = function () {
  console.log(this);
}
var myObject = {
  a: 1,
  b: 2,
  c: 3
}
for (let key in myObject) {
  if(myObject.hasOwnProperty(key)) { // 不遍历原型的方法
  console.log(myObject[key]);
}
}
// for - of 适用遍历数/数组对象/字符串/map/set等拥有迭代器对象的集合.但是不能遍历对象,因为没有迭代器对象.与forEach()不同的是，它可以正确响应break、continue和return语句
// 另外 for-of循环不支持普通对象，但如果你想迭代一个对象的属性，你可以用for-in循环（这也是它的本职工作）或内建的Object.keys()方法,如下
//for (let key of Object.keys(myObject)) {
//  console.log(key);
//}

```





```js
// 遍历
{
  let arr=['add','delete','clear','has'];
  let list=new Set(arr);

  for(let key of list.keys()){
    console.log('keys',key);
  }
  for(let value of list.values()){
    console.log('value',value);
  }
  for(let [key,value] of list.entries()){
    console.log('entries',key,value);
  }

  list.forEach(function(item){console.log(item);})
}

```

**拓展** Object.hasOwnProperty(key)



## **`!!`和`!!+'1'/'0'`的用法**

`!!`

1、！可将变量转换成boolean类型，null、undefined和空字符串取反都为true，其余都为false。

```js
!null=true
 
!undefined=true
 
!''=true
 
!100=false
 
!'abc'=false


```

2、！！常常用来做类型判断，在第一步!（变量）之后再做逻辑取反运算，在js中新手常常会写这样臃肿的代码：
判断变量a为非空，未定义或者非空串才能执行方法体的内容。

```js
var a;
if(a!=null&&typeof(a)!=undefined&&a!=''){
    //a有内容才执行的代码  
}
```

实际上我们只需要写一个判断表达：

```js
if(!!a){
    //a有内容才执行的代码...  
}
```

`!!+'1'/'0'`

```js
console.log(!!'1') //true

console.log(!!'1') //true

console.log(!!+'1') //true

console.log(!!+'0') //false 

console.log(!!+0)  //fase

console.log(!!+1)  //true

//原因 `+` 把字符串转成int
```

## `Object.assign()`和深浅拷贝/合并对象

针对深拷贝，需要使用其他办法，因为 `Object.assign()`拷贝的是属性值。假如源对象的属性值是`一个对象的引用`，那么它也只指向那个引用。

```js
// 用法
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }


// 理解深拷贝和浅拷贝
let obj1 = { a: 0 , b: { c: 0}}; 
let obj2 = Object.assign({}, obj1); 
console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 0}} 

obj1.a = 1; 
console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 0}} 
console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 0}} 

obj2.a = 2; 
console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 0}} 
console.log(JSON.stringify(obj2)); // { a: 2, b: { c: 0}}
 
obj2.b.c = 3; // 一个对象的引用
console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 3}} 
console.log(JSON.stringify(obj2)); // { a: 2, b: { c: 3}} 
```

```js
// Deep Clone
  obj1 = { a: 0 , b: { c: 0}};
  let obj3 = JSON.parse(JSON.stringify(obj1));
  obj1.a = 4;
  obj1.b.c = 4;
  console.log(JSON.stringify(obj3)); // { a: 0, b: { c: 0}}
```



**合并对象**

```js
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
```



拓展:[`Object.getOwnPropertyDescriptor()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)和[`Object.defineProperty()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 。



## Fn.apply/call/bind()

call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。

[Js apply方法详解,及其apply()方法的妙用](https://www.cnblogs.com/chenhuichao/p/8493095.html)

[JS中的call、apply、bind方法详解](https://www.cnblogs.com/moqiutao/p/7371988.html)`非常推荐`

**拓展**

数组去重:

```js
function unique () {
  let arr = [].concat.apply([],arguments)
  console.log(arguments);
  return Array.from(new Set(arr));
}

let a = [1,2,3]
let b = [3,4,5]
console.log(unique(a,b));
// 上面的 [].contact ,可以使用Array.prototype.contact替代,注意是原型上的方法
```



主要我是要解决一下几个问题:

> 1. apply和call的区别在哪里
>
> 2. 什么情况下用apply,什么情况下用call
>
> 3. apply的其他巧妙用法（一般在什么情况下可以使用apply）

**apply**:方法能劫持另外一个对象的方法，继承另外一个对象的属性

>  Function.apply(obj,args)方法能接收两个参数
> obj：这个对象将代替Function类里this对象
> args：这个是数组，它将作为参数传给Function（args-->arguments）

**call**:和apply的意思一样,只不过是参数列表不一样.

>  Function.call(obj,[param1[,param2[,…[,paramN]]]])
> obj：这个对象将代替Function类里this对象
> params：这个是一个参数列表

**bind**: bind()方法会创建一个新函数，称为绑定函数

在讨论bind()方法之前我们先来看一道题目：

```
var altwrite = document.write;
altwrite("hello");
```

结果：`Uncaught TypeError: Illegal invocation`
altwrite()函数改变this的指向global或window对象，导致执行时提示非法调用异常，正确的方案就是使用bind()方法：

```
altwrite.bind(document)("hello")
```

当然也可以使用call()方法：

```
altwrite.call(document, "hello")
```



 1.apply示例:

```js
<script type="text/javascript">
	/*定义一个人类*/
	function Person(name,age)
	{
		this.name=name;
		this.age=age;
	}
	/*定义一个学生类*/
	functionStudent(name,age,grade)
	{
		Person.apply(this,arguments);
		this.grade=grade;
	}
	//创建一个学生类
	var student=new Student("zhangsan",21,"一年级");
	//测试
	alert("name:"+student.name+"\n"+"age:"+student.age+"\n"+"grade:"+student.grade);
	//大家可以看到测试结果name:zhangsan age:21  grade:一年级
	//学生类里面我没有给name和age属性赋值啊,为什么又存在这两个属性的值呢,这个就是apply的神奇之处.
</script>
```

分析: Person.apply(this,arguments);

this:在创建对象在这个时候代表的是student

arguments:是一个伪数组,也就是[“zhangsan”,”21”,”一年级”];

    也就是通俗一点讲就是:用student去执行Person这个类里面的内容,在Person这个类里面存在this.name等之类的语句,这样就将属性创建到了student对象里面
---------------------
2.call示例

在Studen函数里面可以将apply中修改成如下:

Person.call(this,name,age);

3.什么情况下用apply,什么情况下用call

在给对象参数的情况下,如果参数的形式是数组的时候,比如apply示例里面传递了参数arguments,这个参数是数组类型,并且在调用Person的时候参数的列表是对应一致的(也就是Person和Student的参数列表前两位是一致的) 就可以采用 apply , 如果我的Person的参数列表是这样的(age,name),而Student的参数列表是(name,age,grade),这样就可以用call来实现了,也就是直接指定参数列表对应值的位置(Person.call(this,age,name,grade));

4.apply的一些其他巧妙用法
细心的人可能已经察觉到,在我调用apply方法的时候,第一个参数是对象(this), 第二个参数是一个数组集合, 在调用Person的时候,他需要的不是一个数组,但是为什么他给我一个数组我仍然可以将数组解析为一个一个的参数,这个就是apply的一个巧妙的用处,可以将一个数组默认的转换为一个参数列表([param1,param2,param3] 转换为 param1,param2,param3) 这个如果让我们用程序来实现将数组的每一个项,来装换为参数的列表,可能都得费一会功夫,借助apply的这点特性,所以就有了以下高效率的方法:

a) Math.max 可以实现得到数组中最大的一项

因为Math.max 参数里面不支持Math.max([param1,param2]) 也就是数组

但是它支持Math.max(param1,param2,param3…),所以可以根据刚才apply的那个特点来解决 var max=Math.max.apply(null,array),这样轻易的可以得到一个数组中最大的一项(apply会将一个数组装换为一个参数接一个参数的传递给方法)

         这块在调用的时候第一个参数给了一个null,这个是因为没有对象去调用这个方法,我只需要用这个方法帮我运算,得到返回的结果就行,.所以直接传递了一个null过去

b) Math.min  可以实现得到数组中最小的一项

同样和 max是一个思想 var min=Math.min.apply(null,array);

c) Array.prototype.push 可以实现两个数组合并

同样push方法没有提供push一个数组,但是它提供了push(param1,param,…paramN) 所以同样也可以通过apply来装换一下这个数组,即:

```js
	 vararr1=new Array("1","2","3");
 
	 vararr2=new Array("4","5","6");
 
	 Array.prototype.push.apply(arr1,arr2);
```

也可以这样理解,arr1调用了push方法,参数是通过apply将数组装换为参数列表的集合.

通常在什么情况下,可以使用apply类似Math.min等之类的特殊用法:

一般在目标函数只需要n个参数列表,而不接收一个数组的形式（[param1[,param2[,…[,paramN]]]]）,可以通过apply的方式巧妙地解决这个问题!



另外: 

```js
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');
 
  console.log.apply(console, args);
};

//另外在Mongoose的快速模板中
db.on('error', console.error.bind(console, 'connection error:'))
```

5.总结:

一开始我对apply 非常的不懂,最后多看了几遍,自己多敲了几遍代码,才明白了中间的道理,所以,不管做什么事情,只要自己肯动脑子,肯动手敲代码,这样一个技术就会掌握…   

还有比如第四部分得内容,巧妙的解决了实实在在存在的问题,这个肯定不是一个初学者能想到的解决方案(这个也不是我自己想的),没有对编程有一定认识的不会想到这个的,还是一句话,多积累,多学习,提升自己的能力和对编程思想的理解能力才是最关键!


其中有大部分内容参考自:

http://www.cnblogs.com/xiaohongwu/archive/2011/06/15/2081237.html


## Touch事件

### 触摸事件touch

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <title>Title</title>
    <style>
        body{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 200px;
            height: 200px;
            background: pink;
            float: left;
        }
    </style>
</head>
<body>
<!--
解释touch:
1. touch是移动端的触摸事件 而且是一组事件
2. touchstart   当手指触摸屏幕的时候触发
3. touchmove    当手指在屏幕来回的滑动时候触发
4. touchend     当手指离开屏幕的时候触发
5. touchcancel  当被迫终止滑动的时候触发（来电，弹消息）
6. 利用touch相关事件实现移动端常见滑动效果和移动端常见的手势事件
-->
<!--
使用touch:
1.绑定事件：box.addEventListener('touchstart',function () { });
2.事件对象：
名字：TouchList------触摸点（一个手指触摸就是一个触发点，和屏幕的接触点的个数）的集合
changedTouches    改变后的触摸点集合
targetTouches     当前元素的触发点集合
touches           页面上所有触发点集合
3.触摸点集合在每个事件触发的时候会不会去记录触摸
changedTouches 每个事件都会记录
targetTouches，touches 在离开屏幕的时候无法记录触摸点
4.分析滑动实现的原理：
4.1 就是让触摸的元素随着手指的滑动做位置的改变
4.2 位置的改变：需要当前手指的坐标
4.3 在每一个触摸点中会记录当前触摸点的坐标 e.touches[0] 第一个触摸点
4.4 clientX clientY      基于浏览器窗口（视口）
4.4 pageX   pageY        基于页面（视口）
4.4 screenX screenY      基于屏幕
-->
<div class="box"></div>
<script>
    window.onload = function () {
        var box = document.querySelector('.box');
        box.addEventListener('touchstart',function (e) {
            console.log('start');
            console.log(e.touches[0].clientX,e.touches[0].clientY);
            console.log(e.touches[0].pageX,e.touches[0].pageY);
            console.log(e.touches[0].screenX,e.touches[0].screenY);
        });
        box.addEventListener('touchmove',function (e) {
            console.log('move');
            console.log(e);
        });
        box.addEventListener('touchend',function (e) {
            console.log('end');
            console.log(e);
        });
        /*box.addEventListener('click',function (e) {
            console.log('click');
            console.log(e);
        });*/
    }
</script>
</body>
</html>
```

### 手势事件

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <title>Title</title>
    <style>
        body {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 200px;
            height: 200px;
            background: pink;
            float: left;
        }
    </style>
</head>

<body>
    <div class="box"></div>
    <script>
        window.onload = function () {
            /*1. 理解移动端的手势事件*/
            /*2. swipe swipeLeft  swipeRight swipeUp swipeDown */
            /*3. 左滑和右滑手势怎么实现*/
            var bindSwipeEvent = function (dom, leftCallback, rightCallback) {
                /*手势的条件*/
                /*1.必须滑动过*/
                /*2.滑动的距离50px*/
                var isMove = false;
                var startX = 0;
                var distanceX = 0;
                dom.addEventListener('touchstart', function (e) {
                    startX = e.touches[0].clientX;
                    console.log('start');
                });
                dom.addEventListener('touchmove', function (e) {
                    isMove = true;
                    var moveX = e.touches[0].clientX;
                    distanceX = moveX - startX;
                    console.log('move');
                });
                dom.addEventListener('touchend', function (e) {

                    /*滑动结束*/
                    if (isMove&&Math.abs(distanceX) > 50) {
                        console.log('end');

                        if (distanceX > 0) {
                            rightCallback && rightCallback.call(this, e);
                        } else {
                            leftCallback && leftCallback.call(this, e);
                        }
                    }
                    /*重置参数*/
                    isMove = false;
                    startX = 0;
                    distanceX = 0;
                });
            }
            bindSwipeEvent(document.querySelector('.box'), function (e) {
                console.log(this);
                console.log(e);
                console.log('左滑手势');
            }, function (e) {
                console.log(this);
                console.log(e);
                console.log('右滑手势');
            });

        }
    </script>
</body>

</html>

```

### tap事件

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <title>Title</title>
    <style>
        body {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 200px;
            height: 200px;
            background: pink;
            float: left;
        }
    </style>
</head>
<body>
<div class="box"></div>
<!--
1. tap事件  轻击 轻触  （响应速度快）
2. 移动端也有click事件 （在移动为了区分是滑动还是点击，click点击延时300ms）
3. 影响用户体验 响应太慢了。
4. 解决方案：
4.1 使用tap事件（不是移动端原生事件，通过touch相关事件衍生过来） （zepto.js tap事件）了解其原理
4.2 使用一个叫：fastclick.js 提供移动端click响应速度的
4.2.1 下载：https://cdn.bootcss.com/fastclick/1.0.6/fastclick.min.js
4.2.2 使用：
-->
<script src="../js/fastclick.min.js"></script>
<script>
    /*当页面的dom元素加载完成*/
    document.addEventListener('DOMContentLoaded', function() {
        /*初始化方法*/
        FastClick.attach(document.body);
    }, false);
    /*正常使用click事件就可以了*/
</script>
<script>
    window.onload = function () {
        /*使用tap事件*/
        /*1. 响应的速度比click要快   150ms */
        /*2. 不能滑动*/
        var bindTapEvent = function (dom, callback) {
            /*事件的执行顺序*/
            /*在谷歌浏览器模拟看不到300ms的效果*/
            /*在真机上面才能看看到延时效果*/
            var startTime = 0;
            var isMove = false;
            dom.addEventListener('touchstart', function () {
                //console.log('touchstart');
                startTime = Date.now();
                /*Date.now();*/
            });
            dom.addEventListener('touchmove', function () {
                //console.log('touchmove');
                isMove = true;
            });
            dom.addEventListener('touchend', function (e) {
                //console.log('touchend');
                console.log((Date.now() - startTime));
                if ((Date.now() - startTime) < 150 && !isMove) {
                    callback && callback.call(this, e);
                }

                startTime = 0;
                isMove = false;
            });
            /*dom.addEventListener('click',function () {
             //console.log('click');
             });*/
        }

        bindTapEvent(document.querySelector('.box'), function (e) {
            console.log(this);
            console.log(e);
            console.log('tap事件')
        });
    }
</script>
</body>
</html>

```

拓展:`callback && callback.call(this, e);` call , bind, apply



### 封装press事件

```js
<script>
  const touchObj = {
    loop: null,
    isMove: false,
    startX: 0,
    startY: 0,
    distanceX: 0,
    distanceY: 0
  }
  const box = document.querySelector('.box')
  box.addEventListener('touchstart', function (e) {
    console.log('start');
    touchObj.startX = e.touches[0].clientX
    touchObj.startY = e.touches[0].clientY
    touchObj.loop = setTimeout(() => {
      console.log('start in');
      if (!touchObj.isMove) {
        console.log('长按了');
      }
    }, 1000)
  })

  box.addEventListener('touchmove', function (e) {
    let MoveX = e.touches[0].clientX
    let MoveY = e.touches[0].clientY
    touchObj.distanceX = MoveX - touchObj.startX
    touchObj.distanceY = MoveY - touchObj.startY
    if (Math.abs(touchObj.distanceX) > 10 || Math.abs(touchObj.distanceY) > 10) {
      console.log('移动了');
      touchObj.isMove = true
    }
  })

  box.addEventListener('touchend', function (e) {
    clearTimeout(touchObj.loop)
    console.log('end');
  })
</script>
```

在手机浏览器中，长按可选中文本，在项目中出现这种现象，会很难受最好还是禁用此功能为上。
```css
{
-webkit-touch-callout:none;
-webkit-user-select:none;
-khtml-user-select:none;
-moz-user-select:none;
-ms-user-select:none;
user-select:none;
}
/* 来源：https://blog.csdn.net/weixin_42565529/article/details/88233982 */
```

### 简陋的双击事件

```js
<script>
  const touchObj = {
    loop: null,
    clickCount: 0,
    doubleClick: false
  }
  const box = document.querySelector('.box')
  box.addEventListener('touchstart', function (e) {
    touchObj.clickCount += 1
    if (touchObj.clickCount >= 2) {
      touchObj.doubleClick = true
      console.log('双击了');
      clearTimeout(touchObj.loop)
      touchObj.loop = null
      touchObj.doubleClick = true
    }
    console.log(touchObj.clickCount);
    console.log(touchObj);
    touchObj.startX = e.touches[0].clientX
    touchObj.startY = e.touches[0].clientY
    touchObj.loop = setTimeout(() => {
      if (!touchObj.isMove) {
        touchObj.clickCount = 0
      }

    }, 300)
  })
</script>
```

## 正则表达式

[`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp) 的 [`exec`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 和 [`test`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) 方法, 以及 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 的 [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)、[`matchAll`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)、[`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)、[`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) 和 [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) 

[正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)

# Other

callback && callback.call(this, e); 改变this的指向



hammer.js手势库