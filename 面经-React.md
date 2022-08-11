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

## 📎ErrorBoundaries

任何未被错误边界捕获的错误都将导致整个React组件树被卸载

实现一个错误边界，需要实现两个生命周期中的任意一个

```javascript
//会在渲染阶段进行调用，不允许出现副作用
static getDerivedStateFromError(error)  //用于渲染备用的UI
//提交阶段被调用，允许执行副作用
componentDidCatch(error, errorInfo)  //用于打印错误的信息
```

## 📎dangerouslySetInnerHTML

dangerouslySetInnerHTML是React为浏览器DOM提供innerHTML的替换方案（我猜测是有防止XSS攻击的功能）

将需要显示的内容通过key为__html进行传递

## ⚠️React受控/非受控组件

react的受控组件和非受控组件指的是相对于表单而言的，HTML中表单元素的工作方式和其他的DOM元素有些不同（表单均会保持一些内部的state）

**受控组件：**

如果将React中的state成为表单元素的“唯一数据源”，通过onChange事件和setState事件结合更新state属性，通过这种方式控制取值的表单输入元素叫做“受控组件”

```javascript
//这种方式，input中输入内容，input并不会更新，因为state并不会变化，只有state能控制input的显示
const Input = () => {
	const [username, setUsername] = useState(null)
	return (
		<input name = "username" value={username}/>
	)
}
//因此需要使用onChange事件实现“双向绑定”
const Input = () => {
	const [username, setUsername] = useState(null)
	return (
		<input name = "username" onChange={(event)=>setUsername(event.target.value)} value={username}/>
	)
}
//如果是作为一个组件进行使用，需要将控制权交付到父组件身上
const Father = () => {
  const [username, setUsername] = useState()
  return (
  	<Input username={username} setUsername={setUsername}>
  )
}
const Input = ({username,setUsername}) => {
	return (
		<input name = "username" onChange={(event)=>setUsername(event)} value={username}/>
	)
}
```

react中并没有双向绑定，无法做到用户在输入框中输入内容->数据同步更新，因此需要添加onChange事件来完成这一步

**非受控组件：**

如果表单元素并不是经过state，而是通过ref修改或者直接操作DOM，这种不通过state控制的组件叫做非受控组件

```javascript
const Input = () => {
  input = useRef()
  //通过方法得到input的值
  const getValue = () => {
    return input.current.value
  }
  return (
    //通过使用input内置的“state”控制input组件的value，可以通过使用defaultValue指定value的值
  	<input name="username" ref={input}/>
  )
}
```

## 👑React18新增特性

- **新增Render API**

  因为React18的版本变动较大，为了兼容旧版本，React在挂载根节点处，新增挂载借口，如果使用老接口进行挂载，则不会应用React18的新功能，只有通过新接口挂载才会应用新功能

  ```javascript
  import {render} from "react-dom"
  // React 17
  const container = document.getElementById("app")
  render(<App tab="home"/>, container)
  // React 18
  import {createRoot} from "react-dom/client"
  const container = document.getElementById("app")
  const root = createRoot(container) //引入createRoot API用来开启并发渲染模式
  root.render(<App tab="home"/>)
  ```

  组件卸载方式

  ```js
  // React 17
  ReactDOM.unmountComponentAtNode(root);
  // React 18
  root.unmount();
  ```

  移除了render的回调函数，需要使用useEffect来模拟该功能

- **AutomaticBatching**（自动批处理）

  React 18通过在默认情况下执行批处理来实现开箱即用的性能改进。批处理是指未来获得更好的性能，在数据层，将多个状态更新批量处理，合并成一次更新（视图层将多个渲染合并成一次渲染）

  18之前自动批处理：合成事件、 生命周期钩子

  18之后自动批处理：合成事件、生命周期钩子、promise、setTimeout、原生事件

- **flushSync**

  因为18给几乎所有的地方都添加上了自动批处理，如果想要禁止该功能，可以使用flushSync，每个flushSync包裹起来的setState为一次批量更新

  ```jsx
  <div
    onClick={() => {
      flushSync(() => {
      	setCount1(count => count + 1);
      });
      // 第一次更新
      flushSync(() => {
      	setCount2(count => count + 1);
      });
      // 第二次更新
    }}
  >
  ```

- **组件卸载警告**

- **React组件的返回值**

  React18中对空组件的返回值，null和undefi均允许

- **Strict Mode**

  严格模式下，React会对每个组件渲染两次

- **Suspense不再需要fallback**

  React18之前 Suspense没有提供fallback属性会被React跳过，React18之后则不会再被跳过了

