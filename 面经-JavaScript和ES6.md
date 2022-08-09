# JavaScript和ES6

⚠️非常重要

👑重要

⭐️一般

📎了解

## 👑JS数据类型

**基本类型：**number、string、null、undefined、boolean、symbol、bigint

**引用类型：**Object

## ⚠️ES6新增特性

- let关键字

- const关键字

- set数据结构

- map数据结构

  |          | Map                              | Object                                   |
  | -------- | -------------------------------- | ---------------------------------------- |
  | 意外的键 | 只存在显示插入的键               | 由于存在原型，键名所以可能产生冲突       |
  | 键的类型 | 可以是任何类型                   | string和symbol                           |
  | 键的顺序 | 有序的，插入顺序                 | 无序的                                   |
  | Size     | 可以通过size获取                 | 只能手动计算                             |
  | 迭代     | 是iterable的                     | 只能通过键名来获取值                     |
  | 性能     | 在频繁增删键值对的场景下表现更好 | 在频繁添加和删除键值对的场景下未作出优化 |


- 解构赋值

  ```javascript
  const RAPERS=['postmalone','drake','Lil Nas X']
  let [a,b,c]=RAPERS
  ```

- 迭代器

  部署iterator接口（Array、Arguments、Set、Map、String、TypedArray、NodeList）的可以使用如下方式迭代

- 对象方法扩展（Object.is｜Object.assign｜Object.setPrototypeOf）

- 函数的参数设置初始值

- 箭头函数(箭头函数中的this指向函数声明时所在的作用域)

  ```javascript
  let fn = (a, b) => {
  	return a + b
  }
  ```

- 扩展运算符号...

  ```javascript
  const a = ['1', '2', '3']
  const b = ['4', '5']
  const comb = [...a, ...b]
  console.log(comb)
  ```

- 模版字符串``

  ```javascript
  /* 变量的拼接 */
  let lovest ='posty'
  let out=`${lovest}是我最喜欢的歌手`
  ```

- 生成器（function *）

- 数值扩展（Number.EPSILON｜Number.EPSILON｜Number.isFinite等）

- class类（static｜get和set方法）

  ```javascript
  //ES5类
  function Phone(brand,price){
      this.brand = brand
      this.price  = price
  }
  //添加方法
  Phone.prototype.call = function(){
      console.log('我可以打电话')
  }
  //ES6类
  class Phone1{
      //构造函数
      constructor(brand,price){
          this.brand=brand
          this.price=price
      }
      //方法必须使用该语法
      call(){
          console.log('我可以打电话')
      }
      call2 = ()=>{
          console.log('我可以打电话')
      }
  }
  ```

- 类的继承（extend和super）

- Promise

- rest参数用来替代arguments

  ```javascript
  //老方法获取实参的方式
  function date1(){
      //arguments是对象
      //[Arguments] { '0': 'posty', '1': 'music', '2': 'goodbye' }
      console.log(arguments)
  }
  date1('posty','music','goodbye')
  
  //新方法获取实参的方式(把形参没有接收到的参数全部给args)
  function date2(...args){
      //args是数组
      //[ 'posty', 'music', 'goodbye' ]
      console.log(args)
  }
  date2('posty','music','goodbye')
  
  ```

- Symbol

  Symbol表示独一无二的值|Symbol的值是唯一的，用来解决命名冲突的问题|Symbol值不能与其他数据进行运算|Symbol定义的对象属性不能使用for...in循环便利，但是可以使用

## ⚠️var、let和const

ES5中有全局作用域、函数作用域

ES6中有全局作用域、函数作用域和块级作用域（if语句和for语句）

**var：**没有块的概念、但是不能跨函数作用域

**let：**只能在当前块的作用域内访问，不能跨块访问、不能跨函数访问、同一个块级作用域内不能重复声明已存在的变量

**const：**必须初始化、只能在当前块作用域内访问、而且不能修改(引用类型的话，地址不能改，里面的值可以改)、同一个块级作用域内不能重复声明已存在的变量

**经典考题**

var没有块级作用域，与for一起使用时的问题

