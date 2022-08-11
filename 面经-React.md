# React

⚠️非常重要

👑重要

⭐️一般

📎了解

## 📎React出现原因

- 原生的DOM操作繁琐，因此有了JQuery


- 项目变得越来越复杂，光是简化操作DOM远远不够，需要通过组件化的形式让项目结构变得更加清晰，因此出现了像Vue还有React这样的框架

## 📎React特点

react具有声明式（关注我们要什么）、组件化

## ⭐️React和Vue的区别

**共同点：**

- 组件化
- 声明式编程 函数式编程
- 单向数据流，数据驱动视图
- Virtual DOM+Diff算法操作DOM
- 社区成熟，都支持SSR

**不同点：**

- Vue使用模版拥抱html，React使用JSX拥抱JS


- Vue中js，html，css分离；React中all in JS


- Vue“自动挡”，React“手动挡”（api和生态）

## 📎 JSX防止XSS

React DOM在渲染所有输入内容之前，默认会进行转义。可以有效防止XSS攻击。

在用户提交参数前，将提交的字符`<` 、`>`、`&`、`"` 、`'` 、`+`、`/`等进行转义，严格控制输出

## 👑JSX转JS

依靠React.createElement方法（可以从babel的转化中看出，浏览器是识别不了JSX的，需要通过Babel将JSX转换为JS对象）

React.createElement方法在执行时，判断第一个参数首字母的大小写，如果是小写，则会被认为是原生的DOM节点来进行渲染，因此React中自定义组件规定首字母大写

```js
React.createElement(
  type, //小写的话会被认为是原声的DOM节点，大写的话会被认为是自定义组件
  [props],
  [...children]
)
```

## ⭐️React的虚拟DOM

- 虚拟DOM是一个对象结构，真实DOM是一个DOM结构
- 真实DOM身上会挂载很多的属性(如事件、scroll等等信息)，简单来说虚拟DOM要比真实DOM来的轻
- 在传统的Web应用中，数据的变化会实时地更新到用户界面中，由于每次数据微小的变化都会引起DOM的渲染，而虚拟DOM就是将所有的操作聚集到一块，计算出所有的变化后，统一更新一次虚拟DOM

![image-20220810155612549](/Users/louyangbo/Library/Application Support/typora-user-images/image-20220810155612549.png)

在DIff过程中通过对比先后的DOM（这里指的是旧树和WorkInProgress树，边进行对比，边标记effectTag和使用nextEffect链接起来）

**虚拟DOM的优势**

diff算法+批量处理策略

**虚拟DOM就一定会快吗**

diff算法需要耗费时间，因此快不快还是要看实际情况

 ## ⚠️React的diff算法

首先React不同于传统的diff算法，制定了三大策略，使得将时间复杂度O(n^3)转化为O(n)

- Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计（**tree diff**）
- 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构（**component diff**）
- 对于同一层级的一组子节点，它们可以通过唯一 id 进行区分（**element diff**）

**tree diff**

对DOM树进行分层对比，只会对比同一层次的节点。并且仅只有删除和创建两种操作

开发过程中建议：为了保持良好的性能，减少跨层的操作，尽量保持稳定的DOM结构，使用隐藏或显示，而不是真正的删除和添加

**component diff**

即对于同一类型的组件，继续按照原策略进行比较虚拟DOM树，对于不同类型的组件，将不再往下比较而是整个删除，重新创建

开发过程中建议：为了保证良好的性能，不同类型的component应当避免出现相似的DOM tree结构

**element diff**

通过key对比新老DOM，如果有相同的存在的话，再判断是否需要移动位置（错位的节点只进行向后插入，从新队列第一个节点起，依次往后保留老队列中相对先后顺序不变的节点，相对位置改变的节点后插）

开发过程中建议：为了保证性能，开发过程中尽量减少尾部数据向前调整位置的操作

## ⚠️React中key相关问题

**虚拟DOM中key的作用**

简单的说；key是虚拟DOM对象的标识，在更新时key起着极其重要的作用。

