> 本章通过精挑细选的案例，精讲 Vue 中的基础知识，包括实例、生命周期、指令、计算属性、方法、侦听器，表单等部分内容。

## 3.1 [Vue实例](https://cn.vuejs.org/v2/guide/instance.html#ad)

> 视频部分

模板中事件的绑定，必须定义在 methods 里面
事件指令简写：v-on:click --> @:click
 
 vue 实例包含
 1. el
 2. data
 3. methods
 4. props
 5. computed
 6. watch
 7. 。。。

每个组件都是一个 Vue 实例，每个 Vue 实例有很多的实例属性和方法

凡是 $ 符号开头的东西，指的都是 Vue 实例属性(实例方法)

1. el ,data 属于 Vue 实例的属性
2. vm.$destory()销毁 Vue 实例(属于 Vue 实例的方法 )

实例销毁后

1. 双向绑定等也就不存在了，
2. 之前绑定的事件在销毁的过程中没有销毁完全，还是遗留在这里的

组件也是 Vue 的实例，而且 Vue 实例有很多属性和方法。

> 官网文档部分

### 3.1.1 [创建一个 Vue 实例](https://cn.vuejs.org/v2/guide/instance.html#%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA-Vue-%E5%AE%9E%E4%BE%8B)

每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 **Vue 实例**开始的：

```
var vm = new Vue({
  // 选项
})
```