```javascript
var liList = document.querySelectorAll('li') // 共5个li
for( let i=0; i<liList.length; i++){
  //let i = 隐藏作用域中的i // 看这里看这里看这里
  liList[i].onclick = function(){
    console.log(i)
  }
}
//var的写法
var liList = document.querySelectorAll('li')
for( var i=0; i<liList.length; i++){
  liList[i].index = i
  liList[i].onclick = function(){
    console.log(this.index)
  }
}
//var写法，或者添加函数作用域
var liList = document.querySelectorAll('li')
for( var i=0; i<liList.length; i++){
  (function(i){
		liList[i].onclick = function(){
    	console.log(this.index)
  	}
  })(i)
}
```

let和const的暂时性死区

```js
x = "global";
// function scope:
(function() {
    //Cannot access 'x' before initialization
    console.log(x); // not "global"
    let x;
}());
```

## ⚠️原型prototype

每个函数都有一个prototype属性（显式原型），它默认指向一个Object空对象（原型对象），原型对象上有一个constructor，它指向函数对象，给原型对象添加属性（方法），其所有实例对象自动拥有原型对象中的属性（方法）。

每个实例对象都有一个\__proto__（隐式原型）

实例对象的隐式原型等于其对应构造函数的\__prototype__(显式原型)

**原型链：**实例对象调用方法的时候，先在对象身上进行查找，如果有，则调用，没有则从（\__proto__）隐式原型属性上去查找。

![image-20220809174822007](/Users/louyangbo/Library/Application Support/typora-user-images/image-20220809174822007.png)

- Object的原型对象不是object类型，如果成object类型，那就无限循环了。


- Function的显示原型和隐式原型指向同一个对象

## ⚠️ES5类的继承

继承方法：原型链、借用构造函数、组合继承、原型式继承、寄生式继承和寄生组合式继承。

- **原型链**

  特点：实例可继承的属性有：实例的构造函数的属性+父类构造函数属性+父类prototype的属性。

  缺点：新实例无法向父类构造函数传参|单一继承｜所有新实例都会共享父类prototype上的属性。

  ```javascript
  function Father(){
      this.name = 'daddy'
  }
  Father.prototype.say = function(){
      console.log('i am father')
  }
  function Son(){}
  //这里既有父类prototype的内容&&父类构造函数中的内容
  Son.prototype = new Father()
  ```

- **借用构造函数继承**

  特点：解决原型链的缺点

  缺点：只能继承父类构造函数属性｜无法实现构造函数的复用｜每个新实例里面都要call和apply一下，代码臃肿

  ```javascript
  function Father(name){
      this.name = name
  }
  function Son(){
      Father.call(this,'daddy')
  }
  ```

- **组合继承**

  特点：结合原型链和借用构造函数的优点

  缺点：需要调用两次父类构造函数（耗内存），会在实例对象上挂载一次构造器中的属性，在原型对象上也挂载一次原型对象的属性

  ```javascript
  function Father(name){
      this.name = name
  }
  Father.prototype.say = function(){
      console.log('i am daddy')
  }
  function Son(name,age){
      Father.call(this,name) //第二次调用父类的构造函数
      this.age  = age
  }
  Son.prototype = new Father() //第一次调用父类的构造函数
  ```

- **原型式继承(类似于Object.create方法)**

  特点：可以通过实例对象来创建克隆实例（主要关注实例对象）

  缺点：无法复用，prototype共享

  ```javascript
  function object(o){
      function F(){}
      F.prototype = o
      return new F()
  }
  ```

- **寄生式继承**

  特点：创建一个实现继承的函数，以某种方式增强对象（同主要关注实例对象）

  ```javascript
  function object(o){
      function F(){}
      F.prototype = o
      return new F()
  }
  function createAnother(original){
      let clone = object(original) //可以换成其他的返回新对象的函数
      clone.newFun = function(){
          console.log('新增的功能')
      }
      return clone
  }
  ```

- **寄生组合式继承**

  特点：解决了组合式继承中需要调用两次构造函数的缺点

  ```javascript
  function inheritPrototype(son,father){
      let prototype = object(father.prototype) //相比于new father，不会再次调用父类的构造函数，通过父类的克隆原型对象当成子类的原型对象
      prototype.constructor = son
      son.prototype = prototype
  }
  function father(name){
      this.name = this.name
  }
  father.prototype.say = function(){
      console.log('i am daddy')
  }
  function son(name){
      father.call(this,name) //调用父类的构造函数
      this.age = 18
  }
  inheritPrototype(son,father)
  ```


## ⚠️闭包

