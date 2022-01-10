# react

[TOC]





## 官网

[https://reactjs.org/](https://reactjs.org/)

### Create React App



```
npx create-react-app my-app
cd my-app
npm start
```

## 组件的数据

### state

用于记录内部状态

### prop

用于定义外部接口，组件不应该改变prop的值

## 组件的生命周期

生命周期可能会经历三个过程：

- 挂载过程（Mount）

- 更新过程（Update）

- 卸载过程（Unmount)

  

### 挂载过程

当组件实例被创建并插入DOM中时，其生命周期调用顺序如下：

- **constructor()**
- static getDerivedStateFromProps()
- ~~componentWillMount~~  过时
- **render()**
- **componentDidMount()**

## 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- static getDerivedStateFromProps()
- shouldComponentUpdate()
- **render()**
- getSnapshotBeforeUpdate()
- **componentDidUpdate()**

## 卸载

当组件从 DOM 中移除时会调用如下方法：

- **componentWillUnmount()**



## React Router

[https://reactrouter.com/](https://reactrouter.com/)

### Path匹配



