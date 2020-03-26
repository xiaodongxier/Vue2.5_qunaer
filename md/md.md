> 本章将快速讲解部分 Vue 基础语法，通过 TodoList 功能的编写，在熟悉基础语法的基础上，扩展解析 MVVM 模式及前端组件化的概念及优势。

## 2.1 课程学习方法

基础部分，看完视频教程，找到官网对应文档进行阅读吸收。

实战部分，根据视频从头到尾把代码敲写出来。


## 2.2 hello world

### 2.2.1 [兼容性](https://cn.vuejs.org/v2/guide/installation.html)

Vue **不支持** IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有[兼容 ECMAScript 5 的浏览器](https://caniuse.com/#feat=es5)。

### 2.2.2 开发版和生产版本

1. 开发版包含完整的警告和调试模式
2. 生产版删除了警告(33.30KB min+gzip)，体积更小，加载更快

### 2.2.3 hello world 效果的实现

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

### 2.3.1 [循环数据：v-for](https://cn.vuejs.org/v2/guide/#%E6%9D%A1%E4%BB%B6%E4%B8%8E%E5%BE%AA%E7%8E%AF)

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


### 2.3.1 [绑定事件：v-on](https://cn.vuejs.org/v2/guide/#%E5%A4%84%E7%90%86%E7%94%A8%E6%88%B7%E8%BE%93%E5%85%A5)

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

### 2.3.3 [数据双向绑定：v-model](https://cn.vuejs.org/v2/guide/#%E5%A4%84%E7%90%86%E7%94%A8%E6%88%B7%E8%BE%93%E5%85%A5)

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

### 2.3.4 方法定义在 vue 实例的 methods 里面

几个关键字

```
app.$data.inputValue   // 获取 data 中的数据

没有操作DOM，一直在操作数据

MVVM 设计模式
```

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
[预览](https://github.xiaodongxier.com/vue_qunaer/%E7%AB%A0%E8%8A%82%E7%BB%83%E4%B9%A0/2.3.1.Vue%E5%AE%9E%E7%8E%B0todolist%E5%8A%9F%E8%83%BD.html)

## 2.4 MVVM模式


### 2.4.1 MVP

![](http://oss.xiaodongxier.com/blog/image/15851445881334.jpg?imageView2/0/interlace/1/q/70|watermark/2/text/eGlhb2Rvbmd4aWVyLmNvbQ==/font/YXJpYWw=/fontsize/600/fill/IzUxQURFRA==/dissolve/100/gravity/SouthEast/dx/0/dy/0)

> jQuery 面向 DOM 进行开发
> M模型层(ajax请求)  V视图层  P控制器

**MVP todolist**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jQuery todolist</title>
    <script src="../static/jquery/jquery.min.js"></script>
</head>
<body>
    <input type="" id="input">
    <button id="btn">提交</button>
    <ul id="ulElement"></ul>
    <script>
        function Page(){}
        $.extend(Page.prototype,{   // $.extend
            init: function(){
                this.bindEvents()
            },
            bindEvents: function(){
                var btn = $("#btn");
                btn.on("click",$.proxy(this.handleBtnClick,this))   // $.proxy
            },
            handleBtnClick:function(){
                var inputValue = $("#input");
                var ulElement = $("#ulElement");
                ulElement.append(`<li>${inputValue.val()}</li>`)
                inputValue.val("")
            }
        })
        var page = new Page()
        page.init()
    </script>
</body>
</html>
```

### 2.4.2 MVVM

![](http://oss.xiaodongxier.com/blog/image/15851457122101.jpg?imageView2/0/interlace/1/q/70|watermark/2/text/eGlhb2Rvbmd4aWVyLmNvbQ==/font/YXJpYWw=/fontsize/600/fill/IzUxQURFRA==/dissolve/100/gravity/SouthEast/dx/0/dy/0)

> Vue 面向数据进行编程
> M层负责存储数据    V层负责显示数据   VM层(Vue内置的)  没有P层  

**MVVM todolist**

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
        <input type="text" v-model="inputValue" v-on:keyup.enter="enterDown">
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
                },
                enterDown: function(){
                     this.list.push(this.inputValue)
                     this.inputValue = ''
                }
            }
        })
    </script>
</body>
</html>
```

因为无需操作 DOM，Vue 开发要比 jQuery 开发减少 30% 以上的代码量。


### 2.4.3 附加

1. 数据绑定的实现原理
2. 虚拟DOM机制，
3. 相关源码学习


## 2.5 前端组件化

> 一个大型的项目，业务逻辑可能非常的复杂，合理的把页面拆分成一个个组件可以使页面更容易维护

> 每一个组件就是页面上的一个区域

## 2.6 [使用组件化思想修改 TodoList](https://cn.vuejs.org/v2/guide/components.html?#%E7%BB%84%E4%BB%B6%E7%9A%84%E7%BB%84%E7%BB%87)

> 因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

通常一个应用会以一棵嵌套的组件树的形式来组织：

![Component Tree](https://cn.vuejs.org/images/components.png)

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：**全局注册**和**局部注册**。至此，我们的组件都只是通过 `Vue.component` 全局注册的：

```
Vue.component('my-component-name', {
  // ... options ...
})
```

全局注册的组件可以用在其被注册之后的任何 (通过 `new Vue`) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

到目前为止，关于组件注册你需要了解的就这些了，如果你阅读完本页内容并掌握了它的内容，我们会推荐你再回来把[组件注册](https://cn.vuejs.org/v2/guide/components-registration.html)读完。


### 2.6.1 [全局组件使用](https://cn.vuejs.org/v2/guide/components.html#%E5%9F%BA%E6%9C%AC%E7%A4%BA%E4%BE%8B) 


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
            <todo-item v-bind:content="item" v-for="item in list"></todo-item>
        </ul>
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        // 全局组件，可以直接在模板里使用
        Vue.component("TodoItem",{
            props:['content'],
            template:"<li>{{content}}</li>"    // 模板里面用插值表达式
        })
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

### 2.6.2 [数据传递](https://cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87-Prop-%E5%90%91%E5%AD%90%E7%BB%84%E4%BB%B6%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)

> [Prop](https://cn.vuejs.org/v2/guide/components-props.html) 是你可以在组件上注册的一些自定义 attribute。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个属性。
> 一个组件默认可以拥有任意数量的 [prop](https://cn.vuejs.org/v2/guide/components-props.html)，任何值都可以传递给任何 [prop](https://cn.vuejs.org/v2/guide/components-props.html)。
> v-bind -->子组件传入绑定值



### 2.6.3 局部组件使用

[组件注册](https://cn.vuejs.org/v2/guide/components-registration.html)

**全局注册**

到目前为止，我们只用过 `Vue.component` 来创建组件：

```
Vue.component('my-component-name', {
  // ... 选项 ...
})
```

这些组件是**全局注册的**。也就是说它们在注册之后可以用在任何新创建的 Vue 根实例 (`new Vue`) 的模板中。比如：

```
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```

```
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```

在所有子组件中也是如此，也就是说这三个组件*在各自内部*也都可以相互使用。

**局部注册**

全局注册往往是不够理想的。比如，如果你使用一个像 webpack 这样的构建系统，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。

在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：

```
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后在 `components` 选项中定义你想要使用的组件：

```
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

对于 `components` 对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象。

注意**局部注册的组件在其子组件中*不可用***。例如，如果你希望 `ComponentA` 在 `ComponentB` 中可用，则你需要这样写：

```
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```

或者如果你通过 Babel 和 webpack 使用 ES2015 模块，那么代码看起来更像：

```
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```

注意在 ES2015+ 中，在对象中放一个类似 `ComponentA` 的变量名其实是 `ComponentA: ComponentA` 的缩写，即这个变量名同时是：

*   用在模板中的自定义元素的名称
*   包含了这个组件选项的变量名

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
        <input type="text" v-model="inputValue" maxlength="11">
        <button v-on:click="handleBtnClick">提交</button>
        <ul>
            <todo-item 
                v-bind:content="item" 
                v-for="item in list">
            </todo-item>
        </ul>
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        // 局部组件，需要注册才能正常使用
        var TodoItem = {
            props:['content'],
            template:"<li>{{content}}</li>"    // 模板里面用插值表达式
        }
        var app = new Vue({
            el:"#app",
            components: {
                TodoItem:TodoItem
            },
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

## 2.7 简单的组件间传值

> 本节的几个关键字  `index`， `$emit`，`v-bind`

### 2.7.1 父组件向子组件传值

> 通过 v-bind 方式进行数据的传递，
> 父组件可以使用 props 把数据传给子组件。子组件通过 props 进行接收



### 2.7.2 子组件向父组件传值

> 子组件可以使用 $emit 触发父组件的自定义事件。
> 通过事件触发，向上一层触发事件
> 子组件触发的事件，恰好父组件在监听，然后带出子组件传递出来的内容

```javascript
vm.$emit( event, arg )  //触发当前实例上的事件
vm.$on( event, fn );    //监听event事件后运行 fn； 
```

子传父案例（点击删除 list）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>子组件向父组件传值</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model="inputValue">
        <button v-on:click="handleBtnClick">提交</button>
        <ul>
            <todo-item 
                v-bind:content="item" 
                v-bind:index="index"
                v-for="(item,index) in list"
                v-on:delete="handleBtnDelete">
            </todo-item>
        </ul>
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var TodoItem = {
            props:['content','index'],
            template:"<li @click='handleItemClick'>{{content}}</li>",
            methods: {
                handleItemClick: function() {
                    this.$emit("delete",this.index)
                }
            }
        }
        var app = new Vue({
            el:"#app",
            components: {
                TodoItem:TodoItem
            },
            data: {
                list:[],
                inputValue:''
            },
            methods:{
                handleBtnClick: function(){
                     this.list.push(this.inputValue)
                     this.inputValue = ''
                },
                handleBtnDelete: function(index){
                     this.list.splice(index,1)
                    // console.log(idnex) 会报错是为什么呢？
                }
            }
        })
    </script>
</body>
</html> 
```

**疑问：**关于 index 打印报错是为什么呢？alert能正确弹出下标，但是 console.log 却报错是为什么呢？

![](http://oss.xiaodongxier.com/blog/notes/image/15852321164158.jpg?imageView2/0/interlace/1/q/70|watermark/2/text/eGlhb2Rvbmd4aWVyLmNvbQ==/font/YXJpYWw=/fontsize/600/fill/IzUxQURFRA==/dissolve/100/gravity/SouthEast/dx/0/dy/0)


### 2.7.3 v-bind简写

> `v-bind:content="item" ` == `:content="item"`

```javascript
<todo-item v-bind:content="item"></todo-item>
```

等同于

```javascript
<todo-item :content="item"></todo-item>
```

## 2.8 本章小结

### 2.8.1 总结

> v-model  v-bind(简写：':')  v-on(简写：'@')  v-for 

 1. 数据双向绑定
 2. 父子组件传值
 3. todolist


### 2.8.1 补充学习

> 阅读理解官网 [介绍](https://cn.vuejs.org/v2/guide/index.html) 部分内容

**条件**

控制切换一个元素是否显示也相当简单：

```javascript
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
```

```javascript
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

继续在控制台输入 `app3.seen = false`，你会发现之前显示的消息消失了。

这个例子演示了我们不仅可以把数据绑定到 DOM 文本或 attribute，还可以绑定到 DOM **结构**。此外，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用[过渡效果](https://cn.vuejs.org/v2/guide/transitions.html)。








































