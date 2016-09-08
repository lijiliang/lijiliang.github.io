---
title: React实践记录
categories:
    - react
tags: [react]
---

## React写法用: ES5(createClass)还是ES6(class)？

React可以用ES5或ES6完美书写，使用JSX意味着你将通过Babel将JSX通过“build”转化为`React.createElement`调用。很多人利用这个特点在Babel的编译列表中添加一些es2015的语法以此来让ES6完全可用。

### 比较createClass和class

以下是使用`React.createClass`和ES6的`class`书写相同的组件：

```js
var inputControlES5 = React.creatClass({
    getInitialState: function(){
        return{
            text: this.props.initialValue || 'placeholder'
        };
    },
    handleChange: function(event){
        this.setState({
            text: event.target.value
        })
    },
    return : function(){
        return(
                <div>
                    Type something:
                    <input onChange={this.handleChange}
                      value={this.state.text} />
                </div>
            );
        )
    }
})
```

```js
class inputControlES6 extends React.Component{
    constructor(props){
        super(props);

        this.state = {
            text: this.props.initialValue || 'placeholder'
        }

        // es6函数要手动绑定
        this.handleChange = this.handleChange.bind(this);
    }
    handleChange(event){
        this.setState({
            text: event.target.value
        })
    }
    render(){
        return(
            <div>
                Type something:
                <input onChange={this.handleChange}
                  value={this.state.text} />
            </div>
        )
    }
}
```

### 比较两种写法的不同点

#### 函数的绑定

使用`createClass`,每个函数属性都会被React自动绑定，指的是`this.eventFn`这样的函数在任何你需要调用的时候都会自动为你绑定正确的`this`。

在ES6的`class`中，就不同了：函数需要我们去手动绑定它们

另一种方式就是在你使用的地方通过内联来绑定
```js
// .bind
render(){
    return(
        <input onChange={this.handleChange.bind(this)}
        value={this.state.text}/>
    )
}

// --- Or ---
render(){
    return(
        <input onChange={() => this.handleChange()}
        value={this.state.text}/>
    )
}

```

#### 构造函数应该调用super

es6类的constructor需要接收`props`并且调用`super(props)`。这是`createClass`所没有的一点

#### class和createClass的比较

这个很明显，一个调用`React.createClass`并传入一个对象，另一个则是使用class继承`React.Component`。

小提示： 导入Component时如果在一个文件里包含多个组件，可以直接通过以下方式节省一些代码量：

```js
import React, {Component} from 'react'
```

#### 初始化State的设置

`createClass` 接受一个只会在组件被挂载时才会调用一次的`initialState`函数。

ES6 的`class`则使用构造函数。在调用`super`之后，可以直接设置state。


## JSX中的if-else

你没法在JSX中使用 if-else 语句，因为 JSX 只是函数调用和对象创建的语法糖

### 用三元操作表达式

```js
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name ？ this.props.name : "World"}</div>;
  }
});
ReactDOM.render(<HelloMessage name="xiaowang" />, document.body);
```

### 用比较运算符“ || ”

```js
var HelloMessage = React.createClass({
  render: function() {
      return <div>Hello {this.props.name || "World"}</div>;
  }
});
ReactDOM.render(<HelloMessage name="xiaowang" />, document.body);
```

### 用变量来书写

```js
var loginButton;
if (loggedIn) {
  loginButton = <LogoutButton />;
} else {
  loginButton = <LoginButton />;
}

return (
  <nav>
    <Home />
    {loginButton}
  </nav>
)
```

### 内联判断，用立即调用函数表达式

```js
// ES5写法  函数的this调用需要重置this
{
    (function(){
        if(this.state.color){
            return(
                <div>this.state.color</div>
            )
        }
    })()
}

//ES6写法
{
    (()=>{
        if(this.state.color){
            return(
                <div>this.state.color</div>
            )
        }
    })()
}
```
