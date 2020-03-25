> 本章将快速讲解部分 Vue 基础语法，通过 TodoList 功能的编写，在熟悉基础语法的基础上，扩展解析 MVVM 模式及前端组件化的概念及优势。

## 2.1 课程学习方法

基础部分，看完视频教程，找到官网对应文档进行阅读吸收。

实战部分，根据视频从头到尾把代码敲写出来。


## 2.2 hello world

### [兼容性](https://cn.vuejs.org/v2/guide/installation.html)

Vue **不支持** IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有[兼容 ECMAScript 5 的浏览器](https://caniuse.com/#feat=es5)。

### 开发版和生产版本

1. 开发版包含完整的警告和调试模式
2. 生产版删除了警告(33.30KB min+gzip)，体积更小，加载更快

### hello world 效果的实现

**原生实现方法：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>去哪儿</title>
</head>
<body>
    <div id="app"></div>
    <script>
        window.onload = function(){
            var app = document.getElementById("app");
            app.innerText = "hello word"
        }
    </script>
</body>
</html>
```

**Vue实现方法：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>去哪儿</title>
</head>
<body>
    <!-- {{}} 差值表达式 -->
    <div id="app">{{content}}</div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({ // vue 实例
            el:"#app",      // 实例管理的区域
            data:{          // 定义数据
                content:"hello world"
            }
        })
        // setTimeout(function(){
        //     app.$data.content = "bye world"
        // },2000)
    </script>
</body>
</html>
```

> vue书写代码，无需在把时间放在DOM操作上面，只需关注数据即可。

## 2.3 开发TodoList（v\-model、v\-for、v\-on）

> [案例参考](http://todolist.cn/):http://todolist.cn
> 用到的指令：`v-for`、`v-on`、`v-model` 

### [循环数据：v-for](https://cn.vuejs.org/v2/guide/#%E6%9D%A1%E4%BB%B6%E4%B8%8E%E5%BE%AA%E7%8E%AF)

> v-for 指令可以绑定数组的数据来渲染一个项目列表

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```javascript
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

输出结果：
```
1. 学习 JavaScript
2. 学习 Vue
3. 整个牛项目
```


### [绑定事件：v-on](https://cn.vuejs.org/v2/guide/#%E5%A4%84%E7%90%86%E7%94%A8%E6%88%B7%E8%BE%93%E5%85%A5)

> 处理用户输入
> 为了让用户和你的应用进行交互，我们可以用 v-on 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法：

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反转消息</button>
</div>
```

```javascript
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

点击后输出结果：
```
!sj.euV olleH
```

注意在 `reverseMessage` 方法中，我们更新了应用的状态，但没有触碰 DOM——所有的 DOM 操作都由 Vue 来处理，你编写的代码只需要关注逻辑层面即可。

### [数据双向绑定：v-model](https://cn.vuejs.org/v2/guide/#%E5%A4%84%E7%90%86%E7%94%A8%E6%88%B7%E8%BE%93%E5%85%A5)

> Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。


```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```javascript
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```


















方法定义在 vue 实例的 methods 里面


app.$data.inputValue

MVVM 设计方式



**效果的实现**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>todolist</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model="inputValue">
        <button v-on:click="handleBtnClick">提交</button>
        <ul>
            <!-- 循环data中的list数据，循环的每一项放在item里面 -->
            <li v-for="item in list">{{item}}</li>
        </ul>
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data: {
                list:[],
                inputValue:''
            },
            methods:{
                handleBtnClick: function(){
                     this.list.push(this.inputValue)
                     this.inputValue = ''
                }
            }
        })
    </script>
</body>
</html>
```




