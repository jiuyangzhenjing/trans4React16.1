#这是一个简单的对React官方最新版文档的翻译，可能以后会加上与16.1.1版本之前的版本的对比，现阶段仅限于翻译工作。

>由于本人能力有限，翻译中的不当之处欢迎各位提出建议，我将持续修改这份文档。

#### [快速上手](#fasler)

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