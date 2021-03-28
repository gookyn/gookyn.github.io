---
title: 关于 React Hook
tags: React
categories: React
---

## 概念


一些在函数组件中“钩入”React State和生命周期等特性的函数。


## 为什么选用Hook


**1、无需修改组件结构情况下复用状态逻辑** 

- 类组件：复用状态逻辑很复杂，比如高阶组件、context、render props等，需要调整布局逻辑；而且如果层级过深，很容易形成“嵌套地狱”。
- Hook：可以在不修改组件结构的情况下，提取复用状态逻辑。



**2、使用Effect解决复杂的生命周期及函数处理**

- 类组件：同一个生命周期中可能会有很多逻辑，比如在componentDidMount中会有请求数据、设置事件监听、定时器等，然后在componentWillUnmount中清除。完全不相关的逻辑组合在了同一个方法中，而相互关联的逻辑又被拆得很分散，非常难维护。
- Hook：内置已处理生命周期，不需要手动维护，Effect Hook执行的是颗粒化的函数逻辑，可以拆分不同的函数，比如接口请求、设置订阅、操作Dom等是执行不同的方法，并不需要按照生命周期划分；同时支持回调处理，比如取消订阅或取消定时，可以在回调中执行，逻辑关联性更强。



**3、删除class概念**

- 类组件：需要理解并使用this，还需要区分类组件和函数组件；class不能很好的压缩，热重载不稳定。
- Hook：不需要使用this，更多的是函数方案。



## 常用Hook


### State Hook

- 可以在函数组件中使用 state
- 不同于class组件的setState，useState不会自动合并更新对象，需要使用更新函数setXxx计算得出



### Effect Hook

- 合并`componentDidMount`、`componentDidUpdate`、`componentWillUnmount`生命周期
- 赋值给 useEffect 的函数，如数据获取、设置订阅、更新Dom等，会在组件渲染之后再执行
- useEffect 函数中可以返回一个清除函数，会在组件卸载之前执行，比如清除订阅、清除定时等
- 如果只想运行一次effect，可以传空数组[]作为第二个参数



### 其他

- useContext
- useReducer
- useCallback
- useMemo
- useRef
- useImperativeHandle
- useLayoutEffect
- useDebugValue



## 注意事项


- 只能在函数最外层调用，不能在循环、条件判断或者子函数中调用，需要保证多次渲染过程中调用的Hook顺序一致，这样React就可以正确地将内部state和对应的hook关联
- 不能在class组件中使用



## 参考


更多详细 React Hook：[https://react.docschina.org/docs/hooks-intro.html](https://react.docschina.org/docs/hooks-intro.html)