MDN：闭包让开发者可以从内部函数访问外部函数的作用域

怎么产生闭包：

闭包的主要形式：A函数中有变量B和函数C，其中C函数中调用了变量B，并且C函数可以被外部使用（一般return函数C，也可以使用window.C）

闭包变量好像是存储于堆中专门的一个空间中

**闭包作用**

- 间接的访问一个隐藏的变量（非全局变量）,是函数内部和函数外部连接起来的桥梁
- 函数内部的变量在函数执行完毕后，仍然存活在内存中（延长了局部变量的生命周期）

**应用场景**

- 循环中使用闭包解决var定义函数的问题

  ```js
  for ( var i=1; i<=5; i++) {
  	setTimeout( function timer() {
  		console.log( i );
  	}, i*1000 );
  }
  //使用自调用函数，产生闭包
  for (var i = 1; i <= 5; i++) {
    (function(j) {
      setTimeout(function timer() {
        console.log(j);
      }, j * 1000);
    })(i);
  }
  ```


- 设计私有的变量

闭包变现需要进行管理，不使用的时候应当手动清楚

## ⭐️闭包中this的指向问题

除了箭头函数以外（箭头函数的this静态代码的时候就已经确定），this的指向都是在运行的时候决定，闭包的this指向windows

## ⚠️Promise

### Promise基础

promise是一种异步编程的解决方案

Promise本身是同步的，Promise的回调then中的函数是异步的，如果then里面不是函数，那也是同步的

**好处：**

- 指定回调函数的方式更加灵活，可以在使用的时候再设定回调函数，不需要写任务的时候就指定回调函数

- 支持链式调用，可以解决回调地狱的问题（回掉地狱：不便于阅读，不便于异常处理）

**API：**

- **then**

  指定成功的回调和失败的回调

- **catch**

  捕获异常

- **finally**

  不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数

- **resolve**

  **

  ```javascript
  <script>     
    //可以传递一个参数     
    //如果为非promise类型的对象，那么就是一个成功的promise     
    //反之参数的结果决定结果     
    let p1 = Promise.resolve(111);     
    p1.then(value=>{
      //111
      console.log(value);     
    },reason=>{         
      console.log(reason);     
    })
  </script>
  ```

- **reject**

  ```javascript
  let p =Promise.reject("必定失败惹");
  ```

- **all**

  参数可以包含n个promise的数组，只有当所有的promise都成功的时候，才会成功，并返回一个所有promise返回成功所构成的数组。当失败的时候，返回失败的promise的结果

- **race**

  参数可以包含n个promise的数组，状态和结果都由第一个完成的promise决定

- **allSettled**

  所有promise无论成功还是失败都会执行完

### Promise三个状态

实例对象中的属性【PromiseState】值为：

- pending（未决定的）

- resolved/fullfilled（成功）

- rejected（失败）

promise的状态不受外界影响，并且只能改变一次且只能由未决定转换为成功或者为失败。

改变Promise对象状态的三种方法：resolve（）｜reject（）｜throw

### promise改变状态和指定回调顺序

- 当promise的执行器里面为同步任务时，先改变状态，再指定回调。

- 当promise的执行器里面为异步任务时，先指定回调，再改变状态。

但无论如何，回调函数的执行，一定是在改变状态之后的。

### then返回新Promise的状态

是由then方法指定的回调函数执行结果决定

- 抛出异常（失败状态）

- 非Promise类型的对象（成功状态）

- Promise类型的对象（依据该promise对象的状态决定）

### Promise异常穿透

当使用串联多个Promise时，设置失败的回调，只需要在最后一个promise对象中设定，只要在前面的任意位置失败，那么都会调用最后的失败回调。

### 中断Promise

```javascript
//中断Promise链,返回一个pending状态的Promise对象
return new Promise(()=>{})
```

## ⚠️async和await

使用async和await是为了让我们的异步代码看上去更像同步代码

```javascript
async function test(){
  //成功的直接resolve内容会直接被await返回出来
    try{   
        let n = await Chromophore()
        console.log(n)
      //失败的reject内容会在error中
    }catch(error){
        console.log(error)
    }
}
```

**async函数**

默认返回一个Promise对象，并且为成功状态resolve(undefined)，如果手动写了return，那么return的值是什么就是什么

**await表达式**

await右侧表达式

