## 原型

如果想要使用一些属性和方法,并且属性的值在每个对象中都是一样的,方法在每个对象中的操作也都是一样,那么,为了**共享数据,节省内存空间**,是可以把属性和方法通过原型的方式进行赋值

## 原型链

是一种关系**,实例对象**和**原型对象**之间的关系,关系是通过原型(`__proto__`)来联系的

(

当试图得到一个对象f的某个属性时，如果这个对象本身没有这个属性，那么会去它的_proto_（即它的构造函数的prototype）obj._proto_中去寻找.....

)

用`new Student()`创建的对象还从原型上获得了一个`constructor`属性，它指向函数`Student`本身：

```js
xiaoming.constructor === Student.prototype.constructor; // true
Student.prototype.constructor === Student; // true

Object.getPrototypeOf(xiaoming) === Student.prototype; // true

xiaoming instanceof Student; // true
```



![1565881796339](media/1565881796339.png)

```js
  <script>
    //使用对象---->使用对象中的属性和对象中的方法,使用对象就要先有构造函数
    //构造函数
    function Person(name,age) {
      //属性
      this.name=name;
      this.age=age;
      //在构造函数中的方法
      this.eat=function () {
        console.log("吃好吃的");
      };
    }
    //添加共享的属性
    Person.prototype.sex="男";
    //添加共享的方法
    Person.prototype.sayHi=function () {
      console.log("您好啊,怎么这么帅,就是这么帅");
    };
    //实例化对象,并初始化
    var per=new Person("小明",20);
    per.sayHi();
    //如果想要使用一些属性和方法,并且属性的值在每个对象中都是一样的,方法在每个对象中的操作也都是一样,那么,为了共享数据,节省内存空间,是可以把属性和方法通过原型的方式进行赋值

    console.dir(per);//实例对象的结构
    console.dir(Person);//构造函数的结构

    //实例对象的原型__proto__和构造函数的原型prototype指向是相同的

    //实例对象中的__proto__原型指向的是构造函数中的原型prototype
    console.log(per.__proto__==Person.prototype);
    //实例对象中__proto__是原型,浏览器使用的
    //构造函数中的prototype是原型,程序员使用的

    //原型链:是一种关系,实例对象和原型对象之间的关系,关系是通过原型(__proto__)来联系的



  </script>
```

## 原型继承 //组合继承 // class继承

在传统的基于Class的语言如Java、C++中，继承的本质是扩展一个已有的Class，并生成新的Subclass
在js中,我们无法直接扩展一个Class

JavaScript的原型继承实现方式就是：
(改变原型的指向)
定义新的构造函数，并在内部用call()调用希望“继承”的构造函数，并绑定this；

借助中间函数F实现原型链继承，最好通过封装的inherits函数完成；

继续在新的构造函数的原型上定义新方法。

## 原型继承和new/Object.create()

