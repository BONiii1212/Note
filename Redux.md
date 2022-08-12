# React-Redux

## 简介

一个可预测的状态容器

可预测：更改状态均使用纯函数进行修改

- **创建reducer**（用于根据不同的action的type类型来对state做出变动）
- createStore接收一个reducer来**创建store**（store提供subscribe、dispatch和getState方法）
- **创建Action**

![image-20220812133418539](/Users/louyangbo/Library/Application Support/typora-user-images/image-20220812133418539.png)

**为什么React相关面试总会问Redux？**

因为redux推崇一种容器组件与展示组件分离的思想（即HOC的思想）！！！

## Provider

这个Provider就和Context的Provider是一样的

将顶层组件包裹在Provider组件中，并将store作为参数放到Provider组件中，这样子组件均可以访问到Redux中的数据

Provider是一个react组件，仅接收store一个方法

Provider中包含三个方法：constructor构造器，getChildContext（用于返回store方法）、render（简简单单进行一个子节点的返回，render中使用Children.only限制只能有一个字节点）

```javascript
import { Component, Children } from 'react'
    import PropTypes from 'prop-types'
    import storeShape from '../utils/storeShape'
    import warning from '../utils/warning'
    
    let didWarnAboutReceivingStore = false
    function warnAboutReceivingStore() {
      if (didWarnAboutReceivingStore) {
        return
      }
      didWarnAboutReceivingStore = true
    
      warning(
        '<Provider> does not support changing `store` on the fly. ' +
        'It is most likely that you see this error because you updated to ' +
        'Redux 2.x and React Redux 2.x which no longer hot reload reducers ' +
        'automatically. See https://github.com/reactjs/react-redux/releases/' +
        'tag/v2.0.0 for the migration instructions.'
      )
    }
    // Provider组件
    export default class Provider extends Component {
      getChildContext() {
        return { store: this.store }
      }
    	
      constructor(props, context) {
        super(props, context)
        this.store = props.store
      }
    
      render() {
        // 简简单单进行一个子节点的返回，render中使用Children.only限制只能有一个字节点
        return Children.only(this.props.children)
      }
    }
    // 如果是开发环境，添加生命周期钩子，用于警告store变更行为
    if (process.env.NODE_ENV !== 'production') {
      Provider.prototype.componentWillReceiveProps = function (nextProps) {
        const { store } = this
        const { store: nextStore } = nextProps
    
        if (store !== nextStore) {
          warnAboutReceivingStore()
        }
      }
    }
    // 对Provider的prop类型进行限制
    Provider.propTypes = {
      store: storeShape.isRequired,
      children: PropTypes.element.isRequired
    }
		// 通过设置childContextTypes和getChildContext，React自动向下传递信息，子树上的所有组件可以通过定义contextTypes来访问context
    Provider.childContextTypes = {
      store: storeShape.isRequired
    }
```

## connect

真正意义上的链接React组件和Redux store

connect是一个高阶函数，首先传入mapStateToProps、mapDispatchToProps，然后返回一个生产Component的函数(wrapWithConnect)，然后再将真正的Component作为参数传入wrapWithConnect，这样就生产出一个经过包裹的Connect组件

```jsx
connect(mapStateToProps, mapDispatchToProps)(MyComponent)
```

- **mapStateToProps**

  ```js
  // 参数为redux的store和组件自己的props
  // 只要state或者自己的props变动就会导致mapStateToProps被调用生成一个新的stateProps
  mapStateToProps(state, ownProps) : stateProps
  const mapStateToProps = (state) => {
    return {
      count: state.count
    }
  }
  ```

  把state映射到props中

- **mapDispatchToProps**

  把dispatch方法映射到props中

```js
export default function connect(mapStateToProps, mapDispatchToProps, mergeProps, options = {}) {
  return function wrapWithConnect(WrappedComponent) {
    class Connect extends Component {
      constructor(props, context) {
        // 从祖先Component处获得store
        this.store = props.store || context.store
        this.stateProps = computeStateProps(this.store, props)
        this.dispatchProps = computeDispatchProps(this.store, props)
        this.state = { storeState: null }
        // 对stateProps、dispatchProps、parentProps进行合并
        this.updateState()
      }
      shouldComponentUpdate(nextProps, nextState) {
        // 进行判断，当数据发生改变时，Component重新渲染
        if (propsChanged || mapStateProducedChange || dispatchPropsChanged) {
          this.updateState(nextProps)
            return true
          }
        }
        componentDidMount() {
          // 改变Component的state
          //store变动的时候，执行回调，更新state的值
          this.store.subscribe(() = {
            this.setState({
              storeState: this.store.getState()
            })
          })
        }
        render() {
          // 生成包裹组件Connect
          return (
            <WrappedComponent {...this.nextState} />
          )
        }
      }
      Connect.contextTypes = {
        store: storeShape
      }
      return Connect;
    }
  }
```

## react-thunk

```js
function createThunkMiddleware(extraArgument) {
      return ({ dispatch, getState }) => next => action => {
        // 如果action是函数的话，thunk会去帮你执行，然后再去调用dispath
        if (typeof action === 'function') {
          return action(dispatch, getState, extraArgument);
        }
        return next(action);
      };
    }
    
    const thunk = createThunkMiddleware();
    thunk.withExtraArgument = createThunkMiddleware;
    
    export default thunk;
```

## immutable

immutable Data就是一旦创建，就不能再被更改的数据。对immutable对象的任何修改或删除操作都会返回一个新的immutable对象（保证原版本完好的前提下，获得新的版本），主要是为了解决深拷贝所导致的大量性能消耗

修改immutable Data仅仅会修改修改节点已经其父节点直至根节点，返回一个新的root节点的索引

## Redux Toolkit

是Redux的封装版