- 非Promise对象

  await会阻塞后面的代码，先执行async外面的同步代码，同步代码执行完，再回到async内部，把这个非promise的东西，作为 await表达式的结果。

- Promise对象

  await 也会暂停async后面的代码，先执行async外面的同步代码，等着 Promise 对象 fulfilled，然后把 resolve 的参数作为 await 表达式的运算结果。（同步的Promise对象：同步的Promise对象会同步执行，但是await拿到值需要时间等待，异步的Promise对象：异步执行），如果该Promise没有设置该有的resolve,reject中的调用，那么await后面的代码将不会运行

## ⚠️事件循环

**基础概念**

- **主线程：**js引擎执行的线程，执行初始化代码（页面渲染、函数处理等）
- **工作线程：**也称幕后线程，这个线程可能存在于浏览器或js引擎内，与主线程是分开的，处理文件读取、网络请求等异步事件。

**Event Loop（事件循环）**：同步任务会直接在主线程中进行执行，而异步任务由工作线程进行处理，然后放入任务队列（event queue）中。主线程中的任务全部完毕以后，再去读区任务队列中的任务（依照先进先出的规则），推入主线程中进行执行。

**tick（指代码需要执行几遍）的步骤**

每次事件循环操作称为tick，其步骤如下：

- 在此次 tick 中选择最先进入队列的任务( oldest task )，如果有则执行(一次)
- 检查是否存在 Microtasks（微任务） ，如果存在则不停地执行，直至清空Microtask Queue（微任务队列）
- 更新 render
- 主程序重复执行上述的步骤

**宏任务和微任务**

- 宏任务（Macro Task/Task）

  script( 整体代码)、setTimeout、setInterval、I/O、UI 交互事件、setImmediate(Node.js 环境)

- 微任务（Micro Task）

  Promise、MutaionObserver、process.nextTick(Node.js 环境)

## ⚠️事件冒泡和捕获

事件的冒泡和捕获是为了解决页面中事件的发生顺序

同时给元素添加冒泡和捕获，会优先执行捕获，再执行冒泡。默认执行冒泡

**事件的捕获（event capturing）**

事件从最外层开始发生，直到最具体的元素。

addEventListener第三个参数为true时，表示捕获阶段调用事件处理函数。

**事件的冒泡（event bubbling）**

事件会从最内层的元素开始发生，一直向上传播，知道document对象。

addEventListener第三个参数为false（默认为false）时，表示冒泡阶段调用事件处理函数。

e.stopPropagation()｜window.event.cancelBubble = true用于阻止事件的冒泡。

**事件委托**

利用事件冒泡的特性，将本应该注册在子元素上的处理时间注册在父元素上，这样点击子元素时发现其本身没有相应事件就到父元素上寻找。

事件委托下如何判断事件源和执行什么回调函数：通过Event对象中的target属性，可以返回事件的目标节点（事件源），根据事件源，执行不同的回调

优势：减少DOM操作，提高性能。｜随时可以添加子元素，添加子元素会自动有相应的处理事件。

**不会冒泡的事件**

blur（失去焦点）和onfocus（聚焦）

使用功能相同的focusout/focusin（上面的优先级更高）

**真实面试题：**

一个表格很多的tr和td，我想点击单元格，输出td的内容：

答：利用事件委托，在tr上绑定事件，然后点击td的时候，通过事件的冒泡去触发。里面的内容使用e.target.innerHTML获取。如果不想触发的话，在tr上绑定事件，并在捕获阶段给元素设置e.stopPropagation()，由于捕获阶段先于冒泡阶段，因此所有的td元素均阻止事件的冒泡。

## ⭐️target/currentTarget

target永远都是最内层的元素，而currentTarget是事件委托过程中捕获/冒泡所选中的目标。

## 📎取消默认事件

e.preventDefault()

## 📎内存溢出/内存泄漏

**内存溢出**

当程序运行需要的内存超过了剩余的内存时，就抛出内存溢出的错误

**内存泄漏**

占有的内存没有及时释放

## ⚠️深拷贝和浅拷贝

对于引用类型，值存放在堆内存中，栈内存中的变量存储的是在堆内存中的值的地址。浅拷贝会拷贝该地址，导致拷贝方更改值时，原数据也会被改动。而深拷贝，不仅对原对象各个属性逐个复制，而且将原对象各属性所包含的对象也一次采用深复制的方式递归复制到新对象上