[阮一峰](https://www.liaoxuefeng.com/wiki/1022910821149312/1023022126220448)

[**Object.create()**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

polyfill

```js
Object.create = function (proto) {
        function F() {}
        F.prototype = proto;
        return new F();
    };
```



**Object.create(proto)**方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 （请打开浏览器控制台以查看运行结果。）

`proto`新创建对象的原型对象

```js
// Shape - 父类(superclass)
function Shape() {
  this.x = 0;
  this.y = 0;
}

// 父类的方法
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - 子类(subclass)
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// 子类续承父类
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?',
  rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?',
  rect instanceof Shape); // true
rect.move(1, 1); // Outputs, 'Shape moved.'


```

原型继承:廖雪峰

```js
function inherits(Child, Parent) {
    var F = function () {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
    // 相当于
    Child.prototype = Object.create(Parent.prototype)
    Child.prototype.constructor = Child
}
// 以下是MDN的代码
PrimaryStudent.prototype = Object.create(Student.prototype)
// 把PrimaryStudent原型的构造函数修复为PrimaryStudent:
PrimaryStudent.prototype.constructor = PrimaryStudent;
// 相当于 

// 把F的原型指向Student.prototype:
F.prototype = Student.prototype;
// 把PrimaryStudent的原型指向一个新的F对象，F对象的原型正好指向Student.prototype:
PrimaryStudent.prototype = new F();
```

### new操作符原理

```js
//new运算符原理实现
var new1 = function(fun){
    var newObj = Object.create(fun.prototype);
    // obj.__proto__=Foo.prototype;
    // obj.__proto__.constructor=Foo;
    var returnObj = fun.call(newObj);
    if(typeof returnObj === 'object'){
        return returnObj
    }else{
        return newObj
    }
}
```



## 冒泡

```js
let arr = [2,1,5,6,3]

function sortArr(arr) {
  for(let i=0; i<arr.length-1; i++) {
    let flag = true
    for(let j=i; j< arr.length-1-i;j++) {
      if(arr[j]>arr[j+1]) {
        let temp = arr[j]
        arr[j] = arr[j+1]
        arr[j+1] = temp
        flag = false
      }
    }
    if(flag) {
      break
    }
  }
}
sortArr(arr)
console.log(arr);

```



## promise ajax

[Ajax原理一篇就够了](http://www.sohu.com/a/238246281_100109711)

[Ajax原理](https://blog.csdn.net/weixin_37580235/article/details/81459282#1.3%20Ajax的工作原理)

**原理**: 老板 - 秘书 - 小李 的关系, 浏览器 - 中间层(Ajax引擎) - 服务器

![1566304600696](media/1566304600696.png)

**作用**: 提高用户体验, 较少的网络数据传输量

核心对象: XMLHttpRequest

```js
function ajax({ method, path, body, headers}) {
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest()
    xhr.open(method, path)
    for (let key in headers) {
      let value = headers[key]
      xhr.setRequestHeader(key, value)
    }
    xhr.send(body)
    xhr.onreadystatechange = () => {
      if(xhr.readyState === 4) {
        if(xhr.status >= 200 &&xhr.status <= 400) {
          resolve(xhr.responseText)
        } else {
          reject(xhr)
        }
      }
    }
  })
}

ajax({
  method: 'post',
  path: '/xx',
  body: 'username=root&password=123456',
  header: {
    'content-type': 'application/x-www-form-urlencoded',
    'Authorization': 'token'
  }
})
  .then(response => {
    
  })

```



## 闭包

[闭包是什么](https://bbs.csdn.net/topics/392430917?page=1)

### 闭包是什么?

在函数里面return 一个函数;

### 作用

闭包主要是避免全局变量带来的污染,保护局部变量不被回收,对象内部封装一个私有变量(返回的函数会被缓存,一直在内存当中)

案例

点赞

[防抖函数 ](https://blog.csdn.net/l598465252/article/details/85797497)

```js
function debounce (func, delay) {
  let timer;
  let lock = false;
  return function (params) {
    if (lock) {
      // 触发间隔未结束，重新设定计时器
      clearTimeout(timer);
      timer = setTimeout(() => {
        lock = false;
        clearTimeout(timer);
        timer = null;
      }, delay);
    } else {
      lock = true;
      func(params);
    }
  };
}

```

携带状态的函数/

了解闭包先了解js的变量：全局/局部

全局变量可以在任何地方获取

局部变量只能在函数内部获取

闭包的出现，可以获取函数内部的变量且变量一直在内存当中,缓存数据,

在这里稍微看一下js的变量提升

## 深浅拷贝

```js
  <script>
    
    //浅拷贝:拷贝就是复制,就相当于把一个对象中的所有的内容,复制一份给另一个对象,直接复制,或者说,就是把一个对象的地址给了另一个对象,他们指向相同,两个对象之间有共同的属性或者方法,都可以使用
    
    
    var obj1={
      age:10,
      sex:"男",
      car:["奔驰","宝马","特斯拉","奥拓"]
    };
    //另一个对象
    var obj2={};
    
    //写一个函数,作用:把一个对象的属性复制到另一个对象中,浅拷贝
    //把a对象中的所有的属性复制到对象b中
    function extend(a,b) {
      for(var key in a){
        b[key]=a[key];
      }
    }
    extend(obj1,obj2);
    console.dir(obj2);//开始的时候这个对象是空对象
    console.dir(obj1);//有属性

    
    
    
  </script>

  <script>
    //深拷贝:拷贝还是复制,深:把一个对象中所有的属性或者方法,一个一个的找到.并且在另一个对象中开辟相应的空间,一个一个的存储到另一个对象中

    var obj1={
      age:10,
      sex:"男",
      car:["奔驰","宝马","特斯拉","奥拓"],
      dog:{
        name:"大黄",
        age:5,
        color:"黑白色"
      }
    };

    var obj2={};//空对象
    //通过函数实现,把对象a中的所有的数据深拷贝到对象b中
    function extend(a,b) {
      for(var key in a){
        //先获取a对象中每个属性的值
        var item=a[key];
        //判断这个属性的值是不是数组
        if(item instanceof Array){
          //如果是数组,那么在b对象中添加一个新的属性,并且这个属性值也是数组
          b[key]=[];
          //调用这个方法，把a对象中这个数组的属性值一个一个的复制到b对象的这个数组属性中
          extend(item,b[key]);
        }else if(item instanceof Object){//判断这个值是不是对象类型的
     //如果是对象类型的,那么在b对象中添加一个属性,是一个空对象
          b[key]={};
          //再次调用这个函数,把a对象中的属性对象的值一个一个的复制到b对象的这个属性对象中
          extend(item,b[key]);
        }else{
          //如果值是普通的数据,直接复制到b对象的这个属性中
          b[key]=item;
        }
      }
    }

    extend(obj1,obj2);
    console.dir(obj1);
    console.dir(obj2);


// 注意:
/*
深拷贝的另外一种方法就是利用JSON.stringfy和parse



*/


  </script>
```

## ES6交换两个值

```js
[a,b]=[b,a];

a=[b,b=a][0];
```

## [HTML5和CSS3新特性一览](https://www.cnblogs.com/star91/p/5659134.html)

HTML5
1.新元素,包括input 类型,新的语义标签
2.Canvas,
3.新增API(拖放,地理位置,Audio,Video)
4.Web存储
( 

sessionStorage,localStorage,cookies的区别 

不同网站相互之间不能访问,同源策略(安全)

)
5.应用程序缓存
6.WebSocket

CSS3
1.新增一些伪类选择器
2.animation(@keyframes),tramsform
3.背景渐变
4.flex

## MVC和MVVM

MVC: Controller被设计出来并不是处理数据解析的。1、管理自己的生命周期；2、处理Controller之间的跳转；3、实现Controller容器。这里面根本没有“数据解析”这一项

MVVM: 想象Controller是一个Boss，数据是一堆文件（Model），如果现在是MVC，那么数据解析（比如整理文件）需要由Boss亲自完成，然而实际上Boss需要的仅仅是整理好的文件而不是那一堆乱七八糟的整理前的文件。所以Boss招聘了一个秘书，现在Boss就不再需要管理原始数据（整理之前的文件）了，他只需要去找秘书：你帮我把文件整理好后给我。
作者：星球小霸王链接：https://www.jianshu.com/p/b0aab1ffad93来源：简书简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

## [Vue中的身份验证](https://www.cnblogs.com/jtjds/p/9634840.html)

登陆权限: 登陆token,vuex store中存储token,(在axios的拦截器中带上请求头)

通过路由全局前置守卫判断是否带有已经登陆(带有token),axios拦截器判断是否过期



可访问路由权限(动态加载菜单和路由): 在页面加载的时候，就进行权限判断(路由meta属性)路由元信息增加角色,没有就都允许)，有的话通过router.addRoutes方法挂载在可访问路由表上,无权限的操作不进行显示

## VUEX

Vuex 使用**单一状态树**,管理状态的仓库

mutations必须是同步的, 我们都知道任何回调函数中进行的状态改变都是无法追踪的,无法实现捕捉状态devtools
Action 提交的是 mutation，而不是直接变更状态,为了解决mutations只有同步的问题

## 聊聊对vue的一些理解

[聊聊对vue的一些理解](https://www.jianshu.com/p/0c0a4513d2a6)

Vue是一套构建用户界面的渐进式框架,也可以理解为是一个视图模板引擎,强调的是状态到界面的映射。

Vue.js（读音 /vjuː/, 类似于**view**）是一个构建数据驱动的 web 界面的库。Vue.js 的目标是通过尽可能简单的 API 实现**响应的数据绑定**和**组合的视图组件**。(数据驱动和组件化)



## 双向数据绑定

vue的双向绑定是由**数据劫持结合发布者－订阅者模式实现的，**那么什么是数据劫持？vue是如何进行数据劫持的？说白了就是通过**Object.defineProperty()**来**劫持对象属性的** **setter**和**getter** 操作.

那么首先要对数据进行**劫持监听**，所以我们首先要设置一个**监听器Observer**,用来监听所有的属性，当属性变化时，就需要通知**订阅者**Watcher**,看是否需要更新．因为属性可能是多个，所以会有多个订阅者，故我们需要一个**消息订阅器****Dep**来专门收集这些订阅者，并在监听器Observer和订阅者Watcher之间进行统一的管理．以为在节点元素上可能存在一些指令，所以我们还需要有一个**指令解析器**Compile**，对每个节点元素进行扫描和解析，将相关指令初始化成一个订阅者Watcher，并替换模板数据并绑定相应的函数，这时候当订阅者Watcher接受到相应属性的变化，就会执行相对应的更新函数，从而更新视图
(Observer(监听器,劫持所有属性), watcher(订阅者)更新师徒,Dep(消息订阅器)收集订阅者,通知变化,,Compile(指令解析器)初始化视图,绑定更新函数)

![1566446573756](media/1566446573756.png)

## 正则

## VUE query和params区别

```
params 和 query 传参的区别：
1、params传参时  参数不会出现在url的路径上面， 但是刷新页面时param里面的数据会消失
2、query传参时   参数出现在url的路径上面， 刷新页面时query里面的数据不变
```

**如果想要在页面刷新或者执行其它操作之后还保留传递的参数，需要在路由表（routes）中作配置，使用 “：参数名”的形式进行占位处理**



## cookies/session/token

https://www.cnblogs.com/moyand/p/9047978.html

https://segmentfault.com/a/1190000017831088

https://www.cnblogs.com/pengc/p/8714475.html

## cookies/sessionStroage/localStroage

## [document.write()和innerHTML的区别](https://www.cnblogs.com/lyd447113735/p/8856982.html)

[document.write()/innerHTML](https://www.cnblogs.com/fly-xfa/p/6031075.html)

页面有初始内容，点击页面中的按钮向页面中通过document.write()方法写入内容，会发现原先的初始内容消失了，整个页面只剩下了通过write()方法写入的内容。原因是整个页面进行了重绘
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

初始内容


<button onclick="fun()" >按鈕</button>


<script>
    function fun() {
        document.write("write内容");
    }

</script>

</body>
</html>
```
举例二：页面有初始内容，在初始内容后面给定一个节点，通过innerHTML向这个节点写内容，初始内容不消失，通过innerHTML新增加的内容准确的显示在节点位置
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

初始内容<a id="p"></a>


<button onclick="fun()">按钮</button>


<script>
    function fun() {
       document.getElementById("p").innerHTML="新增加的innerHTML内容";
    }

</script>

</body>
</html>
```

[iframe基本内涵](https://www.cnblogs.com/hq233/p/9849939.html)
嵌套文档,
广告

## CSS中的继承

[CSS中的继承](https://www.cnblogs.com/duhuo/p/8018756.html)

font系列 size,style,family

text 系列 align line-height color

visibility, list-style, cursor
## eval()

![1566445284710](media/1566445284710.png)

eval 好不好取决于怎么使用它，一般认为的缺点：
1. 可读性非常差
2. 不好再做优化和编译
3. 会轻微增加性能消耗
4. 不安全，XSS攻击
5. 不好调试

[JavaScript 为什么不推荐使用 eval？](https://www.zhihu.com/question/20591877)


[eval到底有什么问题](https://www.jianshu.com/p/3b2d86f3aecb)

[为什么eval要添加括号呢？](https://www.cnblogs.com/lovebing/p/8302093.html)

```js
console.log(eval("{}"); // undefined

console.log(eval("({})");// object[Object]
```
原因：eval本身的问题。 由于json是以{}的方式来开始以及结束的，在JS中，它会被当成一个语句块来处理，所以必须强制性的将它转换成一种表达式。

## [两种盒子模型](https://wanghan0.github.io/2017/03/31/css-box/#u4E24_u79CD_u76D2_u5B50_u6A21_u578B)

其实盒模型有两种，分别是**ie盒子模型**（IE6以下版本浏览器)和**标准w3c盒子模型**，区别在于前者content的宽度和高度包括了border和padding。

margin（外边界）虽不可见，但是它确实在文档中占据了空间，我们要区分两个概念即：盒子所占空间（计入margin ）和盒子实际的大小（不计入margin） 。

 [实例区分两种盒模型](https://wanghan0.github.io/2017/03/31/css-box/#u5B9E_u4F8B_u533A_u5206_u4E24_u79CD_u76D2_u6A21_u578B)

下面举个例子来区分两种盒模型：一个盒子的 margin 为 20px，border 为 2px，padding 为 10px，content 的宽为 200px、高为 50px。

### [ie盒子模型](https://wanghan0.github.io/2017/03/31/css-box/#ie_u76D2_u5B50_u6A21_u578B)

盒子所占空间：width=20ｘ2+200=240      　　          height=20ｘ2+50=90

盒子实际大小：width=200         　　　　　　　   height=50

### [标准w3c盒子模型](https://wanghan0.github.io/2017/03/31/css-box/#u6807_u51C6w3c_u76D2_u5B50_u6A21_u578B)

盒子所占空间：width=20ｘ2+2ｘ2+10ｘ2+200=264  　  height=20ｘ2+２ｘ2+10ｘ2 +50=114

盒子实际大小：width=200 +2ｘ2+10ｘ2 =224   　　　    height=50+2ｘ2+10ｘ2=74

解释到这里，有的人可能会想起CSS3里面有个叫做box-sizing的属性，咦？两个盒模型不就是它不同取值下的效果吗？那我恭喜你，你说对了～

 [box-sizing和两种盒模型不得不说的事](https://wanghan0.github.io/2017/03/31/css-box/#box-sizing_u548C_u4E24_u79CD_u76D2_u6A21_u578B_u4E0D_u5F97_u4E0D_u8BF4_u7684_u4E8B)

box-sizing有三个取值：

1、content-box:使元素遵循标准 w3c 盒子模型（默认值）。

2、border-box:使元素遵循ie 盒子模型。

3、 inherit： 规定应从父元素继承 box-sizing 属性的值。

那么我可以用box-sizing的不同取值让大家直观地理解两个盒子的区别，也顺带理解这个属性，下面是需要用到的html代码，方便大家看得清楚，我给盒子外面添加一个宽高各500px的灰色背景。

        <div class='bg'>

            <div class='box'></div>
​        </div>

[box-sizing：content-box](https://wanghan0.github.io/2017/03/31/css-box/#box-sizing_uFF1Acontent-box)

​        .box{

​                background-color: #91D4DA;

​                width: 300px;

​                height: 300px;

​                padding: 20px;

​                border: 10px solid #5B5B5B;

​                box-sizing: content-box;    /*默认值，可以不写*/

​            }