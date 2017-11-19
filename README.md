#这是一个简单的对React官方最新版文档的翻译，可能以后会加上与16.1.1版本之前的版本的对比，现阶段仅限于翻译工作。

>由于本人能力有限，翻译中的不当之处欢迎各位提出建议，我将持续修改这份文档。

#### [快速上手](#fasler)

#### [React](#reactElement)

<span id="fasler"></span>

## 快速上手

我们知道，在16版本中React已经修改了component的生成方式，以前的
```
react.creactClass({});
```
方法已经不再支持，现在换成了
```
class X extends x{
}
```
这种es6语法.
以下例子有助于我们快速认识新语法：
```
class times extends React.Component{
    constructor(props){
        super(props);
        this.state = { seconds:0 };
    }
    trick(){
        this.setState(prevState => ({
            seconds: prevState.seconds + 1;
        }))
    }
    componentDidMount(){
        this.interval = setInterval(()=>this.trick(),1000);
    }
    componentWillUnmount(){
        clearInterval(this.interval);
    }
}   
```
<span id="reactElement"></span>

## React 顶级API
> React 是React框架的入口，假如你通过一个\<script>标签引入了React,这些顶级API就是全局可用的。假如你是使用的 NPM 引入的，那么你可以选择通过 es6 语法，以 import React from 'react';的方式引用React.也可以选择通过 es5 语法，以 var React = require('react');的方式引用React.
#### 一. 我们先看看React中有哪些常见的API
1. Components
> React 组件可以让你将应用中UI独立的，可重用的部分拆分出来.可以通过
- React.component
- React.PureComponent
> 来定义Component.

2. Creating React Elements
> 你可以使用React.creactElement()来创建UI,但是官方建议应该是用JSX来描述一个UI组件，JSX只是React.creatcElement的一个语法糖，通过使用JSX,一般情况下你就不用再定义以下方法了：
- createElement();
- createFactory();

3. Transforming Element
React 同时也提供一些其他的API：
- cloneElement()
- isValidElement()
- React.children

#### 二. 接下来分别介绍每一个API定义和用法
- React.Component

----------
> 它是创建一个React组件的基类。使用 es6 语法定义组件：
```
class Greeting extends React.Component{
    render(){
        return <h1>Hello,{this.props.name}</h1>
    }
}
```
你也可以选择不通过es6语法创建一个组件，由于翻译计划的限制，在这里先提供官方链接[use React without es6](https://reactjs.org/docs/react-without-es6.html).

> React.component里面定义了很多方法，我们一个一个了解一下

1. The Component Lifecycle

> 组件都会有几个**生命周期**方法，在执行流程中重写这些方法可以完成特定的逻辑，以**will**开头的方法在特定方法之前被执行，以**did**开头的方法在特定方法之后执行.
- Mounting

> Mounting 相关的方法是在组件创建完成插入到DOM后立即执行的方法.

>constructor(props)\
组件的构造函数在Mounted(即Mounting)之前被调用，当为一个组件实现构造函数时，应该首先执行super(props);否则this.props在使用的时候会提示undefined错误.\
应该避免在构造方法中使用任何会产生副作用或者订阅效果的逻辑，如果有这样的使用场景，应该放在componentDidMount()方法中使用.比如，constructs只是用来初始化组件的状态的[this.state],setState()这种方法就不应该放在constructs中。\
构造函数还经常用于将事件处理程序绑定到类实例。\
更直接一点，假如你既不需要初始化state也不需要绑定事件处理方法，那就把constructs去了吧。
在constructs是一个不错的点子，这样可以有效的分离props和state.
```
constructor(props){
    super(props);
    this.state = {
        color : props.initialColor
    }
}
```
----------------