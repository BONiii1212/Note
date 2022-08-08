## fiber架构

参考文献：

https://juejin.cn/post/6844903975112671239#heading-11

https://juejin.cn/post/7046217872833511454

React16版本引入了Fiber架构

❓为什么要引入Fiber架构？

React16之前，React的递归比较VirtualDOM树，找出变动节点，并更新他们，是同步执行一气呵成的。React的这个Reconcilation阶段会霸占整个浏览器资源，并导致用户触发事件得不到响应，二则会导致掉帧，极度影响用户的体验

❓React官方处理方法？

通过按一定时间分片（60帧以上人的视觉区别效果不大，因此这里俺16ms进行时间分片），在每个16ms中，先执行页面的渲染和绘制，如果有多余的时间，再来执行React的Reconcilation阶段的操作。并且超时后归还浏览器线程的执行权。

存在问题：

- 目前浏览器，并没有中断执行，和抢回执行权的方法。（React主动让出，由于requestIdleCallback API兼容性不好，React自己实现了MessageChannel）
- 由于需要中断Reconcilation阶段的操作，因此需要设计中断和恢复的机制，但是React的diff使用的是递归的方式，很难做到下次获得线程执行权后恢复操作。（模仿函数调用栈设计了Fiber结构，Fiber结构中有上下文信息（父子节点）更方便实现恢复操作）
- 不同的任务同样存在优先级，如何确定各任务的优先级，并保证由于优先级而打乱执行顺序后，state的最终状态不会收到影响

FIber架构下整个更新流程：

![image-20220805205255008](/Users/louyangbo/Library/Application Support/typora-user-images/image-20220805205255008.png)

1.用户通过Scheduler.sheduleCallback生成task任务（一般是setState操作？），有delay参数的会先进入timmer队列等待，没有delay参数的会直接进去taskQueue队列（这两个队列均通过小顶堆实现），每个任务都设置了一个超时时间expirationTime（防止饿死，同时也是用来控制优先级的，默认情况下优先级为文本框输入>本次调度结束需要完成的任务>动画过渡>交互反馈>数据更新>不会显示但以防将来会显示的任务，同时也可以使用React18中的新特性transition API进行控制）

```js
// dom更新队列
updateQueue.push(task);
```

2.当以每一帧16ms切片的时间段内，如果执行完布局和绘制等操作后，还有盈余的时间时，会执行requestIdleCallback请求浏览器进行调度

```js
requestIdleCallback(performWork, {timeout});
```

3.队列是通过小顶堆维护的，因此依次拿出优先级最高的task调用workLoop进行执行，并且每次执行一个task都会判断是否有盈余的时间继续执行下一个，如果本次16ms结束了，队列还有剩余的，那就申请下一次的调度

```js
function performWork(deadline) {
	// 如果更新队列中有任务，并且每一个切片中还有足够的时间给你执行
  while (updateQueue.length > 0 && deadline.timeRemaining() > ENOUGH_TIME) {
    workLoop(deadline);
  }
  // 如果本次切片剩余时间没能全部执行完更新队列中所有的更新请求，继续请求浏览器的调度，在下一个切片中去执行
  if (updateQueue.length > 0) {
    requestIdleCallback(performWork);
  }
}
```

4.workLoop内部用来依次执行执行Fiber单元（每次跟新涉及一个DOM树，每个DOM节点就是一个Fiber单元）

```js
let nextUnitOfWork: Fiber | undefined // 保存下一个需要处理的工作单元
let topWork: Fiber | undefined        // 保存第一个工作单元（一个任务中分为好多个fiber）

function workLoop(deadline: IdleDeadline) {
  // updateQueue中获取下一个或者恢复上一次中断的执行单元
  if (nextUnitOfWork == null) {
    nextUnitOfWork = topWork = getNextUnitOfWork();
  }
  // 每执行完一个执行单元，检查一次剩余时间
  // 如果被中断，下一次执行还是从 nextUnitOfWork 开始处理
  while (nextUnitOfWork && deadline.timeRemaining() > ENOUGH_TIME) {
    // 下文我们再看performUnitOfWork
    nextUnitOfWork = performUnitOfWork(nextUnitOfWork, topWork);
  }

  // 提交工作，下文会介绍
  if (pendingCommit) {
    commitAllWork(pendingCommit);
  }
}
```

5.performUnitOfWork是一个以迭代的方式来处理这些节点（深度优先遍历，迭代方式能够解决中断后不易恢复的缺点，依赖于新的fiber结构）