详细的说；当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】，随后React进行【新虚拟DOM】与【旧虚拟DOM】的diffing比较，比较规则如下：

a.旧虚拟DOM中找到了与新虚拟DOM相同的key：

若虚拟DOM中内容没变，直接使用之前的真实DOM；若虚拟DOM中内容变了，则声称新的真实DOM，随后替换掉页面中之前的真实DOM

b.旧虚拟DOM中未找到与新虚拟DOM相同的key：

根据数据创建新的真实DOM，随后渲染到页面

**用index作为key可能引发的问题**

若对数据进行：逆序添加、逆序删除等迫害顺序的操作（头插，所有的index都要后退一位，也就是key全部都错位了，导致页面全部都要重新加载一边）：会产生没有必要的真实DOM跟新==》界面效果没有问题，但是效率低。

如果结构中还包含输入类的DOM（输入框未更新）：会产生错误的DOM更新==〉界面有问题。

## ⭐️单向数据流/单向绑定

**单向数据流**

数据只能从父组件传递给子组件

**单向绑定**

- 状态变化->页面更新（setState所干的事情）
- React不存在这个数据流：页面更新->状态变化(需要手动去添加，如添加监听事件)

## 📎React组件创建

- React.createClass
- 继承React.Component
- 函数组件

## 📎React代码复用

**Mixin**

```js
var SetIntervalMixin = {
    componentWillMount: function() {
        this.intervals = [];
    },
    setInterval: function() {
        this.intervals.push(setInterval.apply(null, arguments));
    },
    componentWillUnmount: function() {
        this.intervals.map(clearInterval);
    }
};

var TickTock = React.createClass({
  //复用的代码使用mixins进行引入
    mixins: [SetIntervalMixin]
    render: function() {
        return ();
    }
});
```

优点：起到代码复用的功能

缺点：隐式依赖、命名冲突、只能在React.createClass中进行使用

**HOC**

```js
const withContext = (Component) => {
  return (props) => (
    <AppContext.Consumer>
      {({ state, actions }) => {
        return <Component {...props} data={state} actions={actions} />
      }}
    </AppContext.Consumer>
  )
}
export default withContext(Home)
```

优点：容器组件与展示分离、可以给函数式组件添加state，响应生命周期

缺点：不直观，难以阅读、名字冲突、组件层层嵌套、产生多余的组件，影响性能、多个HOC一同使用时，无法判断子组件的props是哪个HOC负责传递的

**Render Prop**

```js
class Mouse extends React.Component {
  static propTypes = {
    render: PropTypes.func.isRequired
  }

  state = { x: 0, y: 0 }

  handleMouseMove = (event) => {
    this.setState({
      x: event.clientX,
      y: event.clientY
    })
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>
        {this.props.render(this.state)}
      </div>
    )
  }
}

const App = () => (
  <div style={{ height: '100%' }}>
    <Mouse render={({ x, y }) => (
      <h1>The mouse position is ({x}, {y})</h1>
    )}/>
  </div>
)
```

优点：可以给函数式组件添加state，响应生命周期、不会产生无用的空组件、不用担心props命名问题，因为不会产生层层嵌套

缺点：难以阅读、会抵消React.PureComponent所带来的优势（因为传递函数，浅比较每次都不一样）

**Custom Hook**

优点：不会出现复用内容因为重名而相互覆盖、避免层级嵌套，让组件更容易理解，毕竟是函数，你可以直接取出来用、使用函数代替class，更符合react函数式理念

缺点：只能在顶层中使用hooks，只能在react函数中使用hooks

## ⭐️React中触发render

shouldComponentUpdate和memo都可以控制render

- 初始化

- setState()在任何情况下都会导致组件的重新渲染，即使state未发生变化


- 只要父组件重新渲染了，那么子组件就会重新渲染（添加React.memo在无props的情况下可以不渲染，有props，想要props不变的情况下不渲染，则需要使用React.memo和useCallback｜useMemo）

## ⚠️React中的Ref

**ref的作用：**

