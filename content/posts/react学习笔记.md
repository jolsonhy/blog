---
title: React学习笔记
date: 2016-06-11 14:23:41
tags: [React, 学习笔记]
categories: [学习笔记]
---

## 前言
前段时间看了一下React和react-router的文档，这几天用它们在做一个实时投票的Web App，这篇文章记录里面遇到的坑及其解决方法。

## 获取真实DOM节点的引用

添加ref属性
```javascript
ref={(ref) => this.name = ref}
```

Demo:
```javascript
export default class Join extends React.Component {
    constructor(props) {
        super(props);

        this.handleSubmit = (e) => {
            e.preventDefault();
            let memberName = this.name.value;
            console.log("TODO: Join member " + memberName);
        }
    }
    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <label>Full Name</label>
                <input
                    ref={(ref) => this.name = ref}
                    className="form-control"
                    placeholder="Enter your full name..."
                    required />
                <button className="btn btn-primary">Join</button>
            </form>
        );
    }
}
```


## 使用React-Router时父组件向子组件传递props
原本渲染所有路由组件的写法为：
```
{this.props.children}
```

现可表示为：
在父组件的return的最底部添加：
```javascript
{React.cloneElement(this.props.children, {propsName: this.props.name})}
```

## React-Router

### 默认路由
```
<IndexRoute component={Audience} />
```

### 重定向
```html
<Redirect from="audience" to="/" />
```

Demo:
```html
ReactDOM.render((
    <Router history={hashHistory}>
        <Route path="/" component={App}>
            <IndexRoute component={Audience} />
            <Route path="board" component={Board} />
            <Route path="speaker" component={Speaker} />
            <Redirect from="audience" to="/" />
            <Route path="*" component={NotFound} />
        </Route>
    </Router>
), document.getElementById('container'));
```