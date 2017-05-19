# vue 入门整理

### 声明式渲染
- 采用简洁的模板语法来声明式的将数据渲染进 DOM
```
<div id="app">
    {{message}}
</div>

<script>
    var data={
        message:"test-one"
     }
   var app = new Vue({
       el:'#app',
       data:data
   })
</script>
```
- 采用这样的方式绑定 DOM 元素属性
```
<div id="app">
    <span v-bind :title="message">
        鼠标悬停几秒钟查看提示信息！
    </span>
    <input v-bind :placeholder="text" :value="aaaa">
</div>
<script>
    var data ={
        message:'test',
        text:'请输入'
    }
    var app=new Vue({
        el:"#app",
        data:data
    })
</script>
```
### 条件与循环
- v-if 控制切换一个元素的显示也相当简单
```
<div id="app-3">
    <p v-if = "seen">zhaoliang</p>
</div>

<script>
    var data = {
        seen:false
    }
    var app = new Vue({
        el:"#app-3",
        data:data
    })
</script>
```
- v-for 指令可以绑定数组的数据来渲染一个项目列表
```
<div id="app">
    <ul>
        <li v-for="x in todos">
            {{x.name}}
            {{x.text}}
        </li>
    </ul>
</div>

<script>
    var data = {
        todos:[
            {name:'eee',text:'study'},
            {name:'syz',text:'ddd'},
            {name:'mnz',text:'ttt'}
        ]
    }
    var app = new Vue({
        el:'#app',
        data:data
    })

</script>
```
### 处理用户输入
- 用 v-on 指令绑定一个事件监听器，通过它调用我们 Vue 实例中定义的方法
```

<div id="app">
    <p>{{message}}</p>
    <button @click="reverseMessage">逆转消息</button>
</div>

<script>
    var data={
        message:"测试数据"
    }
    var method={
        reverseMessage:function () {
            this.message = this.message.split('').reverse().join('')
        }
    }
    var app= new Vue({
        el:"#app",
        data:data,
        methods:method
    })
</script>
```
- Vue 还提供了 v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定
```
<div id="app">
    <p>{{message}}</p>
    <input v-model="message">
</div>

<script>
    var data={
        message: "aaa"
    }
    var app = new Vue({
        el:'#app',
        data:data
    })
</script>
```
### 组件化应用构建
- 子单元通过 props 接口实现了与父单元很好的解耦
```

<div id="app">
    <ol>
        <todo-item v-for="item in list" v-bind :todo="item"></todo-item>
    </ol>
</div>

<script>

    Vue.component('todo-item',{
        props:['todo'],
        template: '<li>{{todo.text}}</li>'
    })

    var data={
        list:[
            {text:'aaa'},
            {text:'bbb'},
            {text:'ccc'}
        ]
    }

    var app=new Vue({
        el:'#app',
        data:data
    })
</script>
```
### vue 构造器
- 每个 Vue.js 应用都是通过构造函数 Vue 创建一个 Vue 的根实例 启动的
```
var vm = new Vue({
  // 选项
})
```

### 属性与方法
- 每个 Vue 实例都会代理其 data 对象里所有的属性
```
var data = { a: 1 }
var vm = new Vue({
  data: data
})
vm.a === data.a // -> true
// 设置属性也会影响到原始数据
vm.a = 2
data.a // -> 2
// ... 反之亦然
data.a = 3
vm.a // -> 3
```
-  Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。
```
var data = {
 a: 1
}
var vm = new Vue({
  el: '#example',
  data: data
})
vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) {
  // 这个回调将在 `vm.a`  改变后调用
})
```
> 注意，不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。

### 实例生命周期
- 每个 Vue 实例在被创建之前都要经过一系列的初始化过程
```
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
// 在实例生命周期的不同阶段调用，如 mounted、 updated 、destroyed 。钩子的 this 指向调用它的 Vue 实例
```
### 模板语法
- 插值

文本
```
<span>Message: {{ msg }}</span>
// 无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。

<span v-once>This will never change: {{ msg }}</span>
// 通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新
```
纯HTML
```
//双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 v-html
 <div v-html="rawHtml"></div>
```
属性
```
Mustache 不能在 HTML 属性中使用，应使用 v-bind 指令：
<div v-bind:id="dynamicId"></div>
这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
<button v-bind:disabled="someDynamicCondition">Button</button>
```
使用JavaScript表达式
```
迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持。
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```
- 指令
> 指令（Directives）是带有 v- 前缀的特殊属性。指令属性的值预期是单一 JavaScript 表达式（除了 v-for，之后再讨论）。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上。
```
<p v-if="seen">Now you see me</p>
这里， v-if 指令将根据表达式 seen 的值的真假来移除/插入 <p> 元素。
```
- 参数
```
一些指令能接受一个“参数”，在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：
<a v-bind:href="url"></a>
在这里 href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。
另一个例子是 v-on 指令，它用于监听 DOM 事件：
<a v-on:click="doSomething">
在这里参数是监听的事件名。我们也会更详细地讨论事件处理。
```
- 缩写
> v- 前缀在模板中是作为一个标示 Vue 特殊属性的明显标识。当你使用 Vue.js 为现有的标记添加动态行为时，它会很有用，但对于一些经常使用的指令来说有点繁琐。同时，当搭建 Vue.js 管理所有模板的 SPA 时，v- 前缀也变得没那么重要了。

v-bind
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```
v-on
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>

```
### 计算属性
```

<div id="app">
    <p>{{message}}</p>
    <p>{{remessage}}</p>
</div>

<script>
    var data={
        message:"test2"
    }
    var getTest={
        remessage:function () {
            return this.message.split('').reverse().join('')
        }
    }
    var app=new Vue({
        el:'#app',
        data:data,
        computed:getTest
    })
</script>


```
### 计算缓存
```
<p>Reversed message: "{{ reversedMessage() }}"</p>
// in component
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
> 我们可以将同一函数定义为一个 method 而不是一个计算属性。对于最终的结果，两种方式确实是相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