- **Concurrent Mode**（并发模式）

- **useTransition**

  主要是为了能在大量的任务下也能保持UI响应，可以将setState标记为不紧急渲染，其可以被其他紧急渲染所抢占

- **useDeferredValue**

  将值变成延时状态

## 👑Hooks的原理

Hooks让函数组件也拥有了state

**函数组件的state是存储在哪里的？**

存储在fiber的memoizedState中，通过next串联链表形式进行保存

执行的时候各自在自己的memoizedState上存取数据，完成各种逻辑

**memoizedState中的链表何时进行创建？**

首次useState的时候执行mountState会进行创建（不同的钩子调用不同的mountXXX进行创建，如mountEffect、mountRef、mountMemo等）

后续调用useState只会执行updateState

## 👑mini-useState

```js
//用于存储state的数组
let state = [];
//用于存储set方法的数组
let setters = [];
//用于记录页面是否是首次渲染
let firstRun = true;
//用于记录指针，连接set方法和state状态
let cursor = 0;
//返回创建set方法，使用cursor来指示该set方法可以更改哪个位置的state
function createSetter(cursor) {
  return function setterWithCursor(newVal) {
    state[cursor] = newVal;
  };
}

//实现useState函数，
export function useState(initVal) {
  //页面首次加载，将state和set方法push进入数组
  if (firstRun) {
    state.push(initVal);
    setters.push(createSetter(cursor));
    firstRun = false;
  }
  //取出state和set方法，返回
  const setter = setters[cursor];
  const value = state[cursor];
  cursor++;
  return [value, setter];
}

// Our component code that uses hooks
function RenderFunctionComponent() {
  const [firstName, setFirstName] = useState("Rudi"); // cursor: 0
  const [lastName, setLastName] = useState("Yardley"); // cursor: 1

  return (
    <div>
      <button onClick={() => setFirstName("Richard")}>Richard</button>
      <button onClick={() => setFirstName("Fred")}>Fred</button>
    </div>
  );
}

// This is sort of simulating Reacts rendering cycle
function MyComponent() {
  cursor = 0; // resetting the cursor
  return <RenderFunctionComponent />; // render
}

console.log(state); // 未渲染：Pre-render: []
MyComponent();
console.log(state); // 首次渲染: ['Rudi', 'Yardley']
MyComponent();
console.log(state); // 第二次渲染: ['Rudi', 'Yardley']
console.log(state); // 点击之后: ['Fred', 'Yardley']
```

## ⚠️useRef

useRef应用场景：

- 用于保存一个变量，使其在组件整个生命周期中不会进行变动
- 用于安全获取组件的句柄

useRef为什么能够保存一个值在整个生命周期中不会变动

首先所有的hooks都是挂载到fiber对象上的memoizedState上面，并且通过调用mountRef，mountRef中通过创建ref对象，将value赋予对象中的current，将对象的索引赋予memoizedState，其实ref就是用来保存一个对象的索引，这样内部的current的值就不会变动了

一般是配合forwardRef+useImperativeHandle

```javascript
//函数式组件式不能绑定ref,因为函数组件没有实例，当为类组件时，指向类组件的实例，因此需要使用forwardRef
//forwardRef用于在函数式组件中传递ref，绑定到子组件中的组件
const Foo = forwardRef((props,ref)=>{
    return(
      <div>
        <input type="text" ref={ref}/>
      </div>
    )
})

//手动绑定指定的ref
//forwardRef一般useImperativeHandle，来实现类式组件中，通过ref来调用子组件身上的方法
const Foo = forwardRef((props,ref)=>{
  const inputRef = useRef()
  //强制绑定ref，此时ref就是{focus:function}对象
  useImperativeHandle(ref,()=>{
    return {
      focus:()=>{
        inputRef.current.focus()
      }
    }
  })
    return(
      <div>
        <input type="text" ref={inputRef}/>
      </div>
    )
})
```

## 👑useEffect闭包问题

useEffect、useCallback、useMemo均存在闭包问题

```tsx
export const Test = (){
  const [num,setNum] = useState(0)
  const add = () => setNum(num+1)
  //永远都是0
  //页面加载的时候会执行一次，形成一个闭包，所以num就是页面加载那个num
	useEffect(()=>{
    setInterval(()=>{
      console.log('num in setInterval:',num)
    },1000)
  },[])
  // 不过怎么按button都是0
  //页面加载的时候会执行一次，形成一个闭包，所以num就是页面加载那个num
  useEffect(()=>{
    return ()=>{
      console.log(num)
    }
  },[])
  //解决方法：为了让num更新的时候能够让之前生成的闭包销毁，重新生成包含新num的闭包，因此要在依赖中添加num
  return <div>
  	<button onClick={add}>add</button>
    <p>{num}</p>
  </div>
}
```

