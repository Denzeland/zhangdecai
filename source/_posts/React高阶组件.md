---
title: React高阶组件
date: 2019-10-20 22:10:48
tags:
- 前端开发
---

### React高阶组件

高阶组件（ higher-order component），类似于高阶函数，它接受 React 组件作为输入，输出一个新的 React 组件。实现高阶组件的方法有如下两种：

- 属性代理（ props proxy） 。高阶组件通过被包裹的 React 组件来操作 props
- 反向继承（ inheritance inversion） 。高阶组件继承于被包裹的 React 组件

下面讲解这两种实现方式：

1. 属性代理

属性代理是常见高阶组件的实现方法，我们通过一个例子来说明：

{% codeblock lang:javascript %}
import React, { Component } from 'React';
const MyContainer = (WrappedComponent) =>
    class extends Component {
        render() {
            return <WrappedComponent {...this.props} />;
        }
    }
{% endcodeblock %}

从这里看到最重要的部分是 render 方法中返回了传入 WrappedComponent 的 React 组件。这样，我们就可以通过高阶组件来传递 props，这种方法即为属性代理。高阶组件可以这样使用：

{% codeblock lang:javascript %}
import React, { Component } from 'React';
class MyComponent extends Component {
// ...
}
export default MyContainer(MyComponent);
{% endcodeblock %}

或者使用装饰器:
{% codeblock lang:javascript %}
import React, { Component } from 'React';

@MyContainer
class MyComponent extends Component {
render() {}
}
export default MyComponent;
{% endcodeblock %}

属性代理可以控制 props, 通过 refs 使用引用, 抽象 state还可以使用其他元素包裹 WrappedComponent

2. 反向继承

从字面意思上看，它一定与继承特性相关。我们同样来看一个简单的实现：
{% codeblock lang:javascript %}
const MyContainer = (WrappedComponent) =>
    class extends WrappedComponent {
        render() {
            return super.render();
        }
    }
{% endcodeblock %}

正如所见，高阶组件返回的组件继承于 WrappedComponent。因为被动地继承了 WrappedComponent，所有的调用都会反向，这也是这种方法的由来。反向继承可以渲染劫持，