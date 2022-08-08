# HTML面经

⚠️非常重要

👑重要

⭐️一般

📎了解

## ⚠️HTML5新增特性

新的语义标签：header｜nav｜article｜section｜aside｜footer

新增多媒体标签：video

新增本地存储：localStorage｜sessionStorage

新增input类型：calendar｜date｜time｜email｜url｜search

新增表单控制：required是否为空｜placeholder提示信息｜multiple多选文件提交｜autofocus自动聚焦｜autocomplete记录历史

新增技术：webworker|websocket|Geolocation|拖拽释放API

## 👑HTML语义化

用正确的标签做正确的事情，根据内容的结构选择合适的标签

**优点：**

- 便于团队的开发与维护


- 在CSS加载之前，也能够呈现出较好的内容结果与代码结构(一些语义化标签自带一些样式)


- 有利于搜索引擎优化（爬虫依赖于标签来确定上下文和各个关键字的权重）


- 方便其他设备的解析

**语义化标签：**

h1~h6 header nav main section footer address ul ol li strong time date code em

## 👑HTML模版

```html
<!-- 定义文档的类型，告知Web浏览器使用哪种html版本 -->
<!DOCTYPE html>
<!-- 定义一个html文档，lang声明网页和部分网页的语言 -->
<html lang="en">
<!-- 声明头部元素 -->
<head>
    <!-- 提供html文档的元数据 -->
    <!-- 定义文档的字符编码 -->
    <meta charset="UTF-8">
    <!-- 用于指定网页的兼容性模式设置 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 将浏览器的视区修改为设备的物理宽度相同的宽度，初始缩放比例为1 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 为文档定义一个标题 -->
    <title>Document</title>
</head>
<!-- 定义文档的主体 -->
<body>
    <!-- 定义客户端脚本 -->
    <script></script>
</body>
</html>
```

## ⭐️script放置位置

- **head：**被调用的时候才执行（因为html从上往下解析，因此可以保证脚本在调用之前必定被加载了）

- **body：**页面加载的时候被执行（通常用来生成页面的内容）

## 👑defer和async

- 普通的script的话是同步的加载和执行


- defer是异步加载（相较于HTML解析），但是执行要等所有的元素解析完成之后（更加适用于普遍应用脚本）


- async同样也是异步加载（相较于HTML解析），但是执行是在加载完后立马执行（适用于哪些不依赖任何脚本的脚本）


## ⭐️title与alt属性

title：除html、script、meta等标签外都可以添加

alt：img、area和input标签可以添加

- 鼠标经过有文字：title


- 图片无法显示的时候，替代文字title和alt均能实现


- title和alt一起使用的时候，图像无法使用的时候，替代文字为alt，鼠标经过说明为title


- alt的值应该清晰、简洁的描述图像的内容，不应该是描述图像的存在或者是图像的文件名

## 👑iframe标签

**优点：**

- 完全隔离css和js，但又可以使用contentWindow和parent来通信，松耦合又不失灵活。


```js
document.getElementById('childframe').contentWindow
```

```js
window.parent.document
```

- 网页如果为了统一风格，部分板块都是一样的，就可以写成一个网页，用iframe来嵌套，增加代码的复用性


- iframe中页面上下文，在一定程度上类似于沙箱隔离，避免污染环境


- 实现跨域**document.domain + iframe跨域**｜**location.hash + iframe跨域**｜**window.name + iframe跨域**


- 方便插入第三方内容

**缺点：**

- iframe阻塞主页面的Onload事件

- iframe和主页面共享链接池，所以会影响页面的并行加载


- iframe会增加http请求


- 不利于搜索引擎优化


- 移动设备兼容性差

## 📎a标签阻止跳转

```tsx
href="javascript:void(0);"
```

用上面的形式，仍然会有警告，使用Antd中的Button，类型为link

```tsx
<Button type={"link"}></Button>
```
