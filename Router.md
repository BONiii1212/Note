# React-Router

## 单页面应用

一个html下，一次性加载js、css等资源，所有页面在一个容器页面下，页面的切换实质是组件的切换。

单页面应用路由实现原理：切换url、监听url变化、渲染不同页面组件

## Router三个库

- **history：**popState、history.pushState的底层实现
- **react-router:**封装了Router、Route和Switch等组件
- **react-router-dom:**Link、BrowserRouter、HashRouter组件

## BrowserRouter

实现基于H5新推出的history API

**改变路由的方法**

这些方法能够使得url的变化，但是不需要进行页面的跳转

- **pushState**

  ```js
  // state 一个月网站相关的对象（入栈的index，唯一标识key），会自动传入popstate事件的回调函数中
  // title心页面的标签（目前浏览器都忽略）
  // 新的路径
  history.pushState(state,title,path)
  ```

  ```js
  state:{ //react-router在state中存储的值
  	idx: 1 //入栈的index
  	key: "lrgoc4qs" //唯一标识key
  	usr: null
  }
  ```

- **replaceState**

  ```js
  history.replaceState(state,title,path)
  ```

**监听路由**

- **popstate事件**

  文档的history对象变化时，触发（pushState和replaceState不会触发该事件，back、go、forward会触发）

**原理：**

- 点击Link之后，触发push方法（首先生成一个新url的location对象，通过window.history.pushState方法更改浏览器当前路由，由于通过pushState方法不会出发popstate事件，因此需要手动调用setState方法触发组件的更新）
- setState方法接收一个location对象，通过合并先后的信息，通知每个listens路由已经发生了变化

pushState手动触发setState，go，back，forward通过popstate事件监听触发setState

## HashRouter

锚点的形式，兼容性更好

**改变路由的方法**

- **window.location.hash**

**监听路由**

- **onhashchange**

区别：

- 表现形式不一样，一个带哈希值#
- BrowserRouter在页面刷新后，如果没有做默认返回，拿着url去请求服务器是请求不到页面的，因为url的变动只是改变组件的变动，并不会去向服务器去发送请求
- 底层不一样一个是history API一个是window.location.hash