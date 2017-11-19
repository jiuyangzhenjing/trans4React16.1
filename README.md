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

> 1. constructor(props)\
组件的构造函数在Mounted(即Mounting)之前被调用，当为一个组件实现构造函数时，应该首先执行super(props);否则this.props在使用的时候会提示undefined错误.\
应该避免在构造方法中使用任何会产生副作用或者订阅效果的逻辑，如果有这样的使用场景，应该放在componentDidMount()方法中使用.比如，constructs只是用来初始化组件的状态的[this.state],setState()这种方法就不应该放在constructs中。\
构造函数还经常用于将事件处理程序绑定到类实例。\
更直接一点，假如你既不需要初始化state也不需要绑定事件处理方法，那就把constructs去了吧。
在constructs是一个不错的点子，这样可以有效的分离props和state.但是这种模式有一个问题就是，state是不会跟随props的更新而更新的，你需要经常自己更新state.\
注：你可能想到使用componentWillReceiveProps(nextProps)去更新状态，但是还是手动更新的方式更加保险一些。
```
constructor(props){
    super(props);
    this.state = {
        color : props.initialColor
    };
}
```
> 2. componentWillMount()\
componentWillMount在mounting(render)状态之前被调用,因此就算设置了setState()也不会触发页面重新渲染组件，所以通常用constructor替换.\
应该避免在构造方法中使用任何会产生副作用或者订阅效果的逻辑,理由见constructor方法.\
这是唯一一个在服务器端渲染的时候回调的函数，日后必有大用。

> 3. componentDidMount()\
这个函数在一个组件Mounted(render)之后立即执行,要执行和DOM相关的初始化以及网络数据的请求应该放在这里。\
这里是一个设置订阅的好地方，如果你在这里设置了订阅的话，不要忘记在componentWillUnmount()中取消掉订阅。\
在这里可以使用steState触发页面再次渲染。








- Updata
> 4. componentWillReceiveProps()\
componentWillReceiveProps方法只会在组件接收到一个新的props时调用，在组件初始化，渲染以及setState()的时候都不会调用，但是一旦调用此方法，就会触发内部的逻辑更新props,所以有必要随时比较this.props和nextProps,确保数据更改的时候在做更新。

> 5. shouldComponentUpdate(nextProps,nextState)\
shouldComponentUpdate()方法在props或者state改变的时候会立即调用，它返回一个boolean值，是一个默认方法，一般不建议重写此方法。

> 6. componentWillUpdate(nextProps,nextState)\
componentWillUpdate方法在rendering之前调用。但在初始化的时候不会调用这个方法。不应该在这里调用任何可能会触发组件更新的操作。\
如果你真的想要在这个时候更改状态什么的，就去componentWillReceiveProps()更改吧，效果是一样的，同时是安全的做法。当然，如果shouldComponentUpdate()方法返回的是一个false，这个方法是不会执行的。

> 7. componentDidUpdate(preProps,preState),在组件更新完成后立即执行该方法，但是初始化的时候是不会执行的。\
这里也是执行DOM操作和请求网络数据的地方。


> 8. setState(updater,[,callback])\
这是响应事件处理程序和请求服务器数据进行用户界面更新的主要方法。应该将其作为一个“请求”而不是一个立即命令，React会在单个通道中更新几个组件，所以可能会有一定程序延迟.

> 9. forceUpdate(callback)\
此方法可以绕过shouldComponentUpdate(),触发组件re-render(),这种大招平时应该不用为妙。

- Unmount
> 10. componentWillUnmount()\
在组建销毁之前执行，有必要在这里将不必要的定时器，网络请求，或者在componentDidMount()中设置的任何订阅取消掉。

> 11. componentDidCatch(error,info)\
在这里调用setState可以返回一个错误提示页面，告诉你那里有错误，可以使用这个方法进行debug;

- Class Properties
> 12. defaultProps\
能够为class本身定义props,component.defaultProps


- Instance Properties
> 13. props\
定义了这个组件调用者传递的数据\
this.props.children定义的是此组件孩子的props而不是这个标签本身的props.

> 14. state\
state里面定义了这个component的私有数据，由用户自己设定，如果在render中使用不到的数据就不应该存在state中，比如一个定时器.应该假装他是一个只读的属性，永远不要直接赋值，要通过setSteta()去操作。
----------------