```js
/**
 * @params fiber 当前需要处理的节点
 * @params topWork 本次更新的根节点
 */
function performUnitOfWork(fiber: Fiber, topWork: Fiber) {
  // 对该节点进行处理
  beginWork(fiber);
  // 如果存在子节点，那么下一个待处理的就是子节点
  if (fiber.child) {
    return fiber.child;
  }
  // 没有子节点了，上溯查找兄弟节点
  let temp = fiber;
  while (temp) {
    completeWork(temp);
    // 到顶层节点了, 退出
    if (temp === topWork) {
      break
    }
    // 找到，下一个要处理的就是兄弟节点
    if (temp.sibling) {
      return temp.sibling;
    }
    // 没有, 继续上溯
    temp = temp.return;
  }
}
```

5.beginWork会依次处理每个Fiber节点，不同的节点进行不同的diff算法

```js
function beginWork(fiber: Fiber): Fiber | undefined {
  if (fiber.tag === WorkTag.HostComponent) {
    // 宿主节点diff
    diffHostComponent(fiber)
  } else if (fiber.tag === WorkTag.ClassComponent) {
    // 类组件节点diff
    //diffClassComponent中会调用类组件的生命周期函数，又因为中断后恢复，如果有更高优先级的，会执行更高优先级的，改task将被重制，从头开始，因此这里会被多次调用
    diffClassComponent(fiber) 
  } else if (fiber.tag === WorkTag.FunctionComponent) {
    // 函数组件节点diff
    diffFunctionalComponent(fiber)
  } else {
    // ... 其他类型节点，省略
  }
}
```

6.performUnitOfWork就是通过迭代的方式遍历workInProgress，然后对比旧树中的节点，一边比较一边构建新树，然后在Fiber节点的effectTag中打上更新标记，并用链表的形式将所有有副作用的Fiber结构串联起来，后面commit阶段仅需跟着这个链表来进行跟新就行了。（具体比较的方法略）

![image-20220805152714602](/Users/louyangbo/Library/Application Support/typora-user-images/image-20220805152714602.png)

#### Fiber结构

```tsx
interface Fiber {
  // ⚛️ 节点的类型信息
  // 标记 Fiber 类型, 例如函数组件、类组件、宿主组件
  tag: WorkTag,
  // 节点元素类型, 是具体的类组件、函数组件、宿主组件(字符串)
  type: any,

	// ⚛️ 结构信息 
  return: Fiber | null,
  child: Fiber | null,
  sibling: Fiber | null,
  // 子节点的唯一键, 即我们渲染列表传入的key属性
  key: null | string,

	// ⚛️节点状态
  // 节点实例(状态)：
  //        对于宿主组件，这里保存宿主组件的实例, 例如DOM节点。
  //        对于类组件来说，这里保存类组件的实例
  //        对于函数组件说，这里为空，因为函数组件没有实例
  stateNode: any,
  // 新的、待处理的props
  pendingProps: any,
  // 上一次渲染的props
  memoizedProps: any, 
  // 上一次渲染的组件状态
  memoizedState: any,

  // ⚛️ 副作用（diff过程中对需要变更的节点打上标记）
  // 当前节点的副作用类型，例如节点更新、删除、移动
  effectTag: SideEffectTag,
  // 和节点关系一样，React 同样使用链表来将所有有副作用的Fiber连接起来
  nextEffect: Fiber | null,

  // ⚛️ 替身
  // 指向旧树中的节点（react的diff过程就是一边和旧树比较一遍构建新树）
  alternate: Fiber | null,
}
```

#### requestIdleCallback API

```tsx
window.requestIdleCallback(
  callback: (dealine: IdleDeadline) => void,
  option?: {timeout: number} // 任务繁忙的时候，没有盈余时间，当超过该时间将
)
interface IdleDealine { // 回调中传入的期限
  didTimeout: boolean // 表示任务执行是否超过约定时间
  timeRemaining(): DOMHighResTimeStamp // 任务可供执行的剩余时间
}
```

#### MessageChannel

```tsx
const el = document.getElementById('root')
const btn = document.getElementById('btn')
const ch = new MessageChannel()
let pendingCallback
let startTime
let timeout

ch.port2.onmessage = function work()  {
  // 在绘制之后被执行
  if (pendingCallback) {
    const now = performance.now()
    // 通过now - startTime可以计算出requestAnimationFrame到绘制结束的执行时间
    // 通过这些数据来计算剩余时间
    // 另外还要处理超时(timeout)，避免任务被饿死
    // ...
    if (hasRemain && noTimeout) {
      pendingCallback(deadline)
    }
  }
}

// ...

function simpleRequestIdleCallback(callback, timeout) {
  requestAnimationFrame(function animation() {
    // 在绘制之前被执行
    // 记录开始时间
    startTime = performance.now()
    timeout = timeout
    dosomething()
    // 调度回调到绘制结束后执行
    pendingCallback = callback
    ch.port1.postMessage('hello')
  })
}
```