## ⭐️useEffect/useLayoutEffect

render同步、useLayoutEffect微任务、useEffect宏任务

执行顺序：父组件render>子组件render> 子组件useLayoutEffect>父组件useLayoutEffect>子组件useEffect>父组件useEffect

## ⚠️useCallback/useMemo

**useMemo:**

1.同步（因为是同步所以不能在里面操作DOM之类的副作用）

2.渲染前调用（shouldComponentUpdate功能）

useMemo返回的的是一个值，用于避免在每次渲染时都进行高开销的计算。在子组件中使用 shouldComponentUpdate， 判定该组件的 props 和 state 是否有变化，从而避免每次父组件render时都去重新渲染子组件。

```jsx
const result = useMemo(() => {
    for (let i = 0; i < 100000; i++) {
      (num * Math.pow(2, 15)) / 9;
    }
}, [num]);
```

**useCallback:**

useCallback返回一个函数，当把它返回的这个函数作为子组件使用时，可以避免每次父组件更新时都重新渲染这个子组件。是特殊版本的useMemo

```jsx
const renderButton = useCallback(
     () => (
         <Button type="link">
            {buttonText}
         </Button>
     ),
     [buttonText]    // 当buttonText改变时才重新渲染renderButton
);
```

## ⚠️useEvent

**useEvent：**

解决useCallback的闭包问题：

- 函数在整个生命周期内永久存活
- 保证拿到最新的state

老版本中React为了解决该问题提出的方案

```javascript
function useEventCallback(fn, dependencies) {
  const ref = useRef(() => {
    throw new Error('Cannot call an event handler while rendering.');
  });

  useEffect(() => {
    ref.current = fn;
  }, [fn, ...dependencies]);

  return useCallback(() => {
    //通过ref保存方法的饮用，就可以避免闭包问题
    const fn = ref.current;
    return fn();
  }, [ref]);
}
```

RDC功能中可以使用useEvent

**useEvent中到底使用useLayoutEffect还是useEffect**

均不符合预期，由于父组件将使用useEvent包裹的函数通过props传递给子组件时，由于执行顺序为

父组件render>子组件render> 子组件useLayoutEffect>父组件useLayoutEffect>子组件useEffect>父组件useEffect

因此子组件的render和useLayoutEffect均在useEvent将函数赋予ref之前，如果在这两个阶段读取props中的函数，将会不符合期望

答：就算是使用useLayoutEffect和useEffect均会出现不符合预期的结果，目前第三方的Hooks实现都是放在render中的

```js
function useEventCallback(fn, dependencies) {
  const ref = useRef(() => {
    throw new Error('Cannot call an event handler while rendering.');
  });
	// 不包裹任何副作用，ref在父组件进行render的时候就赋值了
  ref.current = fn;

  return useCallback(() => {
    //通过ref保存方法的饮用，就可以避免闭包问题
    const fn = ref.current;
    return fn();
  }, [ref]);
}
```

## 📎useState惰性渲染

函数签名：

```tsx
function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];
```

添加函数只是惰性渲染。`initialState` 参数只会在组件的初始渲染中起作用，后续渲染时会被忽略。如果初始 state 需要通过复杂计算获得，则可以传入一个函数，在函数中计算并返回初始的 state，此函数只在初始渲染时被调用.

所以要在useState中保存函数，则需要再加一层函数

```tsx
const [fun, setfun] = React.useState(()=>()=>{console.log('我是初始化函数')}
```

## 📎setState两种写法

```tsx
const [state, setState] = useState({
  stat:"idle"
})
//但是这样写涉及了state，如果放在useEffect中，并且state放入依赖中，就回导致无限渲染
useEffect(()=>{
  setState({...state, stat:"loading"})
},[state])
//安全写法useReducer用的就是这种写法
//这样不会直接涉及到state
setState(prevState => ({...prevState, stat: "loading"}))
```

## ⭐️useReducer

是useState的替代方案。使用useReduce可以将操作状态的实现交给reducer，使用者只需要提供action（操作状态的方法的别名和所需要提供的数据即可）

useState适合定义单个状态，useReducer适合定义一群会互相影响的状态。

