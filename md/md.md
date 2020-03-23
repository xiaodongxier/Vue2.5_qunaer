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



循环数据：v-for
绑定事件：v-on
方法定义在 vue 实例的 methods 里面
数据双向绑定：v-model