虽然没有完全遵循 [MVVM 模型](https://zh.wikipedia.org/wiki/MVVM)，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 `vm` (ViewModel 的缩写) 这个变量名表示 Vue 实例。

当创建一个 Vue 实例时，你可以传入一个**选项对象**。这篇教程主要描述的就是如何使用这些选项来创建你想要的行为。作为参考，你也可以在 [API 文档](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE) 中浏览完整的选项列表。

一个 Vue 应用由一个通过 `new Vue` 创建的**根 Vue 实例**，以及可选的嵌套的、可复用的组件树组成。举个例子，一个 todo 应用的组件树可以是这样的：

```
根实例
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

我们会在稍后的[组件系统](https://cn.vuejs.org/v2/guide/components.html)章节具体展开。不过现在，你只需要明白所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。


### 3.1.2 [数据与方法](https://cn.vuejs.org/v2/guide/instance.html#%E6%95%B0%E6%8D%AE%E4%B8%8E%E6%96%B9%E6%B3%95)

当一个 Vue 实例被创建时，它将 `data` 对象中的所有的属性加入到 Vue 的**响应式系统**中。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```javascript
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的属性
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

当这些数据改变时，视图会进行重渲染。值得注意的是只有当实例被创建时就已经存在于 `data` 中的属性才是**响应式**的。也就是说如果你添加一个新的属性，比如：

```javascript
vm.b = 'hi'
```

那么对 `b` 的改动将不会触发任何视图的更新。如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值。比如：

```javascript
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

这里唯一的例外是使用 `Object.freeze()`，这会阻止修改现有的属性，也意味着响应系统无法再*追踪*变化。

```javascript
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

```javascript
<div id="app">
  <p>{{ foo }}</p>
  <!-- 这里的 `foo` 不会更新！ -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

除了数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 `$`，以便与用户定义的属性区分开来。例如：

```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

以后你可以在 [API 参考](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)中查阅到完整的实例属性和方法的列表。

## 3.2 [Vue实例生命周期](https://cn.vuejs.org/v2/guide/instance.html#%E5%AE%9E%E4%BE%8B%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)

> 视频教程部分

初始化完成 --> beforeCreate --> 外部的注入,双向的绑定 --> create -->

判断是否有 el 选项  --> 是 --> 判断是否有 template 选项 --> 否 --> el 外层的 html 当成模板

beforeMount(即将挂载前) --> 元素挂载到页面之上 --> mounted(页面已经渲染)

组件即将被销毁 --> beforeDestroy --> 组件被完全销毁 --> destroyed

数据改变还没渲染 --> brforeUpdate --> 重新渲染之后 --> updated


无需我们手动调用，Vue 的内部会在某个时刻自动执行这些函数

Vue 的生命周期函数并不放在 methods 对象里面，而是单独的放在 Vue 实例里即可

一共 11 个周期函数，讲解的是其中 8 个，详细讲解 [点这里](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)


> 官网文档部分

### 3.2.1 [实例生命周期钩子](https://cn.vuejs.org/v2/guide/instance.html#%E5%AE%9E%E4%BE%8B%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

比如 [`created`](https://cn.vuejs.org/v2/api/#created) 钩子可以用来在一个实例被创建之后执行代码：

```
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 [`mounted`](https://cn.vuejs.org/v2/api/#mounted)、[`updated`](https://cn.vuejs.org/v2/api/#updated) 和 [`destroyed`](https://cn.vuejs.org/v2/api/#destroyed)。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。

不要在选项属性或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

### 3.2.2 [生命周期图示](https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA)

下图展示了实例的生命周期。你不需要立马弄明白所有的东西，不过随着你的不断学习和使用，它的参考价值会越来越高。

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

> Vue官方文档 "[实例](https://cn.vuejs.org/v2/guide/instance.html)" 部分阅读

> [11 个生命周期函数](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)

## 3.3 [Vue的模版语法](https://cn.vuejs.org/v2/guide/syntax.html)

> 视频教程

1. 插值表达式  {{}}
2. v-text=""    
3. v-html=""

> 官网文档部分

凡是 v-** 指令后都是 js 的表达式 ,不仅仅可以写变量，还可以进行拼接、 js 操作、js 表达式

> 官网教程

> Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，[直接写渲染 (render) 函数](https://cn.vuejs.org/v2/guide/render-function.html)，使用可选的 JSX 语法。


### 3.3.1 插值

#### 文本

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

```javascript
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 `msg` 属性的值。无论何时，绑定的数据对象上 `msg` 属性发生了改变，插值处的内容都会更新。

通过使用 [v-once 指令](https://cn.vuejs.org/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```javascript
<span v-once>这个将不会改变: {{ msg }}</span>
```

#### 原始 HTML

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 [`v-html` 指令](https://cn.vuejs.org/v2/api/#v-html)：

```javascript
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

![原始 HTML](http://oss.xiaodongxier.com/blog/image/20200331113202.png?imageView2/0/interlace/1/q/70|watermark/2/text/eGlhb2Rvbmd4aWVyLmNvbQ==/font/YXJpYWw=/fontsize/600/fill/IzUxQURFRA==/dissolve/100/gravity/SouthEast/dx/10/dy/5)


这个 `span` 的内容将会被替换成为属性值 `rawHtml`，直接作为 HTML——会忽略解析属性值中的数据绑定。注意，你不能使用 `v-html` 来**复合局部模板**，因为 Vue 不是基于字符串的模板引擎。反之，对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位。

> 你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。请只对可信内容使用 HTML 插值，**绝不要**对用户提供的内容使用插值。

#### Attribute

Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)：

```javascript
<div v-bind:id="dynamicId"></div>
```

对于布尔 attribute (它们只要存在就意味着值为 `true`)，`v-bind` 工作起来略有不同，在这个例子中：

```javascript
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` attribute 甚至不会被包含在渲染出来的 `<button>` 元素中。

#### 使用 JavaScript 表达式

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

```javascript
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含**单个表达式**，所以下面的例子都**不会**生效。

```javascript
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

> 模板表达式都被放在沙盒中，只能访问[全局变量的一个白名单](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9)，如 `Math` 和 `Date` 。你不应该在模板表达式中试图访问用户定义的全局变量。

### 3.3.2 指令

> 指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：

```javascript
<p v-if="seen">现在你看到我了</p>
```

这里，`v-if` 指令将根据表达式 `seen` 的值的真假来插入/移除 `<p>` 元素。

#### 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```javascript
<a v-bind:href="url">...</a>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` attribute 与表达式 `url` 的值绑定。

另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

```javascript
<a v-on:click="doSomething">...</a>
```

在这里参数是监听的事件名。我们也会更详细地讨论事件处理。

#### 动态参数

> 2.6.0 新增

从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

```javascript
<!--
注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。
-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 `data` 属性 `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。

同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

```javascript
<a v-on:[eventName]="doSomething"> ... </a>
```

在这个示例中，当 `eventName` 的值为 `"focus"` 时，`v-on:[eventName]` 将等价于 `v-on:focus`。

**对动态参数的值的约束**

动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。

**对动态参数表达式的约束**

动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。例如：

```javascript
<!-- 这会触发一个编译警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。

在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还需要避免使用大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写：

```javascript
<!--
在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
除非在实例中有一个名为“someattr”的 property，否则代码不会工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```

#### 修饰符

修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```javascript
<form v-on:submit.prevent="onSubmit">...</form>
```

在接下来对 [`v-on`](https://cn.vuejs.org/v2/guide/events.html#事件修饰符) 和 [`v-for`](https://cn.vuejs.org/v2/guide/forms.html#修饰符) 等功能的探索中，你会看到修饰符的其它例子。

### 3.2.3 [缩写](https://cn.vuejs.org/v2/guide/syntax.html#%E7%BC%A9%E5%86%99)

> `v-` 前缀作为一种视觉提示，用来识别模板中 Vue 特定的 attribute。当你在使用 Vue.js 为现有标签添加动态行为 (dynamic behavior) 时，`v-` 前缀很有帮助，然而，对于一些频繁用到的指令来说，就会感到使用繁琐。同时，在构建由 Vue 管理所有模板的[单页面应用程序 (SPA - single page application)](https://en.wikipedia.org/wiki/Single-page_application) 时，`v-` 前缀也变得没那么重要了。因此，Vue 为 `v-bind` 和 `v-on` 这两个最常用的指令，提供了特定简写：

#### [`v-bind` 缩写](#v-bind-缩写 "v-bind 缩写")

```javascript
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

#### [`v-on` 缩写](#v-on-缩写 "v-on 缩写")

```javascript
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

它们看起来可能与普通的 HTML 略有不同，但 `:` 与 `@` 对于 attribute 名来说都是合法字符，在所有支持 Vue 的浏览器都能被正确地解析。而且，它们不会出现在最终渲染的标记中。缩写语法是完全可选的，但随着你更深入地了解它们的作用，你会庆幸拥有它们。

## 3.4 [计算属性,方法与侦听器](https://cn.vuejs.org/v2/guide/computed.html)

### 3.4.1 计算属性

> #### 视频教程

> 计算属性 computed 有一个缓存的机制

```html
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>计算属性</title>
</head>
<body>
    <div id="app">
        {{fullName}}
        {{age}}
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data: {
                firstName:"Wang",
                lastName: "Yongjie",
                age: 28
            },
            // 计算属性（缓存机制）
            computed: {
                fullName: function(){
                    console.log("计算了一次")
                    return this.firstName + '' + this.lastName
                }
            }
            
        })
    </script>
</body>
</html>
```
 
初始化代码的时候控制台会打印一次 `console.log("计算了一次")`
控制台进行修改 `age` 的时候 `app.age = "27"`, `copmuted` 中的 `console.log` 是不进行打印的
只有控制台进行修改 `firstName` 或者 `lastName` 的时候，`console.log` 才会进行打印


### 3.4.2 方法

> 方法 methods 内部没有缓存机制

```html
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>计算属性</title>
</head>
<body>
    <div id="app">
        <!-- 方法调用需要加个括号 {{fullName()}} -->
        {{fullName()}}
        {{age}}
    </div>

    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data: {
                firstName:"Wang",
                lastName: "Yongjie",
                age: 28
            },
            methods: {
                fullName: function(){
                    console.log("计算了一次")
                    return this.firstName + '' + this.lastName
                }
            }
        })
    </script>
</body>
</html>
```

初始化代码的时候控制台会打印一次 `console.log("计算了一次")`
控制台进行修改 `age`、`firstName` 或者 `lastName` 的时候，`console.log` 都会进行打印

### 3.4.3 侦听器

> 方法 watch ,类似 computed 存在缓存机制

```html
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>侦听器</title>
</head>
<body>
    <div id="app">
        {{fullName}}
        {{age}}
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data: {
                firstName:"Wang",
                lastName: "Yongjie",
                fullName: "WangYongjie",
                age: 28
            },
            watch: {
                firstName: function(){
                    console.log("计算了一次 1")
                    this.fullName = this.firstName + '' + this.lastName
                },
                lastName: function(){
                    console.log("计算了一次 2")
                    this.fullName = this.firstName + '' + this.lastName
                }
            }
        })
    </script>
</body>
</html>
```
初始化代码的时候控制台不会执行打印
控制台进行修改 `age` 的时候 `app.age = "27"`,`copmuted` 中的 `console.log` 是不进行打印的
只有控制台进行修改 `firstName` 或者 `lastName` 的时候，`console.log` 才会进行打印

> computed > watch > methods
> 如果一个功能可以通过以上的三种方法实现，优先推荐使用 computed 进行实现。
> 因为该方法代码不仅简洁，同时性能又高

> #### 官方文档教程，详细讲解点击 [这里](https://cn.vuejs.org/v2/guide/computed.html)
  
## 3.5 计算属性的 getter 和 setter

fullName 首先会去 data 里面进行寻找，如果找不到再去 computed 里面去找

``` html
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>计算属性的getter和setter</title>
</head>
<body>
    <div id="app">
        {{fullName}}
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data: {
                firstName: "Dell",
                lastName: "Lee"
            },
            // computed: {
            //     fullName: function(){
            //         return this.firstName + ' ' + this.lastName
            //     }
            // },
            // computed 依赖的值发生变化的时候才会重新计算
            computed: {
                fullName: {
                    get: function(){
                        return this.firstName + ' ' + this.lastName
                        console.log("取值的时候执行")
                    },
                    // set 能够接受外部传入的 Value
                    set: function(value){
                        var arr = value.split(" ");
                        this.firstName = arr[0]
                        this.lastName = arr[1]
                        console.log(value)
                        console.log("设置 fullName 值的时候执行")
                    }
                }
            }
        })
    </script>
</body>
</html>
```

通过本节例子理解 computed 计算属性上不仅可以使用 get 的方法通过其他值算出一个新值。

同时还可以使用 set 方法，通过设置一个值，来改变他相关联的值，又会引起 fullName 的重新计算，页面也会跟着变化成新的内容。

## 3.6 Vue中的样式绑定

### 3.6.1 [绑定 HTML Class](https://cn.vuejs.org/v2/guide/class-and-style.html)

#### [1. 对象语法](#对象语法 "对象语法")

我们可以传给 `v-bind:class` 一个对象，以动态地切换 class：

```javascript
<div v-bind:class="{ active: isActive }"></div>
```

上面的语法表示 `active` 这个 class 存在与否将取决于数据属性 `isActive` 的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)。

你可以在对象中传入更多属性来动态切换多个 class。此外，`v-bind:class` 指令也可以与普通的 class 属性共存。当有如下模板：

```javascript
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

和如下 data：

```javascript
data: {
  isActive: true,
  hasError: false
}
```

结果渲染为：

```javascript
<div class="static active"></div>
```

当 `isActive` 或者 `hasError` 变化时，class 列表将相应地更新。例如，如果 `hasError` 的值为 `true`，class 列表将变为 `"static active text-danger"`。

绑定的数据对象不必内联定义在模板里：

```javascript
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

渲染的结果和上面一样。我们也可以在这里绑定一个返回对象的[计算属性](computed.html)。这是一个常用且强大的模式：

```javascript
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

#### [2. 数组语法](#数组语法 "数组语法")

我们可以把一个数组传给 `v-bind:class`，以应用一个 class 列表：

```javascript
<div v-bind:class="[activeClass, errorClass]"></div>
```

```javascript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

渲染为：

```javascript
<div class="active text-danger"></div>
```

如果你也想根据条件切换列表中的 class，可以用三元表达式：

```javascript
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

这样写将始终添加 `errorClass`，但是只有在 `isActive` 是 truthy[\[1\]](#footnote-1) 时才添加 `activeClass`。

不过，当有多个条件 class 时这样写有些繁琐。所以在数组语法中也可以使用对象语法：

```javascript
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

#### [3. 用在组件上](#用在组件上 "用在组件上")

> 这个章节假设你已经对 [Vue 组件](components.html)有一定的了解。当然你也可以先跳过这里，稍后再回过头来看。

当在一个自定义组件上使用 `class` 属性时，这些 class 将被添加到该组件的根元素上面。这个元素上已经存在的 class 不会被覆盖。

例如，如果你声明了这个组件：

```javascript
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

然后在使用它的时候添加一些 class：

```javascript
<my-component class="baz boo"></my-component>
```

HTML 将被渲染为：

```javascript
<p class="foo bar baz boo">Hi</p>
```

对于带数据绑定 class 也同样适用：

```javascript
<my-component v-bind:class="{ active: isActive }"></my-component>
```

当 `isActive` 为 truthy[\[1\]](#footnote-1) 时，HTML 将被渲染成为：

```javascript
<p class="foo bar active">Hi</p>
```

### [3.6.2 绑定内联样式](https://cn.vuejs.org/v2/guide/class-and-style.html#%E7%BB%91%E5%AE%9A%E5%86%85%E8%81%94%E6%A0%B7%E5%BC%8F)

#### [1. 对象语法](#对象语法-1 "对象语法")

`v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名：

```javascript
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```javascript
data: {
  activeColor: 'red',
  fontSize: 30
}
```

直接绑定到一个样式对象通常更好，这会让模板更清晰：

```javascript
<div v-bind:style="styleObject"></div>
```

```javascript
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

同样的，对象语法常常结合返回对象的计算属性使用。

#### [2. 数组语法](#数组语法-1 "数组语法")

`v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上：

```javascript
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

#### [3. 自动添加前缀](#自动添加前缀 "自动添加前缀")

当 `v-bind:style` 使用需要添加[浏览器引擎前缀](https://developer.mozilla.org/zh-CN/docs/Glossary/Vendor_Prefix)的 CSS 属性时，如 `transform`，Vue.js 会自动侦测并添加相应的前缀。

#### [4. 多重值](#多重值 "多重值")

> 2.3.0+

从 2.3.0 起你可以为 `style` 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

```javascript
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 `display: flex`。

> 对象绑定、数组绑定

## 3.7 [Vue中的条件渲染](https://cn.vuejs.org/v2/guide/conditional.html)

### 3.7.1 视频教程笔记

> 视频教程笔记

**v-if 、v-else 、v-show**

v-if=false  DOM节点不存在，每次操作都是删除 DOM 节点和添加 DOM 节点
v-show=false  DOM节点是存在的只是 display:none; 
如果频繁切换显示/隐藏，v-show 性能更高，因为无需频繁添加 DOM 节点
v-if 与 v-else 和 v-else-if 需要紧贴在一起使用, 否则会报错



**关于 key 值**

Vue在重新渲染页面的时候，他会尽力去复用页面已经存在的 DOM，它的机制会尽力复用页面中已经存在的 DOM

当给某个标签加入 key 值得时候，Vue会知道他是页面上唯一的元素，如果两个元素 key 值不一样，Vue 不会尝试去复用（虚拟 DOM 中的一个内容）

加入 key 值能解决其中的 bug



> 官网文档笔记


### 3.7.2 [`v-if`](#v-if "v-if")

[观看本节视频讲解](https://learning.dcloud.io/#/?vid=8 "Vue.js 教程 - 条件渲染")

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

```javascript
<h1 v-if="awesome">Vue is awesome!</h1>
```

也可以用 `v-else` 添加一个“else 块”：

```javascript
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

#### [1. 在 `<template>` 元素上使用 `v-if` 条件渲染分组](#在-lt-template-gt-元素上使用-v-if-条件渲染分组 "在 <template> 元素上使用 v-if 条件渲染分组")

因为 `v-if` 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。最终的渲染结果将不包含 `<template>` 元素。

```javascript
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

#### [2. `v-else`](#v-else "v-else")

你可以使用 `v-else` 指令来表示 `v-if` 的“else 块”：

```javascript
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

`v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

#### [3. `v-else-if`](#v-else-if "v-else-if")

> 2.1.0 新增

`v-else-if`，顾名思义，充当 `v-if` 的“else-if 块”，可以连续使用：

```javascript
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

类似于 `v-else`，`v-else-if` 也必须紧跟在带 `v-if` 或者 `v-else-if` 的元素之后。

#### [4. 用 `key` 管理可复用的元素](#用-key-管理可复用的元素 "用 key 管理可复用的元素")

Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。例如，如果你允许用户在不同的登录方式之间切换：

```javascript
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

那么在上面的代码中切换 `loginType` 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`<input>` 不会被替换掉——仅仅是替换了它的 `placeholder`。

自己动手试一试，在输入框中输入一些文本，然后按下切换按钮：

![key](http://oss.xiaodongxier.com/blog/static/gif/20200401160352.gif?imageView2/0/interlace/1/q/70|watermark/2/text/eGlhb2Rvbmd4aWVyLmNvbQ==/font/YXJpYWw=/fontsize/600/fill/IzUxQURFRA==/dissolve/100/gravity/SouthEast/dx/0/dy/0)


这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 `key` 属性即可：

```javascript
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

现在，每次切换时，输入框都将被重新渲染。请看：

![key](http://oss.xiaodongxier.com/blog/static/gif/20200401160611.gif?imageView2/0/interlace/1/q/70|watermark/2/text/eGlhb2Rvbmd4aWVyLmNvbQ==/font/YXJpYWw=/fontsize/600/fill/IzUxQURFRA==/dissolve/100/gravity/SouthEast/dx/0/dy/0)

注意，`<label>` 元素仍然会被高效地复用，因为它们没有添加 `key` 属性。

### 3.7.3 [`v-show`](#v-show "v-show")

另一个用于根据条件展示元素的选项是 `v-show` 指令。用法大致一样：

```javascript
<h1 v-show="ok">Hello!</h1>
```

不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS 属性 `display`。

注意，`v-show` 不支持 `<template>` 元素，也不支持 `v-else`。

### 3.7.4 [`v-if` vs `v-show`](#v-if-vs-v-show "v-if vs v-show")

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

### 3.7.5 [`v-if` 与 `v-for` 一起使用](#v-if-与-v-for-一起使用 "v-if 与 v-for 一起使用")

**不推荐**同时使用 `v-if` 和 `v-for`。请查阅[风格指南](https://cn.vuejs.org/v2/style-guide/#避免-v-if-和-v-for-用在一起-必要)以获取更多信息。

当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。请查阅[列表渲染指南](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if) 以获取详细信息。

## 3.8 [Vue中的列表渲染](https://cn.vuejs.org/v2/guide/list.html)

**key 值的使用**

在使用 key 值得时候，每个循环项上最好都带一个 key 值，提高性能

key值唯一，并且尽量不要使用它的 index 作为 key 值，能使性能达到最优


**数组变异方法**

> push pop shift unshift splice sore  reverse

直接操作下标形式改变数据，数据变了，但是页面是不会跟着变得
所以如果需要改变数据中的数组数据，那必须通过变异方法来进行修改

```html
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="shortcut icon" href="http://oss.xiaodongxier.com/blog/image/logo.png" type="image/x-icon">
    <title>Vue中的列表渲染</title>
</head>
<body>
    <div id="app">
        <!-- 使用 :key="index" 比 :key="item.id"后端返的 id 性能要高 -->
        <div v-for="(item,index) of list" 
             :key="item.id">
            {{item.text}} --- {{item.id}}
        </div>
    </div>
    <script src="../static/vue/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data: {
                list: [{
                    id: "001",
                    text:"wang"
                },{
                    id: "002",
                    text:"yong"
                },{
                    id: "003",
                    text:"jie"
                }
                ]
            }
        })
    </script>
</body>
</html>
```

以上案例控制台通过 `app.list[0] = {id:"0001",text:"newWang"}` 进行修改数据，页面是无效的

必须通过数组变异方法进行修改 `app.list.splice(1,1,{id:"0001",text:"newWang"})`

**改变引用**























## 3.9 Vue中的set方法




































## 3.10 （新）Vue中的事件绑定




































## 3.11 （新）Vue中的表单绑定




































## 3.12 （新）章节小节











> 