```tsx
import { useCallback, useReducer } from "react";

const UNDO = "UNDO";
const REDO = "REDO";
const SET = "SET";
const RESET = "RESET";

type State<T> = {
  past: T[];
  present: T;
  future: T[];
};

type Action<T> = {
  newPresent?: T;
  type: typeof UNDO | typeof REDO | typeof SET | typeof RESET;
};
//（state,action）=>newState的reducer
//state是现在的state值，action是操作方法和需要的数据，需要返回新的state的值
const undoReducer = <T>(state: State<T>, action: Action<T>) => {
  //下面就是所有操作state的方法
  const { past, present, future } = state;
  const { newPresent } = action;
	//配套的dipatch的方法，工作机制类似于setState的第二种用法
  switch (action.type) {
    case UNDO: {
      if (past.length === 0) return state;

      const previous = past[past.length - 1];
      const newPast = past.slice(0, past.length - 1);

      return {
        past: newPast,
        present: previous,
        future: [present, ...future],
      };
    }

    case REDO: {
      if (future.length === 0) return state;

      const next = future[0];
      const newFuture = future.slice(1);

      return {
        past: [...past, present],
        present: next,
        future: newFuture,
      };
    }

    case SET: {
      if (newPresent === present) {
        return state;
      }
      return {
        past: [...past, present],
        present: newPresent,
        future: [],
      };
    }

    case RESET: {
      return {
        past: [],
        present: newPresent,
        future: [],
      };
    }
  }
  return state;
};

export const useUndo = <T>(initialPresent: T) => {
  //useReducer有两个参数，第一个是上面定义的undoReducer，另一个是状态的初始值
  const [state, dispatch] = useReducer(undoReducer, {
    past: [],
    present: initialPresent,
    future: [],
  } as State<T>);

  const canUndo = state.past.length !== 0;
  const canRedo = state.future.length !== 0;

  const undo = useCallback(() => dispatch({ type: UNDO }), []);

  const redo = useCallback(() => dispatch({ type: REDO }), []);

  const set = useCallback(
    (newPresent: T) => dispatch({ type: SET, newPresent }), []);

  const reset = useCallback(
    (newPresent: T) => dispatch({ type: RESET, newPresent }), []);

  return [state, { set, reset, undo, redo, canUndo, canRedo }] as const;
};
```

## 👑useEffect模拟生命周期

```js
//仅在首次渲染之后进行执行（仅执行一次）
useEffect(()=>{
    console.log('hello')
},[])
```

```js
//每次页面渲染都会执行
useEffect(()=>{
    console.log('hello')
})
```

```js
//监听num，num变化的时候才会执行
useEffect(()=>{
    console.log('hello')
},[num])
```

```js
//模拟componentDidMount
useEffect(()=>{
    console.log('componentDidMount')
},[])
//模拟componentWillUnmount
useEffect(()=>{
    return ()=>{
        console.log('componentWillUnmount')
    }
})
//模拟componentDidUpdate
const useMounted = () => {
    const [mounted, setMounted] = useState(false);
    useEffect(() => {
        !mounted && setMounted(true);
        return () => setMounted(false);
    }, []);
    return mounted;
}
const mounted = useMounted() 
useEffect(() => {
    mounted && fn()
})
```

## 📎函数/类组件setState的区别

**类式组件**

```javascript
this.state = {
  name:'lyb',
  age:14
}
changeState = () => {
  this.setState({name:'qxl'}) //自动合并，并生成新的对象
}
```

**函数式组件**

```javascript
const [state, setState] = useState({
  name:'lyb',
  age:14
})
const changeState = () => {
	setState({age:13})//直接进行替换，并不会合并，因此在使用函数式组件的setState的时候一般会使用到Object.assign
  //setState(Object.assign({},{...state,name:'qxl'}))
}
```

## 📎Portal

提供一种将子节点渲染到存在于父组件以外的DOM节点的优秀方案

```javascript
//第一个child是任何可渲染的React子元素，字符串或fragment，第二个参数container是DOM元素
ReactDOM.createPortal(child, container)
```

通常情况下子组件将会被渲染在离他最近的父组件下，但是当父组件有overflow：hidden或者z-index的时候，子组件的显示将会受到影响（例如对话框、悬浮卡和提示框等），需要脱离父类css的样式

通过protal将元素放置DOM树中的任何位置，但是其其他行为和普通的React字节点行为一样，protal仍然能够在React树中进行，尽管这些React树的祖先不是DOM树中的祖先

因此Protal可以脱离父组件的css样式的影响，又可以保持原有的事件处理能力