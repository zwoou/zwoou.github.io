# react

[TOC]



## var、let、const区别

- var定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问。

- let定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问。

- const用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改。



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

- 装载过程（Mount）

- 更新过程（Update）

- 卸载过程（Unmount)

  

### 装载过程

- constructor
- getInitialState
- getDefaultProps
- componentWillMount
- render
- componentDidMount

## React Router

[https://reactrouter.com/](https://reactrouter.com/)