**深拷贝的实现方法：**

```javascript
var obj = { a:1, arr: [2,3] };

//判断是否是基本数据类型
function isPrimitive(value){
    return (
        typeof value === 'string'||
        typeof value === 'number'||
        typeof value === 'symbol'||
        typeof value === 'boolean'
        )
}
//判断是否是一个js对象
function isObject(value){
    return Object.prototype.toString.call(value)==='[object Object]'
}

//深拷贝
function cloneDeep(value){
    //记录被拷贝的值，防止循环引用
    let memo = {}
    return baseClone(value)

    function baseClone(value){
        let res;
        //基本数据类型，直接返回
        if(isPrimitive(value)){
            return value
        //引用数据类型，浅拷贝新值来代替
        }else if(Array.isArray(value)){
            res = [...value]
        }else if(isObject(value)){
            res = {...value}
        }
        //检查浅拷贝的对象的属性值有没有引用数据类型，如果有，则需要递归拷贝
        //Reflect.ownKeys返回一个由目标对象自身的属性键组成的数组。
        Reflect.ownKeys(res).forEach(key =>{
            //null表示一个空对象指针，所以需要排除
            if(typeof res[key]==='object'&&res[key]!=null){
                //如果之前拷贝过了,res[key]是引用类型数据
                if(memo[res[key]]){
                    //这里调用的时候，其实已经变成独立的了
                    res[key]=memo[res[key]]
                }else{
                    //这里的res[key]还是指向原数据的，但是调用下面函数的时候，他就会被更改为新的独立的
                    memo[res[key]]=res[key]
                    //循环克隆
                    res[key] = baseClone(res[key])
                }
            }
        })
        return res
    }
    
}
//test
let copy = cloneDeep(obj)
copy.arr[0]=1
console.log(obj.arr[0])
```

## 👑Fetch、Ajax和Axios

- **Ajax：**书写麻烦，并且多个请求之间如果有先后关系，容易出现回调地狱，并不是关注分离的思想
- **JQuery ajax：**同样存在回调地狱，同样不是关注分离思想，并且如果单纯只是想使用ajax去引入整个JQuery非常不合理
- **Axios：**根据Ajax进行封装。同样不是关注分离的思想，支持对请求和相应数据做相应的处理｜支持Promise的API｜提供一些并发请求的结构｜支持中断请求｜客户端支持防止CSRF｜从node.js创建http请求
- **Fetch：**符合关注分离，缺点：但是对400和500都当做成功的请求，需要封装去处理（fetch不用用catch来捕获400和500，需要判断response.ok来判断）｜fetch默认不会带cookie，需要添加配置项｜

```javascript
//Ajax
const xhr = new XMLHttpRequest()
xhr.open('POST','http://localhost')
xhr.setRequestHeader('Content-Type','application/json')
xhr.send(param)
//1open调用完毕，2send调用完毕，3服务端返回部分结果，4返回所有结果
xhr.onreadystatechange=function (){
	if(xhr.readyState===4){
		if(xhr.status>=200&&xhr.status<300){
			console.log(xhr.status); //状态码
			console.log(xhr.statusText); //状态字符串
			console.log(xhr.getAllResponseHeaders()); //响应头
			console.log(xhr.response) //响应体
		}
	}
}

// Axios
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});

// Fetch
window.fetch(url,config)
	.then(async (response)=>{
    if(response.status==401){ //token失效
      console.log('token失效')
      await auth.logout()
      window.location.reload()
      return Promise.reject({mess:"请重新登录"})
    }
    const data = await response.json()
    //fetch的异常需要判断response.ok而不是直接根据状态码
    if(response.ok){
      return data
    }else{
      return Promise.reject(data)
    }
  }).catch(()=>{
  console.log('无法连接服务器')
})
```

## 📎Fetch中断

使用AbortController来实现fetch的请求中止

```js
const btn1 = document.getElementById("btn1")
const btn2 = document.getElementById("btn2")
//使用AbortController()创建的实例身上的signal标记请求，abort取消请求
const controller = new AbortController()
const signal = controller.signal

btn1.addEventListener('click',()=>{
  fetch('http://localhost:3000/posts',{
    method: 'Get',
    signal: signal
  }).then(async response=>{
    if(response.ok){
      const data = await response
      console.log(data)
    }
  })
})
btn2.addEventListener('click',()=>{
  controller.abort()
})
```