由于React推崇的是受控组件的形式，因此优先使用数据流的来控制子组件，但是聚焦关机视频播放等操作没发使用数据流的形式进行控制，这时候就需要使用ref，React提供ref的方式可以让我们安全获取DOM元素或组件的句柄。

- React通过Props的单向数据流控制子组件（声明式）
- React通过ref控制子组件（命令式）

**ref的使用场景：**

优先使用声明式的Props，尽量减少使用ref

- 管理焦点、视频播放
- 操作dom进行的动画
- 集成第三方的dom库
- 暴露子组件中的方法

**获取Ref的方法：**

```jsx
//String ref 根组件上使用无效｜无法被组合
<input ref="myRef"/>
this.refs.myRef
//Callback ref
<input ref={e=>{this.myRef=e}}/>
this.myRef
//React.createRef
this.myRef = React.createRef();
<input ref={this.myRef} />
//函数式组件中useRef
const myRef = useRef(null)
<input ref={myRef} />
```

## ⚠️setState同步/异步

setState本身是同步执行的，只是在某些环境下调用的顺序在更新之前，没法马上拿到更新之后的值。因此就形成了所谓的异步。

**为什么要使用setState来更新状态，而不是直接使用this.state.count = 1？**

保证内部的一致性，即使状态同步更新，最后的props也不是（只有当父组件进行render的时候才能知道props）

**为什么会形成异步？**

React对其做了性能优化（批量预处理，将每一个需要更新的state放入一个队列中，批量预处理将会在同步代码执行完成以后进行更新state，因此同步代码无法获取到最新的state）

**如何立即获取更新后的值**

使用setState的第二个参数

如果是函数式组件中可以用如下的方法

```js
export default function App() {
  const [nums, setNums] = useState(1);
  const nums_now = useRef(null)
  //执行顺序，setNums(nums+1),render,useEffect跟新ref的值，执行print()
  useEffect(()=>{
    nums_now.current = nums
  },[nums])
  const add = () =>{
    setNums(nums+1)
    setTimeout(()=>print(),0)
  }
  //单独封装一个函数是为了不生成闭包
  const print = () =>{
    console.log(nums_now.current)
  }
  
  return (
    <div className="App">
      <button onClick={add}>点击</button>
    </div>
  );
}
```

**React18之前**

isBatchingUpdates == true：合成事件、生命周期函数

isBatchingUpdates == false：原生事件、setTimeout、setInterval、Promise

**React18之后**

isBatchingUpdates == true：合成事件、原生事件、生命周期函数、Promise、setTimout

可以使用flushSync方法进行禁止

## ⭐️React三要素

**state：**存储组件的特有信息

**props：**用于组件之间的通信

**ref：**提供一种操作dom的方式

## ⭐️类组件中this绑定

**1.如果函数不是箭头函数**

state是定义在实例身上的，但是Component组件中的方法如果想要访问实例身上的state，需要在构造器中将这些方法使用bind改变this的指向。

```js
//之后都使用新函数，老函数使用bind改变this的指向
this.new_changeWeather = this.changeWeather.bind(this);
```

**2.如果函数是箭头函数**

箭头函数中的this会指向其所在的环境，因此箭头函数中this指向组件

## ⚠️React.Memo

类似于preComponent的作用

父组件的render都会导致子组件的render，使用React.memo包裹的组件，如果没有props的话，父组件render子组件将不会进行render

如果有props想要起到preComponent的作用，则需要同时使用React.memo和useMemo/useCallback

## ⚠️React生命周期

**旧生命周期**

-  组件挂载：getDefaultProps、getInitialState、componentWillMount、render、componentDidMount
- 组件更新：componentWillReceiveProps、shouldComponentUpdate、CompoentWillUpdate、render、componentDidUpdate
- 组件卸载：componentWillUnmount

**新生命周期**（fiber出现的原因，会打断Reconcilation阶段，某些生命周期将会重复执行）

- 组件挂载：getDerivedStateFromProps、render、componentDidMount
- 组件更新：getDerivedStateFromProps、shouldComponentUpdata、render、getSnapshotBeforeUpdate、update、componentDidUpdate
- componentWillUnmount