## ⚠️判断数据类型

- typeof 运算符号

  缺点：typeof检测null的时候也会返回Object

- instanceof运算符（利用原型链来进行查找）

  优点：弥补了typeof不能具体检测属于哪个对象的局限性

  缺点：不能用来检测和处理字面量方式创建出来的基本数据类型，即原始数据类型

  ```js
  function checkIsInstanceOf(obj,type){
      let proto = Object.getPrototypeOf(obj)
      while(proto!=null){
          if(proto===type.prototype){
              return true
          }
          proto = Object.getPrototypeOf(obj)
      }
      return false
  }
  ```

- constructor

  优点：作用和instanceof相同，可以处理引用类型还能处理原始数据类型

  使用constructor的时候最好加上()，放置js把它当成小数

  ```js
  (1).constructor === Number
  ```

  缺点：由于是函数原型上面的属性，类的原型进行重写后，可能导致判断不准确

- Object.prototype.toString.call()方法

  Object.prototype.toString返回当前方法的执行体所属类的详细信息

  返回结果如：[object Number]

## 📎typeof Null === ‘object’

javascript的最初实现中，javascript的值是通过一个表示类型的标签和实际数据值表示的（32位），null是机器码0x00的形式表示（这也是为什么Number(null)==0），typeof则是根据标志符来进行判断的

判断类型的小型标签(1-3位)：

000:对象｜1:整数｜010:双精度类型｜100:字符串｜110:布尔类型

补充：

undefined：-2^30（整数范围之外的数字）

null：机器码NULL

## ⭐️null/undefined

- null转数字为0，undefined转数字为NaN
- null表示尚未存在的对象，undefined表示缺少值

## 👑判断数组类型

- instanceof


- 对象的constructor对象


- Object.prototype.toString.call()方法


- Array.isArray()方法

## ⭐️箭头函数

- 不需要function关键字来创建


- 函数体只有一个return语句的时候，可以省略return


- 改变this的指向

## 👑new的过程

- 创建一个空对象obj

- obj的\__proto__指向原型


- 绑定this值，让Func中的this指向obj，执行Func的函数体

- 如果构造函数返回的是对象的话，就返回执行构造函数时返回的对象，如果不是对象的话，返回1中创建的对象

## 📎document.write和innerHTML

- document.write是直接将内容写入界面的内容流，会导致页面全部重绘


- innerHTML是将内容写入某个DOM节点，不会导致页面全部重绘

## 📎innerHTML、innerText和outerHTML

- innerHTML标签内部的所有内容


- outerHTML包括标签，以及内部的所有内容


- innerText标签内部的文本内容

## ⭐️JS内存回收机制

垃圾采集器将不具有可达性（可以访问到的或者可以使用的数据）的对象删除。具体采用标记清除和引用计数

**标记清除**

垃圾回收器一开始会给内存中所有变量加上一个标记。垃圾回收器会定期地从根开始找可获得变量，并且将这些可获得变量的标记清除。没被清楚标记的变量将会被回收。

**引用计数**

为每个变量添加引用计数，引用计数为0的就直接回收。

## ⚠️call、bind和apply

都可以重新定义this的指向

```js
let obj = {
    name:'lyb',
    age:18,
    print:function(){
        console.log(this.name,this.age)
    }
}
let obj2 = {
    name:'posty',
    age:26
}
obj.print()//lyb 18
//call、bind和apply作用一样
obj.print.call(obj2)//posty 26
```

区别：

- call和bind传递参数写法为(a,b,c);而apply传递参数的写法为([a,b,c])


- call和apply调用后返回结果，而bind返回函数需要再添加一个()


- bind可以给构造函数指定this


call、bind和apply均不能改变箭头函数中this的指向，因为箭头函数在定义的时候已经制定了他具体指向谁，而不是在调用的时候决定的。

❓多次调用bind()，this只由第一次bind()所决定

## 📎获取箭头函数中所有参数

普通函数可以使用arguments（对象形式存储）和...args（数组形式存储）；箭头函数只能使用...args获取

## ⚠️数组扁平化

```js
let arr = [1, [2, 3, [4, 5]]]
// 1 reduce
function flatten1(arr){
    return arr.reduce((pre,cur)=>{
        return pre.concat(Array.isArray(cur)?flatten1(cur):cur)
    },[])
}
// 2 toString/split
function flatten2(arr){
    return arr.toString().split(',').map(value=>Number(value))
}

// 3 join/split
function flatten3(arr){
    return arr.join(',').split(',').map(value=>Number(value))
}

// 4 拓展运算符
function flatten4(arr){
    while(arr.some(value=>Array.isArray(value))){
        arr = [].concat(...arr)
    }
    return arr
}

// 5 flat
function flatten5(arr){
    while(arr.some(value=>Array.isArray(value))){
        arr = arr.flat()
    }
    return arr
}
```

## ⭐️==和===

**===**两者类型和值都相等才会返回true

**==**首先比较两者的类型是否相同，不相同的话，先进行转化，然后再进行比较

- null == undefind返回true


- string和number进行==的时候，string转换成number


- 判断其中一方是否是boolean的时候，boolean转换为number


- 判断其中一方是否为object，且另一方是string、number或者symbol，是的话就把object转化成原始类型进行比较

## 👑async await的原理

```js
//其实和co函数库一样，不过比co函数库简单，因为不用考虑thunk函数
//主要是自动执行next和添加回调（将上一次的yield中异步执行的结果（均为{value,done}等形式）中value部分，作为上一次yield的结果）
function asyncToGenerator(generatorFunction){
    return function(){
        const gen = generatorFunction.apply(this,arguments)
        return new Promise((resolve,reject)=>{
            const run = (mode,result)=>{
                let genResult
                try{
                    genResult = gen[mode](result)
                }catch{
                    return reject(result)
                }
                const {value, done} = genResult
                if(done == 'done'){
                    return resolve(value)
                }else{
                    return Promise.resolve(value).then(
                        res=>run("next",res),
                        rej=>run("error",rej)
                    )
                }
            }
            run("next")
        })
    }
}
```

## 📎ToPrimitive

对象类型转换时，会调用内置的[@@toPrimitive]函数。

- 如果期望或得一个string或default，则[@@toPrimitive]会先调用toString。如果toString属性不存在，则会调用valueOf。如果也不存在则会抛出一个TypeError

- 如果期望获得number类型，则[@@toPrimitive]会首先尝试valueOf，若失败再尝试toString也可以自己重写[Symbol.toPromitive]方法


```js
//未重写版本
let a = {
    name:'小王',
    age:18,
}
console.log(a+'') //[object Object]
//重写toPrimitive版本
let a = {
    name:'小王',
    age:18,
    [Symbol.toPrimitive](){
        return `name is ${this.name}, age is ${this.age}`
    }
}
console.log(a+'') //name is 小王, age is 18
```

## 📎this指向优先级

new创建实例绑定构造函数中的this>bind，call和apply函数>obj.foo()调用的方法>foo()调用方式（window）

箭头函数的this一旦被绑定就不会再被任何方法改变

## ⭐️JSON

JSON是一种轻量级的数据交换格式

JSON是一种语法，用来序列化对象、数组、数值、字符串、布尔值和null

| JavaScript类型 | JSON的不同点                                                 |
| -------------- | ------------------------------------------------------------ |
| 对象和数组     | 属性名称必须使用双引号括起来，最后一个属性后不能有逗号       |
| 数值           | 禁止出现前导0（stringify会自动忽略，但是parse会抛出异常），如果有小数点，则后面至少跟着一位数字 |
| 字符串         | 字符串必须使用""引用起来，如'"\u2028\u2029"',行分隔符和段分隔符被允许，部分控制符号被禁止，只有有限的字符可能会被转义 |

- JSON.parse()解析JSON字符串，并返回对应的值，可以额外传入一个转换函数，用来将生成的值和其属性，在返回之前进行某些修改


- JSON.stringify()返回与指定值对应的JSON字符串，可以通过额外的参数，控制仅包含mouser属性，或者以自定义的方法来替换某些key对应的属性值

## 📎XML和JSON

数据体积方面：JSON比XML数据的体积更小

传输速度方面：JSON的速度要远远快于XML

数据交互方面：JSON与JavaScript的交互更加方便

数据描述方面：XML对数据的描述性比JSON好

## 📎XML和HTML

XML被设计用来传输和存储数据（是不作为的，仅仅是用来结构化、存储以及传输数据）｜HTML被设计用来显示数据